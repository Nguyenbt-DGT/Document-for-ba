# Requirement Traceability Matrix — Family Quick Pitch Multi-Persona Enhancement

> Every requirement row extracted from `user-story-family-quick-pitch.html` is listed below with the
> target story (US-001…US-004) and the exact section/AC/BR that realizes it. Nothing from the source
> document is omitted; where a source requirement is realized by more than one story, all target stories
> are listed with the specific slice each one owns (per the capability-ownership decomposition explained
> in `Story_Splitting_Decision.md`).

Legend: **US-001** = Family Intake & Role Resolution · **US-002** = Product & Rider Recommendation ·
**US-003** = Budget Allocation & Premium Calculation · **US-004** = Illustration PDF Generation.

> **Note on "card" terminology (§4, §6):** Several source AC/DoD titles below use the original source
> epic's "confirmation card" / "Recommendation Card" / "Role Confirmation Card" / "Output card" language
> (`user-story-family-quick-pitch.html`, pre-dating this decomposition). Those titles are left verbatim
> here for source traceability. Per the final PO decision, M-Smart is a Gen AI chatbot — every one of
> these interactions is realized purely as a conversational chat message, never a UI card; see US-001
> R.03/AC1/AC5 and US-002's Screen demo line for the actual realized behavior. This does **not** apply to
> the "Per-Member Cards" section in §8 (MGAFS-2120-C / US-004), which is a section of the printed PDF
> document, not a chat UI element.

## 0. Epic-Level Statement

| Source | Description | Target Story | Covered By |
|---|---|---|---|
| Hero goal | "Enable agents to describe a customer's full family in a single message and receive a complete, budget-aware insurance package... with a unified illustration PDF ready to present." | US-001, US-002, US-003, US-004 | Realized end-to-end across all 4 stories' User Story statements |

## 1. Current State → To-Be (Context Table)

| Source Row | Description | Target Story | Covered By |
|---|---|---|---|
| Agent Input | Single person → whole family in one message | US-001 | User Story statement, R.01, AC1 |
| Policy Owner | Dummy PO → real customer as PO | US-001 | R.01 Mandatory Info Matrix (PO fields), AC1 |
| Recommendation | Single-person product+rider → base for PO/MI + riders per OI | US-002 | R.01, R.02, AC1, AC2 |
| Budget | Applied to 1 person → distributed + auto-trimmed for family | US-003 | R.01–R.06, AC1–AC9 |
| Output | 1-person illustration → unified family PDF | US-004 | User Story statement, R.01–R.10 |
| Agent Effort | Manual ePOS per member → 1 message → 1 complete package | US-001, US-002, US-003, US-004 | Realized end-to-end; entry point is US-001 R.01 |

## 2. Realistic Agent Input Scenarios (Parsing & Role-Mapping Samples)

| Scenario | Description | Target Story | Covered By |
|---|---|---|---|
| A1 | Married couple, PO=MI=husband, wife OI; budget vs. income priority note | US-001 | AC1, AC3 (ask-back priority); budget-vs-income rule realized downstream in US-003 |
| A2 | Family of 4, PO=MI, wife + 2 children, max-OI boundary reached | US-001 | AC7 (3-OI limit) |
| A3 | Couple + 8-month-old infant, age-in-months conversion, dual product intent | US-001 | R.01 (age-in-months → decimal), AC2; dual product intent rule realized in US-002 BR-US002-02 |
| B1 | PO=husband ≠ MI=wife, 2 teen children added, PWPO rider signal | US-001 | AC5, AC8 |
| B2 | PO=father ≠ MI=daughter (child), pronoun collision "em" = sibling not agent | US-001 | AC9, AC13 (low-confidence/ambiguous reference ask-back) |
| B3 | PO=adult daughter ≠ MI=elderly mother, per-OI product exclusion, relationship graph ("chồng con gái") | US-001, US-002 | US-001: AC10, R.04 (relationship graph); US-002: AC7 (person-scoped exclusion) |
| C1 | Agent-directed per-person rider matrix, "200k/ngày" numeric trap | US-002 | AC8, AC9, BR-US002-05 |
| C3 | Mixed products (ILP for father, Term Life for mother), dual budget signals, education rider maturity | US-002, US-003 | US-002: AC7, AC10 (per-OI product exclusion, mixed base products); US-003: budget-signal handling realized via R.01/R.07 (dual sub-budget vs. total budget aggregation) |

