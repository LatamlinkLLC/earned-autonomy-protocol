# The Earned Autonomy Protocol

**A governance framework for AI systems operating in consequential domains.**

Intelligence is capability. A framework makes it a trusted partner.

---

## What This Is

The Earned Autonomy Protocol is a practitioner-built governance methodology for deploying AI in regulated, high-stakes business processes. It was developed by [Latamlink LLC](https://intake.sentinelos.us/earned-autonomy) beginning in December 2025 and has been in live production operation since early 2026.

The core principle: **AI does not start with autonomy. It earns it incrementally through demonstrated reliability in bounded decisions, logged and verified before scope expands.**

This is the opposite of how most AI deployments work. The default pattern is: deploy the model, give it broad access, hope it performs well, add constraints when it fails. That pattern treats trust as a default state revoked on failure. The Earned Autonomy Protocol treats trust as a zero-balance account funded through evidence.

## Chronology

| Date | Event |
|---|---|
| December 13, 2025 | SentinelOS doctrine created: execution autonomy prohibited by default |
| December 31, 2025 | Governance formalized: Trust Composite Score (TCS), Go-Live Indicator (GLI), Externally Accountable Intelligence (EAI) |
| February 2, 2026 | Cloud Security Alliance publishes Agentic Trust Framework (independent convergence) |
| April 2026 | Anthropic Mythos sandbox escape validates the governance thesis |
| April 2026 | White paper published: "Making AI Decisions Worth Automating" |
| May 1, 2026 | White paper formally citable on SSRN |
| May 27, 2026 | First multi-path Trust Genesis ceremony: 10 decision paths evidence-locked with Trust Composite Scores |
| June 11, 2026 | Discipline 13 authored: verification rituals made model-independent |
| July 1, 2026 | Reconciliation proof-of-life in the reference implementation: 20 of 20 account-months verified to the cent, hash-locked |
| July 16, 2026 | Supervised-operator doctrine ruled: perception loop automated, action loop human-gated |
| July 2026 | Match/Mismatch podcast live (episodes 1–3): the protocol's public reflection layer |
| July 28, 2026 | Doctrine update v1.1: the Two Loops, constitutional vs. earned boundaries, the Thirteen Disciplines published |

## Key Concepts

### Earned Autonomy
Trust is built through demonstrated performance, not assumed at deployment. An AI agent begins in observation-only mode. Execution authority is granted incrementally, per domain, based on logged evidence of correct behavior. Every grant is reversible.

### Trust Genesis Event
The first live autonomous action an AI agent takes in a domain, with human verification of the result. Not a test. A real action on real data, independently validated. Every new domain requires its own genesis event.

### Decision Layers
Not all decisions require the same governance:
- **Deterministic**: rule-based, binary. AI executes autonomously.
- **Probabilistic**: AI informs, human approves.
- **Judgment**: AI flags, human decides with full deliberation.

### Decision Log
The audit trail that makes autonomy defensible. A decision-specific record: what was decided, by whom, what evidence was considered, what the outcome was.

### The Perception–Action Split
Not "automate everything" and not "gate everything" — split by loop. The **perception loop** (classify, reconcile, verify, flag) runs autonomously and continuously: it only produces claims, and claims are checkable. The **action loop** (move money, file with a government, contact a client, write to the book of record) binds the outside world and stays human-gated. Prove perception before wiring action. *(Implementation Notes, Note 5.)*

### The Two Loops
The task loop asks *"is the work right?"* The judgment loop asks *"is the deciding right — and who should hold which decision?"* Verified tasks generate evidence; the judgment loop spends that evidence to recalibrate standards and move the autonomy boundary. Nested, not parallel: an implementation that runs only the task loop has static autonomy; one that runs only the judgment loop grants scope on vibes. Lineage: double-loop learning (Argyris, 1977). *(Note 7.)*

### Constitutional vs. Earned Boundaries
Earned boundaries move on evidence. Constitutional invariants — money movement, government filings, unprompted client contact, credential entry — are not on the performance axis at all: they change only by explicit human ceremony, stated in plain words and logged, never as a side effect of a good streak. *(Note 6.)*

## The Seven EAI Laws

The protocol operates under Externally Accountable Intelligence (EAI), built on seven non-negotiable laws:

1. **External Authority Supremacy.** Authority resides outside the intelligence. AI cannot grant itself permission.
2. **No Action Without Permission.** Execution requires explicit authorization. Default state is observation only.
3. **Observation Is a Valid State.** Non-execution is not failure. Trust is earned through demonstrated constraint.
4. **Probabilistic Reasoning Requires Deterministic Control.** AI may reason probabilistically, but control structures must be deterministic.
5. **Reversibility Is Mandatory.** All actions must be reversible or constrained to prevent irreversible harm.
6. **Silence Is Success.** No action is preferable to uncertain action.
7. **Accountability Exists Outside the Model.** Responsibility cannot be delegated to AI. Humans own outcomes.

## The Thirteen Disciplines

The operational layer beneath the laws — the habits the reference implementation runs every session. Disciplines 10–13 were added after this repo's first publication; the Implementation Notes carry the incidents that produced them.

1. **Atomic Verification** — walk every boundary crossing; never declare "done" from the happy story.
2. **Canary Pattern** — plant known-answer checks that scream when the system drifts.
3. **Idiot-Proof Standard** — build so the tired operator cannot misuse it.
4. **We Handle, Not Panic** — composure is part of the control system.
5. **Three Places Mantra** — a fact is not durable until it lives in three independent places.
6. **EAI Core Laws** — the seven laws above, held as a daily discipline.
7. **Inspect Before Propose** — read the artifact before proposing changes to it.
8. **Ground Before Synthesize** — cite the live source in the same turn as the claim.
9. **Write As You Go** — persist state when it changes, not at session close *(Note 2)*.
10. **Narrative Coherence Is A Red Flag** — smooth prose about system state means nothing was checked; if claims outnumber fresh lookups, you synthesized instead of grounded.
11. **Reach Via The Wired System** — the agent's reach is the system's credentials and connectors, not one chat's tool list; "I don't have access" is usually a claim about the wrong layer.
12. **Commits Are The Notebook** — the log is the only medium that cannot be lost; every commit is written for a zero-context successor (what, why, where-else, next).
13. **Verification Rituals** — model-independent: a weaker model following a strong ritual beats a strong model improvising *(Note 4)*.

## Independent Convergence

The Cloud Security Alliance's Agentic Trust Framework, published February 2, 2026, independently arrived at the same core principle: "agents earn autonomy through demonstrated trustworthiness."

The Earned Autonomy Protocol was operational seven weeks before CSA published. We did not cite CSA. They did not cite us. When practitioners and standards bodies arrive at the same principle independently, the principle is probably correct.

## Regulatory Alignment

The Earned Autonomy Protocol maps natively against AI governance statutes in the jurisdictions where the reference implementation operates. The same four mechanisms meet the obligations in each framework; the mapping work is incremental, not structural.

| Jurisdiction | Statute | Enforcement | Mapping |
|---|---|---|---|
| European Union | AI Act Article 26 (Regulation 2024/1689) | August 2, 2026 | [White Paper V2](https://intake.sentinelos.us/earned-autonomy) |
| Colorado, USA | SB 24-205 (Colorado AI Act) | June 30, 2026 | [MAPPINGS/colorado_sb_24_205.md](./MAPPINGS/colorado_sb_24_205.md) |
| Utah, USA | SB 149 (Artificial Intelligence Policy Act) | In effect since May 1, 2024 | *Coming soon* |
| Texas, USA | HB 149 (TRAIGA) | January 1, 2026 | *Coming soon* |
| California, USA | AB 2013 (training data transparency) | January 1, 2026 | *Coming soon* |

Additional jurisdictions are added as the regulatory matrix expands. Contributions and corrections welcome via issue or PR.

## White Paper

The full practitioner's account is available at:
**[Making AI Decisions Worth Automating: The Earned Autonomy Protocol](https://intake.sentinelos.us/earned-autonomy)**

By Nicolas Aillon, Latamlink LLC. Co-authored with Aila (Claude Opus 4.6, Anthropic).

## License

This specification is released under [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).

You are free to share, adapt, and build upon this work, provided you give appropriate credit to Latamlink LLC and link to the original.

## About

**Latamlink LLC** is a bilingual consultancy and technology practice serving U.S. and LATAM-market operators in regulated industries. The Earned Autonomy Protocol was built from the experience of deploying AI in U.S. tax services, where disciplined AI makes your operations trustworthy, enhances compliance, and gives your customers peace of mind.

**Aila** is the governed AI partner operating under this protocol. Finnish for "bearer of light." Derived from Aillon (AI-llon), the founder's surname.

**SentinelOS** is the operating methodology that houses the protocol.

---

*Latamlink LLC | nicolas@latamlinkus.com | [sentinelos.us](https://intake.sentinelos.us)*
