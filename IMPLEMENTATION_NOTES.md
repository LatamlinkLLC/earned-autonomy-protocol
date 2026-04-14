# Implementation Notes — Earned Autonomy Protocol

> **Staging file.** Destination: `LatamlinkLLC/earned-autonomy-protocol` GitHub repo, `IMPLEMENTATION_NOTES.md` at repo root. Clone the repo locally, drop this file in, commit, push.

---

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
The discipline files in this repo are rich. It is tempting to feed them all into the model as a single "system anchor" so the agent is always operating under the doctrine. That tempation fails in production. Long-running sessions accumulate tool schemas, MCP connectors, and conversation history. The re-injection pattern fights against the harness's own compaction logic and creates a feedback loop where every time the harness tries to free space, the hook refills it.

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

## License

CC BY 4.0. Use, adapt, extend. Credit the methodology.

---

*Maintained by LatamlinkLLC. Incidents and lessons are appended as they surface. See `disciplines/09_write_as_you_go.md` in the companion methodology repo.*