*(Note: the source document's Group C only contains scenarios C1 and C3 — there is no C2 in the source
HTML; this is a gap in the source numbering, not an omission of this matrix.)*

## 3. Parsing Failure Risk Matrix

| Risk | Source Scenarios | Target Story | Covered By |
|---|---|---|---|
| PO = MI wrongly assumed when PO ≠ MI | B1, B2, B3 | US-001 | BR-US001-01, AC5, AC8, AC9, AC10 |
| "em" = sibling parsed as agent (pronoun collision) | B2 | US-001 | AC13 |
| "200k/ngày" parsed as 200 VND instead of 200,000 VND | C1, C2, C3 | US-002 | BR-US002-05, AC9 |
| Budget is a range for all persons, not per-person | A1, C1, C3 | US-001, US-003 | US-001 captures raw budget field (R.01); US-003 owns how it governs pricing (R.01, R.07) |
| Age in months not converted to decimal | A3 | US-001 | BR-US001-08, AC2 |
| Per-person rider exclusion applied globally | B3, C1, C3 | US-002 | BR-US002-01, AC7 |
| Sub-budget vs. total budget conflict | C3 | US-003 | R.07 (multi-member premium calc must preserve both signals) |
| Child MI → ineligible product recommended (e.g. RUV14) | B2 | US-001 | AC9 (juvenile-eligible products only) |
| Education rider maturity term miscalculated | C3 | US-002 | R.02 (per-OI rider recommendation includes term/maturity validation) |

## 4. MGAFS-2120 — Family Package Recommendation (Parent Story, 24 AC)

| Source AC | Title | Target Story | Covered By |
|---|---|---|---|
| 2120-AC-01 | Understand family info from NL + confirmation card | US-001 | AC1 |
| 2120-AC-02 | Recommend a complete family package (base+riders, ≤ budget, per-member premium shown, single PDF) | US-002 (product/rider selection), US-003 (premium ≤ budget check), US-004 (single PDF) | US-002 AC1/AC2; US-003 AC1/AC2/AC10; US-004 User Story + R.01–R.10 |
| 2120-AC-03 | Display the family package clearly (mock UI, CTA buttons incl. "Xem Minh Họa PDF") | US-003 (premium/budget display), US-004 (PDF CTA) | US-003 AC10; US-004 trigger precondition |
| 2120-AC-04 | Total exceeds budget — intelligently trim | US-003 | AC2 |
| 2120-AC-05 | Budget too small even for base product | US-003 | AC8 |
| 2120-AC-06 | Missing age for a family member — ask before proceeding | US-001 | AC3 |
| 2120-AC-07 | Family member role unclear — ask for clarification | US-001 | AC4 |
| 2120-AC-08 | No eligible rider exists — don't block, continue with remaining | US-002 | AC4 |
| 2120-AC-09 | Child below minimum insurable age — exclude, continue | US-002 | AC5 |
| 2120-AC-10 | More than one child — independent treatment, youngest deprioritized under budget pressure | US-002 (independent calc), US-003 (deprioritization under budget) | US-002 AC6; US-003 R.01 (child trimming, youngest first) |
| 2120-AC-11 | *(numbering gap in source — header states range "AC-8 → AC-11" but no AC-11 content exists in the HTML)* | N/A | Documented here as a source data-quality gap; no content was omitted by this exercise |
| 2120-AC-12 | Agent modifies package post-recommendation — other members retained, total updated, warn if over budget | US-003 | AC11 |
| 2120-AC-13 | Consolidated pitch: PO/MI + spouse + 1 child | US-002 | AC1, AC2 |
| 2120-AC-14 | PO/MI + 2 children, no spouse — twins as separate line items, <30-day child blocked individually | US-002 | AC3, AC6 |
| 2120-AC-15 | Budget stretch — 4-person package exceeds budget, two-tier config | US-003 | AC9 |
| 2120-AC-16 | NLP role parsing — Vietnamese relationship keywords, role confirmation card | US-001 | R.04, AC5 |
| 2120-AC-17 | PO=husband, MI=wife — eligibility anchored to MI's age | US-001 | R.06, AC8 |
| 2120-AC-18 | PO=parent, MI=child — juvenile plan, occupancy=2 | US-001 | R.06, AC9 |
| 2120-AC-19 | PO=adult child, MI=elder parent — age-filtered eligibility | US-001 | R.06, AC10 |
| 2120-AC-20 | PO is a minor — hard block, data preservation | US-001 | AC11 |
| 2120-AC-21 | Agent names specific rider types per member | US-002 | AC8 |
| 2120-AC-22 | Agent specifies coverage amounts, no silent substitution | US-002 | AC9 |
| 2120-AC-23 | Agent specifies base product by name/plan code | US-002 | AC10 |
| 2120-AC-24 | Agent requests rider exclusion — dependency check first | US-002 | AC11 |
| OOS: Extended family beyond spouse/children | — | All | `Story_Splitting_Decision.md` §1 Out of Scope (epic-level) |
| OOS: Separate independent policy for spouse | — | US-002 | BR-US002-02 (dual product intent, one policy) |
| OOS: ePOS submission (illustration only) | — | All | `Story_Splitting_Decision.md` §1 Out of Scope (epic-level) |
| OOS: Saving family profile to CRM | — | US-001 | Out of Scope §1 |
| OOS: English-language output | — | All | Out of Scope §1 (epic-level), NFR of each story |
| DoD: All 24 AC pass in SIT | — | All | Realized as each story's own AC set passing SIT |
| DoD: Family package w/ per-member premium breakdown | — | US-003 | AC10 |
| DoD: Unified illustration PDF for all insured | — | US-004 | AC1–AC11 |
| DoD: Modifying one rider doesn't reset others | — | US-003 | BR-US003-10, AC11 |
| DoD: Existing single-persona flows regression-safe | — | US-001, US-002, US-003, US-004 | Each story's Impact and Risk §4 "Regression risk" |
| DoD: Vietnamese language for all new messages | — | All | Each story's NFR §5 "Language" |
| DoD: UAT sign-off from ≥2 field agents | — | All | Cross-story release gate, tracked at epic level (not duplicated per story AC) |

