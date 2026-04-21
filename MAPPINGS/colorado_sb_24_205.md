# Colorado AI Act (SB 24-205) — Earned Autonomy Mapping

**Statute:** Colorado Senate Bill 24-205, the Colorado Artificial Intelligence Act
**Signed:** May 17, 2024 (Governor Jared Polis)
**Effective:** February 1, 2026 (rebuttable presumption clock started)
**Enforcement:** June 30, 2026 (Attorney General enforcement authority)
**Codification:** Colorado Revised Statutes §§ 6-1-1701 through 6-1-1707

---

## Who this applies to

The Colorado AI Act regulates two roles:

- **Developers** — entities that develop, or intentionally and substantially modify, a high-risk artificial intelligence system.
- **Deployers** — entities that deploy a high-risk AI system in Colorado.

A **high-risk AI system** is defined as any AI system that, when deployed, makes or is a substantial factor in making a **consequential decision** — a decision that has a material legal or similarly significant effect on a consumer's access to, or the cost, terms, or availability of:

- Education or employment
- Financial or lending services
- Healthcare services
- Housing
- Insurance
- Legal services
- Government services

If your AI touches any of the above, and you operate in Colorado or serve Colorado consumers, you are almost certainly in scope as a deployer.

---

## The four deployer obligations

Per C.R.S. § 6-1-1703, a deployer of a high-risk AI system must:

**1. Notice to consumers** — Notify the consumer when a high-risk AI system is making, or is a substantial factor in making, a consequential decision about them. The notice must be provided before the decision and must describe the system in plain language.

**2. Right to correct data** — Provide consumers a reasonable opportunity to correct any incorrect personal data that the high-risk AI system processed in making the consequential decision.

**3. Right to appeal through human review** — Provide consumers a reasonable opportunity to appeal an adverse consequential decision through human review.

**4. Reasonable care against algorithmic discrimination** — Use reasonable care to protect consumers from known or reasonably foreseeable risks of algorithmic discrimination arising from the intended and contracted uses of the high-risk AI system.

---

## Earned Autonomy mapping

| Colorado deployer obligation | Earned Autonomy mechanism | Where it lives in the protocol |
|---|---|---|
| **1. Notice to consumers** | Every decision touching a consumer is logged with a timestamp, the responsible operator, the data considered, and the outcome. The consumer receives a plain-language notice of AI involvement before the consequential decision is finalized. Transparency is the default posture, not an opt-in feature. | EAI Law 7 (Accountability Exists Outside the Model) · Decision Log (README, Key Concepts) · Discipline 12 (Commits Are The Notebook — provable history) |
| **2. Right to correct data** | The customer collection flow is built for correction. Per-entity capture, visible state at every step, and an offline escape hatch when the system cannot accept the data. Every field the AI considered is surfaced to the consumer in reviewable form. | Discipline 03 (IPS — Idiot-Proof Standard: make the right action obvious) · EAI Law 5 (Reversibility Is Mandatory) |
| **3. Right to appeal through human review** | Every consequential decision in the reference implementation has a named human approver. The AI classifies and drafts. A human being decides. No exceptions. When a consumer appeals, the appeal is routed to the named approver, not re-processed by the same system. | EAI Law 2 (No Action Without Permission) · EAI Law 6 (Silence Is Success) · Decision Layers (README, Key Concepts — Probabilistic and Judgment tiers require human approval) |
| **4. Reasonable care against algorithmic discrimination** | Default to non-execution on unknown or ambiguous state. If the system is not sure, it stops. If a tired operator could cause harm, a structural guard is added. The Trust Genesis Event pattern forces live validation before any new domain is given autonomy, with logged evidence of correct behavior across representative cases before scope expands. | Earned Autonomy principle (trust as a zero-balance account) · EAI Law 3 (Observation Is a Valid State) · EAI Law 4 (Probabilistic Reasoning Requires Deterministic Control) · Trust Genesis Event (README, Key Concepts) |

Four obligations. Four mechanisms. All four were built before the Colorado AI Act was enforceable, because they are the disciplines a small AI practice needs in order to be trustworthy at all.

---

## What deployers should note

- **The rebuttable presumption clock started February 1, 2026.** If you can document reasonable care starting from that date, you are in a materially better position when enforcement begins June 30, 2026.
- **Risk management program is required.** The statute requires deployers to implement and maintain a risk management program. The Earned Autonomy Protocol constitutes such a program when implemented faithfully.
- **Impact assessments are required annually** and upon any intentional and substantial modification. Use the Decision Log as the primary evidence source.
- **No private right of action.** Enforcement is by the Colorado Attorney General under the Colorado Consumer Protection Act.
- **Safe harbor exists for deployers** who comply with recognized AI risk management frameworks (NIST AI RMF, ISO/IEC 42001). The Earned Autonomy Protocol is consistent with both. See the companion methodology for the ISO 42001 alignment note.

---

## Related US state statutes (same framework, same mapping)

| Jurisdiction | Statute | Status | Mapping file |
|---|---|---|---|
| Colorado | SB 24-205 | Effective Feb 1, 2026 · enforcement Jun 30, 2026 | This document |
| Utah | SB 149 (Utah Artificial Intelligence Policy Act) | Effective May 1, 2024 · applies to regulated occupations including tax preparation | Coming soon |
| Texas | HB 149 (TRAIGA, Texas Responsible AI Governance Act) | Effective Jan 1, 2026 | Coming soon |
| California | AB 2013 | Effective Jan 1, 2026 (training data transparency) | Coming soon |

The Earned Autonomy Protocol is not a Colorado-specific framework. The same four mechanisms map against Utah's occupational rules, Texas's TRAIGA, and California's training-data transparency requirement. When a new jurisdiction publishes a regulatory matrix, the mapping work is incremental, not structural.

---

## References

- Colorado SB 24-205 — full text: https://leg.colorado.gov/bills/sb24-205
- Colorado Revised Statutes §§ 6-1-1701 through 6-1-1707
- Earned Autonomy Protocol — specification: https://github.com/LatamlinkLLC/earned-autonomy-protocol
- White Paper V2 — "Making AI Decisions Worth Automating": https://intake.sentinelos.us/earned-autonomy
- SSRN preprint: https://papers.ssrn.com/sol3/papers.cfm?abstract_id=6576486
- Implementation notes: https://github.com/LatamlinkLLC/earned-autonomy-protocol/blob/main/IMPLEMENTATION_NOTES.md

---

*Latamlink LLC · nicolas@latamlinkus.com · Mapping published 2026-04-21 · CC BY 4.0*
