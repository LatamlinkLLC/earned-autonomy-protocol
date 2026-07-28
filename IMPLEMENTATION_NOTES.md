# Implementation Notes — Earned Autonomy Protocol

## About this document

The Earned Autonomy Protocol (EAP) is the methodology. This document is not part of the methodology. It is a set of **operational notes from the reference implementation** — the real system (LatamlinkLLC / SentinelOS) where EAP was developed and is in production. These notes capture lessons learned that affect how anyone implementing EAP should structure their own tooling.

Posted under the same CC BY 4.0 license as the rest of the repo.

---

## Note 1 — Load doctrine by pointer, not by embed

**Lesson date:** 2026-04-14
**Incident:** Complete loss of an 8-hour Claude Code session mid-work.
**Root cause:** Doctrine re-injection hook embedded the full methodology (11KB main file + 8 discipline files, ~55KB total) into every LLM system prompt on every session start and every conversation compaction. The API provider's request-size ceiling (20MB on Anthropic at time of writing) was reached after enough tool calls and tool schemas accumulated. Once the limit was hit, no message of any size could be sent. The session was unrecoverable.

**Fix:** Replace the embed with a **pointer block**. Emit discipline titles, one-line operating rules, and file paths — let the agent read the full file on demand when the task needs it.

**Why this matters for anyone implementing EAP:**
The discipline files in this repo are rich. It is tempting to feed them all into the model as a single "system anchor" so the agent is always operating under the doctrine. That temptation fails in production. Long-running sessions accumulate tool schemas, MCP connectors, and conversation history. The re-injection pattern fights against the harness's own compaction logic and creates a feedback loop where every time the harness tries to free space, the hook refills it.

**Recommended pattern:**

```
# Session anchor (output at start only, ~1-2KB target)

You are operating under the Earned Autonomy Protocol.

## Disciplines (read the file on demand)
01 Atomic Verification
02 Canary Pattern
03 IPS (Idiot-Proof Standard)
04 We Handle, Not Panic
05 Three Places Mantra
06 EAI Core Laws
07 Inspect Before Propose
08 Ground Before Synthesize
09 Write As You Go, Not At The End
10 Narrative Coherence Is A Red Flag
11 Reach Via The Wired System
12 Commits Are The Notebook
13 Verification Rituals (Model-Independent)

## Source of truth
- Disciplines directory: <your repo path>/disciplines/
- Core laws: <your repo path>/EAI_CORE_LAWS.md

## Operating rules
- Inspect before propose (07). Read the file, do not guess.
- Ground before synthesize (08). Cite the source in the same turn.
- Write as you go, not at the end (09). Persist state continuously.
```

**Do not hook this into PreCompact events.** Compaction is the harness freeing context space. Re-injecting on compaction is exactly the feedback loop that killed our session. SessionStart only.

---

## Note 2 — Persistence is a discipline, not a ritual

**Lesson date:** 2026-04-14 (same incident)
**Observation:** The session that died had built 7 major assets over multiple days (white paper, landing page, public GitHub repo, SSRN submission, three LinkedIn posts). The assets survived because they were files on disk, in git, on SSRN, on LinkedIn — external, durable. The **reasoning chain** that produced them (naming brainstorm, structural decisions, trade-offs considered and rejected) did not survive because it lived only in the chat buffer.

No sync log entry had been written in four days. Persistence was planned as a session-close ritual. By the time the discipline fired, the window was already dead.

**Fix — Discipline 09: Write As You Go, Not At The End.**

State changes get written to files **the moment they happen**, not at session close:
- A decision is made → append to sync log
- An asset is created → append to sync log
- A thread moves state → edit the state file in place
- A constraint is discovered → append to lessons log

Session close becomes a **diff** operation (what moved, what's blocked, what's new since start), not a dump operation (remembering everything that happened). If you are remembering things at session close, you already failed discipline 09.

**Why this matters for EAP:**
The protocol treats trust as something AI earns through demonstrated reliability. That requires a **provable history**. A provable history requires durable state. Durable state requires continuous persistence. If your implementation batches persistence, you are risking trust in a single failure point.

---

## Note 3 — Parallel implementation surfaces need parallel fixes

**Observation:** The same anti-pattern (doctrine embed) existed in two places in the reference implementation:
1. The operator's chat hook (`sentinelos_doctrine_inject.sh`)
2. The agent backend's system prompt builder (`decision_connector.py`)

Fixing only one would have left the other as a future context bomb. Single canonical source, single fix pattern, applied everywhere.

**Why this matters:** If you build an EAP-governed system with both an interactive operator surface and an autonomous agent backend, audit both for the same anti-patterns. They will drift independently if you only patch one.

---

## Note 4 — Verification rituals must be model-independent

**Lesson date:** 2026-06-11
**Observation:** A two-day session on the strongest model we had operated to date produced an unusual density of catches: a $2 delta between two independent systems that decomposed into three engine biases touching an entire cohort; a false "system is frozen" verdict traced to reading document headers instead of `git log`; parked customer drafts that contradicted already-filed reality. None of the catches came from raw capability. All of them came from a small set of repeated behaviors the model exhibited unprompted.