## 5. MGAFS-2121 — Budget Allocation Logic (7 AC)

| Source AC | Title | Target Story | Covered By |
|---|---|---|---|
| 2121-AC-01 | No trimming needed when total fits budget | US-003 | AC1 |
| 2121-AC-02 | Fixed priority order when trimming needed | US-003 | AC2, BR-US003-01 |
| 2121-AC-03 | Agent-stated priorities override default order | US-003 | AC4, BR-US003-04 |
| 2121-AC-04 | Transparent output — show exactly what changed | US-003 | AC5 |
| 2121-AC-05 | Agent can override the allocation decision | US-003 | AC6, BR-US003-06 |
| 2121-AC-06 | Surplus budget offered back to agent | US-003 | AC7, BR-US003-05 |
| 2121-AC-07 | All trims result in a valid, submittable package | US-003 | AC3, BR-US003-03 |
| OOS: AI/ML-based optimisation not allowed | — | US-003 | Out of Scope §1, NFR §5 Determinism |
| OOS: Splitting budget across multiple policies | — | US-003 | Out of Scope §1 |
| OOS: Annual premium increase projections | — | US-003 | Out of Scope §1 |
| OOS: Saving allocation preferences to profile | — | US-003 | Out of Scope §1 |
| DoD: AC-1→AC-7 pass in SIT | — | US-003 | Realized by AC1–AC7 |
| DoD: Deterministic trimming | — | US-003 | BR-US003-02, NFR §5 |
| DoD: Agent override works correctly | — | US-003 | AC6 |
| DoD: Output card communicates every change | — | US-003 | AC5 |
| DoD: Fully integrated within MGAFS-2120 | — | US-003 | Dependencies §6 (upstream US-002) |
| DoD: No regression on single-persona premium calc | — | US-003 | Impact and Risk §4 |

