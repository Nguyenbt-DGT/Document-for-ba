# Story Splitting Decision — Family Quick Pitch Multi-Persona Enhancement

> Output of the team discussion (PO + BA + SA + QA + Scrum Master) required by the Mission workflow,
> Steps 1–4, before any User Story was written. Source requirement: `user-story-family-quick-pitch.html`.
> Story template standard: `MPA_User_Story_Templates.md`. Agent guidelines used: `.kombai/Agent_Document/PO.md`,
> `.kombai/Agent_Document/BA.md` (SA.md and QA.md guideline files do not yet exist in this repo; the SA and
> QA perspectives below were produced directly from general Solution Architecture / QA practice applied to
> the domain facts already established in PO.md/BA.md).

---

## STEP 1 — PO: Business Framing

**Business Goal**
Let a Manulife agent (FA) describe an entire family in one natural-language message and receive a
complete, budget-aware, presentation-ready family insurance package — instead of pricing each family
member separately by hand in ePOS.

**Business Value**
- Faster quoting → more quotes per day per agent → higher close rate.
- Fewer manual ePOS calculation errors.
- A single, professional, unified illustration document instead of N disconnected single-person
  illustrations — stronger pitching tool.

**Stakeholders**
- Manulife FA (agent) — primary user/actor.
- End customer (family being insured) — indirect beneficiary, never a direct system actor.
- Product/Underwriting team — owns rider eligibility rules.
- Compliance team — must approve the new PDF template.
- Actuarial/Product Engine team — owns the pricing API that must support multi-insured payloads.
- DESGOD/fegod (design) team — owns the infographic design spec.

**Personas**
- **FA (Financial Advisor / Agent)** — the sole "As a..." actor across the epic; uses M-Smart conversationally.
- **PO (Policy Owner)** — the contract owner/payor (may or may not be the Main Insured).
- **MI (Main Insured)** — primary insured, tied to the base product.
- **OI (Other Insured)** — spouse or child(ren), covered via riders, max 3 per policy.

**In Scope (epic-level)**
Natural-language family intake, PO/MI/OI role resolution, per-member base product + rider recommendation,
budget-aware allocation across the whole family, per-member premium calculation displayed conversationally,
and one unified illustration PDF for the whole family.

**Out of Scope (epic-level)**
Extended family beyond spouse/children (parents, siblings) as insured members; separate independent
policies per family member; ePOS submission (illustration only); saving the family profile to CRM/DPMS;
English-language output; AI/ML-based (non-deterministic) budget optimization; PDF eSignature; automated
PDF delivery via email/Zalo; per-FA PDF template customization.

**Assumptions**
- The Product Engine API can be made to support (or be called sequentially for) a multi-insured payload —
  tracked as the #1 pre-sprint blocker.
- The Rider Eligibility Matrix will be documented by the BA into the knowledge base before development
  starts on the recommendation capability.
- Bug MGAFS-1858 (rider memory loss on re-calculation) is fixed as a prerequisite, not in parallel.
- FA license validation (MIT, per MGAFS-1834 / MGAFS-2071) already exists and is reused unchanged, not
  rebuilt.
- All new conversational and document output is Vietnamese-only for this phase.

---

## STEP 2 — BA: Full Requirement Extraction by Capability