**Fix — Discipline 13: Verification Rituals (Model-Independent).** Convert the behaviors from instinct into procedure, so the quality survives any model change:

1. **The two-system rule.** No number gets frozen — report cell, customer email, headline — until it is checked against an independent system. Every delta gets named and explained, however small. The magnitude of a delta says nothing about the magnitude of its cause.
2. **Live source before verdict.** Claims about state require the live artifact: `git log`, not doc headers; the deployed build, not the repo copy; the actual mailbox, not the assumption of sending. Know the topology of your evidence — one account's view is partial evidence, never a verdict.
3. **Decision memo shape.** Every fork is presented as: empirical tests first (run them, don't reason about them), costs stated honestly in both directions, one recommendation, and **riders** — verification steps that travel with the approval and execute as part of it. An approval without riders is a hope.
4. **Drafts expire.** Any parked artifact — email draft, post, migration, spec — is expired the moment reality changes after its creation date. Re-check against current state before firing. Parked is not preserved; parked is decaying.
5. **Mistakes get banked with lookup keys.** Every error becomes a memory entry containing the lookup key that would have prevented it. The test of a good banked lesson: the next agent pays a lookup, not a rediscovery.
6. **Operator prompts as quality forcing.** The operator's questions are part of the quality system: "What live source did you check?" "Does the row sum?" "Where does this number come from?" An agent that cannot answer the first question has not earned the verdict it is offering.

**Why this matters for EAP:** trust that is portable across models is the whole point of governance. **A weaker model following a strong ritual beats a strong model improvising.** If your autonomy grants depend on a specific model's instincts, you granted autonomy to a vendor's weights, not to your system.

---

## Note 5 — The perception–action split

**Lesson date:** 2026-07-16
**Observation:** "Automate everything" and "gate everything" are both wrong, and the useful boundary is not by task type but by loop. The **perception loop** (classify, reconcile, verify, flag) can run autonomously and continuously: it only produces claims, and claims are checkable against canaries. The **action loop** (move money, file with a government, contact a client, write to the book of record) binds the outside world, and that is where harm lives.

**Doctrine:** automate the perception loop to canary-green; keep the action loop human-gated. **Prove perception before wiring action.** The agent operates, the human approves. In the reference implementation, reconciliation, extraction, and analysis run lights-out, while every send, filing, payment, and ledger write waits for a human click.

**Why this matters for EAP:** this is the deployment *order* for the Decision Layers. Autonomy expands through perception first because perception failures are cheap to catch (a wrong claim meets a canary) while action failures are expensive to reverse. An implementation that automates action before perception has the protocol backwards.

---

## Note 6 — Constitutional vs. earned boundaries

**Lesson date:** 2026-07-22
**Observation:** A pure evidence loop eventually argues against its own gates: "error rate has been zero for six months — remove the human gate." If all boundaries are earned, all boundaries are negotiable, and a good streak becomes an argument for removing the controls that produced the streak.

**Doctrine:** two kinds of boundaries. **Earned boundaries** move on evidence: what the agent may drive, draft, reconcile, generate. **Constitutional invariants** are not on the performance axis at all: moving money, filing legal or tax documents, unprompted client contact, entering credentials. These change only by explicit human ceremony — the change stated in plain words, logged, never bundled inside another approval — and never as a side effect of good performance.

**Why this matters for EAP:** Law 5 (reversibility) and Law 7 (accountability outside the model) imply that some gates exist because of what the action *is*, not how well the agent performs it. Write the invariant list down before the streak starts. Otherwise the streak will write it for you.

---

## Note 7 — The two loops: task correctness vs. judgment calibration

**Lesson date:** 2026-07-28
**Observation:** In a public exchange about applying Auftragstaktik (Commander's Intent) to AI agents, a distinction we had been living without naming became visible. Intent-decompression skills, back-briefs, iteration loops — these optimize the **task loop**: is this work aimed right, converged, logged? Necessary, and not sufficient.

**Doctrine:** the Earned Autonomy Protocol runs one loop up — the **judgment loop**. Verified task outcomes are consumed as evidence to recalibrate the decision system itself: which standards the agent applies, which operator corrections persist (feedback stored as calibration records with the *why* and the *how to apply*, not as chat scrollback), and where the earned boundary moves next. The task loop asks "is the work right?" The judgment loop asks "is the deciding right — and who should hold which decision?"

The loops are nested, not parallel. Task correctness generates the evidence; judgment calibration spends it. Established lineage: single-loop vs. double-loop learning (Argyris, 1977). Mission command carries the same pair: the drill, and the after-action review.

**Why this matters for EAP:** run only the task loop and your autonomy is static — the agent does the same trusted things forever. Run only the judgment loop and your autonomy is unearned — scope expands on vibes. Earned autonomy is precisely the composition: the inner loop proves, the outer loop grants, the constitution bounds (Note 6).

---

## License

CC BY 4.0. Use, adapt, extend. Credit the methodology.

---

*Maintained by LatamlinkLLC. Incidents and lessons are appended as they surface. See `disciplines/09_write_as_you_go.md` in the companion methodology repo.*