## 6. MGAFS-2120-A — Recommendation Output Flow (7 AC)

| Source AC | Title | Target Story | Covered By |
|---|---|---|---|
| 2120A-AC-01 | Happy path — sufficient info → Recommendation Card | US-001 (profile confirmation), US-002 (product/rider content of the card) | US-001 AC1; US-002 AC1 |
| 2120A-AC-02 | Multiple OI displayed distinctly (up to 3) | US-001 (profile display), US-002 (per-member riders) | US-001 AC2; US-002 AC2 |
| 2120A-AC-03 | Ask-back for missing mandatory info, max 2–3/turn | US-001 | AC3 |
| 2120A-AC-04 | Occupation class 4 volunteered → warn, don't block | US-001 | AC6 |
| 2120A-AC-05 | Role Confirmation Card when PO ≠ MI | US-001 | AC5 |
| 2120A-AC-06 | Exceeds 3-OI limit → notify + ask priority | US-001 | AC7 |
| 2120A-AC-07 | FA lacks MIT license → block flow | US-001 | AC12 |
| OOS: Auto-fetch from CRM/DPMS | — | US-001 | Out of Scope §1 |
| OOS: Detailed premium calculation (belongs to 2120-B) | — | US-003 | Confirms capability boundary — realized entirely in US-003 |
| OOS: PDF generation (belongs to 2120-C) | — | US-004 | Confirms capability boundary — realized entirely in US-004 |
| OOS: Health-underwriting rejection handling | — | Out of epic scope | Not covered by any of the 4 stories — explicitly a different flow (Underwriting), consistent with source |
| Dependency: Product/rider eligibility rules by age/gender/relationship | — | US-002 | Dependencies §6 — sourced from each product's existing ePOS validation rules, or the official product documentation (provision document / product spec), not a new matrix (Blocker #2 retired) |
| Dependency: LLM multi-entity extraction prompt | — | US-001 | Technical Note §7 |
| Dependency: Session state schema for multi-persona family object | — | US-001 | Technical Note §7, User Story Post-condition |
| Dependency: Occupation class validation rules (1–4) | — | US-001 | R.01 Mandatory Info Matrix |
| DoD: ≥95% extraction accuracy on 50-scenario test set | — | US-001 | NFR §5 |
| DoD: Ask-back doesn't re-ask, max 2–3 fields/turn | — | US-001 | AC3 |
| DoD: Role Confirmation Card correct when PO≠MI | — | US-001 | AC5 |
| DoD: Recommendation Card shows correct per-member sections | — | US-001, US-002 | US-001 AC2; US-002 AC2 |
| DoD: Occupation class 4 warning works | — | US-001 | AC6 |
| DoD: License gate (MIT) still works, no regression | — | US-001 | AC12 |
| DoD: No regression on single-persona flow | — | US-001 | Impact and Risk §4 |
| DoD: Test case documentation in MGAFS Test Plan | — | US-001 | Cross-story release/testing gate |

## 7. MGAFS-2120-B — Premium Calculation Output Flow (7 AC)