The BA read every section of `user-story-family-quick-pitch.html` (Hero, Current→To-Be, all 9 realistic
agent-input scenarios A1–A3/B1–B3/C1/C3, the Parsing Failure Risk Matrix, all Acceptance Criteria of
MGAFS-2120 [24 AC], MGAFS-2121 [7 AC], MGAFS-2120-A [7 AC], MGAFS-2120-B [7 AC], MGAFS-2120-C [10 AC], all
Out-of-Scope callouts, all Definition-of-Done lists, the Story Map, and the 3 pre-sprint blockers — **56
source Acceptance Criteria in total**, none summarized away.

Every requirement was grouped into exactly **4 business capabilities** based on the distinct backend
responsibility it represents, not the screen it happens to render on:

| Capability | What it owns | Produces (contract) for the next capability |
|---|---|---|
| **C1 — Family Intake & Role Resolution** | Turning a free-text message into a structured, agent-confirmed family profile: who is PO, who is MI, who are the OI, their age/gender/relationship, ask-back for missing mandatory fields, the 3-OI limit, minor-PO block, FA license gate. | A validated **Family Profile** object |
| **C2 — Product & Rider Recommendation** | Selecting the base product for PO/MI and eligible riders for each OI, applying narrowly-scoped per-person exclusions, honoring agent-directed product/rider/amount overrides, handling eligibility gaps and multiple children. | A **Recommended Package** (products + riders, no pricing yet) |
| **C3 — Budget Allocation & Premium Calculation** | Deterministic, rule-based budget-fit trimming; per-member premium calculation; conversational premium display; re-calculation on FA adjustment; validation-error handling; response-time SLA. | A **Priced, budget-fit Package** |
| **C4 — Illustration PDF Generation** | Rendering the priced package into a 7-section infographic PDF (cover, coverage map, premium overview, per-member cards, summary table, policy value chart, fund allocation chart), compliance and bilingual requirements, generation SLA and file-quality constraints. | A downloadable **PDF document** |

Each source requirement item (context row, scenario, AC, business rule, DoD line, OOS line, dependency,
blocker) was tagged to exactly one primary capability (see `Requirement_Traceability_Matrix.md` for the
row-by-row mapping — 100% of source rows are accounted for).

Note on source data quality: the original document's AC numbering for MGAFS-2120's "Eligibility & Coverage
Gaps" group is labelled "AC-8 → AC-11" but only AC-08, AC-09, AC-10 exist in the source HTML (AC-11 is a
numbering gap in the source, not an omission by this exercise). This gap is called out explicitly in the
Traceability Matrix rather than silently invented.

---

## STEP 3 — SA: Technical Impact Review

| Capability | Backend impact | API impact | DB impact | Integration impact | Existing module impact | Technical risk |
|---|---|---|---|---|---|---|
| C1 Intake & Role Resolution | New multi-entity extraction/NLP prompt; extended session-state schema for a multi-persona family object | New/extended LLM extraction call; no external product API calls | Session/conversation state only — no new persistent table (CRM save is explicitly out of scope) | FA license check (MGAFS-2071, in progress) reused as-is | Must not regress the existing single-persona parsing path | Pronoun/relationship-graph ambiguity ("em", "chồng con gái") and age-in-months/numeric-unit parsing ("200k/ngày") are LLM accuracy risks, not architecture risks |
| C2 Recommendation | Eligibility/matching logic against the Rider Eligibility Matrix; per-member recommendation builder | Product catalog / eligibility lookups (existing) | Needs the Rider Eligibility Matrix authored into the knowledge base (Blocker #2) before dev starts | RUV14 Base Product Setup (MGAFS-2057, in progress) needed for full rider coverage | Single-persona recommendation path must keep working unchanged | Scope-leak risk: a per-person exclusion ("no ILP for the mother") must not be applied globally — must be enforced in code, not left to prompt discipline alone |
| C3 Budget & Premium | Deterministic rule-based trimming engine (explicit priority order, not AI-guessed); premium aggregation across up to 4 insured | `calculatePremiums` must accept a multi-insured payload in one call, or the story must fall back to sequential calls + client-side aggregation (Blocker #1 — the single biggest external risk to the whole epic) | None | Depends on Product Engine API contract confirmation | Bug MGAFS-1858 (rider lost on re-calculation) must be fixed first — a 3-OI family will expose it 3× worse than single-persona | Response-time SLA (≤8s) with up to 4 parallel/sequential premium calls is a real perf risk if the API only supports single-insured calls |
| C4 Illustration PDF | Multi-insured PDF layout engine (7 sections, 3 chart types, per-member accent colors); server-side SVG/Canvas rendering | Depends on the PDF generation engine (`gen-pdf-engine`) supporting a multi-insured template | None | Compliance sign-off on the new template — historically the longest lead-time item; should start **in parallel with C1/C2**, not wait for Sprint N+2 | Existing single-person PDF generation path must not regress | SLA ≤30s for 4 insured, ≥150 DPI, ≤5MB output; illustration table ordering bugs (MGAFS-1953/1855/1978) already exist in the single-person path and must not be reintroduced |

**Recommended slicing (SA sign-off):** C1 → C2 → C3 → C4 map directly onto four independently deployable
backend modules with a clean, narrow data contract between each (Family Profile → Recommended Package →
Priced Package → PDF). This is the same natural service boundary the reference document already used for
its 3-step Output Flow (Recommendation → Premium → PDF) plus a Budget Allocation rule engine that the
reference document itself already tracked as an independently blocking rule-engine story
(MGAFS-2121) — the SA agrees the boundary is technically sound and does **not** recommend forcing a
different cut purely to hit a target count.

---

## STEP 4 — PO + BA + SA: Final 4-Story Split

The team agreed to split by **business capability**, not by screen, matching the SA-reviewed module
boundaries above:

| New Story | Capability | Absorbs source stories/AC groups | Story Points |
|---|---|---|---|
| **US-001** | Family Intake & Role Resolution | MGAFS-2120-A (AC-01→AC-07 — intake/ask-back/role-confirmation/limit/license portions; the product-recommendation content embedded in 2120A-AC-01/02's mock-ups is realized by US-002, not duplicated here) + MGAFS-2120 AC-01 (family understanding + confirmation card), AC-06, AC-07, AC-16→AC-20 | 13 |
| **US-002** | Family Product & Rider Recommendation | MGAFS-2120 AC-02, AC-03 (product/rider selection & package-assembly portions), AC-08→AC-10, AC-13, AC-14, AC-21→AC-24 | 13 |
| **US-003** | Family Budget Allocation & Premium Calculation | MGAFS-2121 (all 7 AC) + MGAFS-2120-B (all 7 AC) + MGAFS-2120 AC-04, AC-05, AC-12, AC-15 | 13 |
| **US-004** | Family Illustration PDF Generation | MGAFS-2120-C (all 10 AC) — unchanged capability boundary | 13 |

> Note: 2120-AC-02 ("recommend a complete family package") and 2120-AC-03 ("display the family package
> clearly") each touch more than one downstream capability in the source text (product selection, premium
> totals, and a PDF CTA button, all in one mock-up). Per the capability-ownership principle in Step 3, each
> was decomposed so that the *product/rider selection* decision lives in US-002, the *premium total and
> budget-fit display* lives in US-003 (already covered there by the 2120-B conversational format ACs), and
> the *"view PDF" trigger* lives in US-004 — see `Requirement_Traceability_Matrix.md` for the exact
> row-by-row split. This avoids duplicating the same requirement as separate Acceptance Criteria in two
> stories.

**Why this maximizes the mission's 4 objectives:**
- **Parallel development:** once the Family Profile / Recommended Package / Priced Package contracts are
  agreed up front (documented as Dependencies in each story below), the C1, C2, C3, C4 teams can each build
  and unit-test against a *mocked* upstream contract instead of waiting for the real upstream story to
  finish — this is materially more parallel than treating the whole epic as one 47-point story.
- **Independent deployment:** each capability is a distinct backend module (NLP/session service,
  recommendation engine, pricing/budget rule engine, PDF rendering service) that can be feature-flagged and
  shipped on its own sprint.
- **Independent testing:** each story's Acceptance Criteria are expressed entirely in terms of its own
  input contract and output contract — QA can test C2 with a hand-built Family Profile fixture without C1
  ever running, and so on.
- **Low coupling / high cohesion:** every AC inside a story belongs to the same single business decision
  (who is in the family / what do we recommend / what does it cost / what does the document look like);
  cross-capability concerns (e.g. "recalculate premium after a rider is added") are placed in the
  capability that owns the *decision*, not the capability that happens to *display* the result.

**Story points rationale:** each story pools significant NLP/business-rule complexity (13 pts is the
top of the mission's allowed 5–13 range) and is deliberately not sub-divided further, per PO judgment,
because further splitting (e.g. "happy path" vs "edge cases" as separate stories) would violate the
mission's explicit rule to split by capability, not by scenario type.

**Sequencing:** US-001 → US-002 → US-003 → US-004 must ship in this order (hard dependency chain, same as
the reference document's own Step 1→2→3 flow). Teams may start build against mocked contracts earlier, but
integration testing is necessarily sequential.