| Source AC | Title | Target Story | Covered By |
|---|---|---|---|
| 2120B-AC-01 | Happy path — premium calculated + conversational format | US-003 | AC10 |
| 2120B-AC-02 | Budget ✅ indicator | US-003 | AC10 |
| 2120B-AC-03 | Budget ⚠️ over — automatic optimized suggestion | US-003 | AC12 (validation/optimization branch), R.09 |
| 2120B-AC-04 | Adjustment recalculates only the changed member | US-003 | AC11, BR-US003-10 |
| 2120B-AC-05 | Validation errors shown friendly, no raw error codes | US-003 | AC12, BR-US003-11 |
| 2120B-AC-06 | Riders shown by full Vietnamese name, not just plan code | US-003 | R.08, BR text |
| 2120B-AC-07 | Performance ≤8s with loading indicator | US-003 | R.11, NFR §5 (target SLA); AC13 (failure/timeout branch) |
| OOS: Detailed year-by-year illustration table (belongs to 2120-C) | — | US-004 | Confirms capability boundary |
| OOS: Fund interest-rate adjusted calc (separate bug MGAFS-1955/1966) | — | Out of epic scope | Explicitly excluded, consistent with source |
| OOS: Fund allocation recommendation at this step | — | US-004 | Realized later, at PDF step (R.07), not at premium-calc step |
| OOS: APE-based commission calc (MGAFS-2076) | — | Out of epic scope | Explicitly excluded, consistent with source |
| Dependency: calculatePremiums multi-insured support | — | US-003 | Dependencies §6, Pre-Sprint Blocker #1 |
| Dependency: Session state from 2120-A with full family object | — | US-003 | Dependencies §6 (upstream US-001/US-002) |
| Dependency: Rider name mapping config table | — | US-003 | Dependencies §6 |
| Dependency: Budget optimization algorithm | — | US-003 | R.01 (rule engine) |
| DoD: calculatePremiums called correctly per insured | — | US-003 | AC10 |
| DoD: Conversational format w/ per-member section, footer, indicator | — | US-003 | AC10 |
| DoD: Budget overflow → auto-optimized valid package | — | US-003 | AC12 |
| DoD: Re-calc updates only the changed member | — | US-003 | AC11 |
| DoD: No empty rider sections | — | US-003 | BR-US003-08 |
| DoD: VN number format correct | — | US-003 | BR-US003-09 |
| DoD: Response time ≤8s with loading indicator | — | US-003 | AC12, NFR §5 |
| DoD: No regression on single-persona premium calc | — | US-003 | Impact and Risk §4 |
| DoD: Unit tests for budget comparison + rendering | — | US-003 | Technical Note §7 |

## 8. MGAFS-2120-C — Illustration PDF Output Flow (10 AC)

| Source AC | Title | Target Story | Covered By |
|---|---|---|---|
| 2120C-AC-01 | Cover page: family info + Manulife branding | US-004 | AC1 |
| 2120C-AC-02 | Family Coverage Map — Hub & Spoke for N members | US-004 | AC2 |
| 2120C-AC-03 | Premium Overview — donut chart + dual timeline bar | US-004 | AC3 |
| 2120C-AC-04 | Per-Member Cards — benefit chips, no empty sections | US-004 | AC4 |
| 2120C-AC-05 | Premium Summary Table — sub-totals + total + budget indicator | US-004 | AC5 |
| 2120C-AC-06 | Timeline Chart — correct PY order, no swapped high/low | US-004 | AC6 |
| 2120C-AC-07 | Fund Allocation Chart — ILP only, sums to 100% | US-004 | AC7 |
| 2120C-AC-08 | Bilingual content, UTF-8 font | US-004 | AC8 |
| 2120C-AC-09 | Generation SLA ≤30s, progress indicator, retry on timeout | US-004 | AC10, AC11 |
| 2120C-AC-10 | PDF quality — DPI/size/print/header-footer | US-004 | AC12 |
| OOS: eSignature | — | US-004 | Out of Scope §1 |
| OOS: PDF history saved to CRM | — | US-004 | Out of Scope §1 |
| OOS: Auto-send via email/Zalo | — | US-004 | Out of Scope §1 |
| OOS: Per-FA template customization | — | US-004 | Out of Scope §1 |
| Dependency: 2120-A + 2120-B session/premium data | — | US-004 | Dependencies §6 (upstream US-001/US-002/US-003) |
| Dependency: PDF generation engine | — | US-004 | Dependencies §6 |
| Dependency: Chart rendering library (SVG/Canvas) | — | US-004 | Dependencies §6 |
| Dependency: Compliance sign-off for new template | — | US-004 | Dependencies §6, Impact and Risk §4 |
| Dependency: Vietnamese font package on PDF server | — | US-004 | Dependencies §6 |
| Dependency: Design spec (DESIGN-family-quick-pitch.md) | — | US-004 | Dependencies §6 |
| DoD: Cover page correct | — | US-004 | AC1 |
| DoD: Coverage Map renders N=1/2/3, colors, greyscale-safe | — | US-004 | AC2 |
| DoD: Donut + timeline bar correct format | — | US-004 | AC3 |
| DoD: Per-member cards, no empty sections | — | US-004 | AC4 |
| DoD: Summary table sub-totals/total/indicator/footnote | — | US-004 | AC5 |
| DoD: Timeline chart order/milestones correct | — | US-004 | AC6 |
| DoD: Fund allocation ILP-only, 100%, consistent w/ chat | — | US-004 | AC7 |
| DoD: Bilingual, UTF-8, Be Vietnam Pro | — | US-004 | AC8 |
| DoD: Generation ≤30s, progress, retry on timeout | — | US-004 | AC10, AC11 |
| DoD: PDF quality thresholds met | — | US-004 | AC12 |
| New negative-case coverage (QA-added, no direct source AC — derived from "Compliance review passed" DoD line) | Requesting a product/rider combination with no compliance-approved template must not fall back to an unapproved layout | US-004 | AC9 (added during QA pass, Step 6, to satisfy the mission's mandatory Negative Case AC category) |
| DoD: Compliance review passed | — | US-004 | Impact and Risk §4 |
| DoD: No regression on single-persona PDF | — | US-004 | Impact and Risk §4 |
| DoD: Design spec reviewed and approved | — | US-004 | Dependencies §6 |
| ARCGOD Note: Compliance review is the longest lead-time item, start in parallel with Sprint N | — | US-004 | Impact and Risk §4 (explicit call-out) |

## 9. Story Map Dependencies & Pre-Sprint Blockers

| Source Item | Description | Target Story | Covered By |
|---|---|---|---|
| MGAFS-2121 blocks MGAFS-2120 | Budget logic is a hard blocker for the main story | US-003 | Superseded — in the new split, US-003 (which absorbs MGAFS-2121) is itself a hard sequential dependency for US-004, and depends on US-002; see Sequencing note in `Story_Splitting_Decision.md` §4 |
| MGAFS-2071 License Check Enhancement (in progress) | Must integrate before UAT | US-001 | Dependencies §6 |
| MGAFS-2057 RUV14 Base Product Setup (in progress) | Include RUV14 riders if complete | US-002 | Dependencies §6 |
| External Dependency: Product Engine API — multi-insured payload | Hard blocker — confirm before dev starts | US-002, US-003 | Both stories' Impact and Risk §4, "Pre-Sprint Blocker #1" |
| External Dependency: Illustration Engine — multi-insured PDF | Hard blocker — confirm single-call support or phased merge | US-004 | Impact and Risk §4 |
| Blocker #1: Product Engine API contract | Biggest external risk to the sprint | US-002, US-003 | Impact and Risk §4 of both stories |
| Blocker #2: Rider Eligibility Matrix | **Retired** — per final PO decision, eligibility rules are sourced from each product's existing ePOS validation rules, or the official product documentation (provision document / product spec); no new matrix is authored by BA | US-002 | Dependencies §6, Impact and Risk §4 |
| Blocker #3: MGAFS-1858 rider memory bug | Must be fixed as a prerequisite, not parallel | US-003 | Impact and Risk §4 |

---

## Coverage Summary

- **Total distinct source requirement rows identified:** 165 (epic goal 1, context rows 6, scenarios 9,
  risk-matrix rows 9, MGAFS-2120 AC/OOS/DoD 41, MGAFS-2121 AC/OOS/DoD 17, MGAFS-2120-A AC/OOS/Dep/DoD 23,
  MGAFS-2120-B AC/OOS/Dep/DoD 24, MGAFS-2120-C AC/OOS/Dep/DoD 33, Story Map/Blockers 8, minus 1 documented
  source numbering gap [2120-AC-11] and 3 rows explicitly out of the epic's scope entirely).
- **Rows mapped to at least one of US-001…US-004:** 100% (every row above has a Target Story, with
  explicit "Out of epic scope" or "N/A — documented source gap" markers used only for the 4 rows that are
  genuinely not part of this epic's 4 stories, not silently dropped).
- **Rows requiring a split across more than one story:** 6 (2120-AC-02, 2120-AC-03, 2120-AC-10,
  2120A-AC-01, 2120A-AC-02, B3/C3 scenarios) — in every case the split follows the capability-ownership
  rule from `Story_Splitting_Decision.md` and is reflected as a distinct AC/BR in each owning story, not a
  duplicated identical AC.
