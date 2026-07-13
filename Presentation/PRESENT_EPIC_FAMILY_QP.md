# Family Quick Pitch — Multi-Persona Enhancement (Epic Summary)

> Presentation summary of `US-001.md` → `US-004.md`. Source epic: `user-story-family-quick-pitch.html` —
> M-Smart Quick Pitch, Family Quick Pitch Multi-Persona Enhancement. Full detail (Scope, Requirements,
> NFR, Dependencies, Technical Notes) lives in the individual `us/US-00N.md` files; this document
> condenses each story to 5 points: **Objective, Business Rules, Trigger, Pre/Post-condition, AC Scenario
> Flow**.

## Epic Flow

```
US-001                 US-002                  US-003                    US-004
Family Intake    →     Product & Rider    →    Budget Allocation &  →    Illustration PDF
& Role Resolution       Recommendation           Premium Calculation       Generation
(Family Profile)        (Recommended Package)    (Priced Package)         (PDF document)
```

M-Smart is a **Gen AI chatbot** — every FA-facing confirmation and summary across all 4 stories is a
conversational chat message, never a UI card. Product/rider eligibility is sourced from each product's
existing **ePOS validation rules, or official product documentation (provision document / product
spec)** — not a new eligibility matrix.

---

## US-001 — Family Intake & Role Resolution

### 1. Objective

As a Manulife agent (FA) using M-Smart, describe the customer's full family in a single natural-language
message and have M-Smart correctly identify the Policy Owner (PO), Main Insured (MI), and Other Insured
(OI — spouse/children), asking back only for what's genuinely missing — so the FA doesn't fill separate
forms per family member, and the rest of the Quick Pitch flow starts from an accurate, agent-confirmed
family profile.

### 2. Business Rules

| Rule ID | Rule Name | Description |
|---|---|---|
| BR-US001-01 | PO = MI by default | Assumed equal unless the message has an explicit signal otherwise |
| BR-US001-02 | Occupation class default, never proactively asked | Class 1 (adult) / class 2 (child <18), silently applied |
| BR-US001-03 | Occupation class 4 accepted only if volunteered | FA-stated only; triggers a non-blocking warning |
| BR-US001-04 | Max 3 OI per policy | Excess members trigger a prioritization prompt, not silent truncation |
| BR-US001-05 | PO must be ≥ 18 | Hard block; already-collected MI/OI data is preserved |
| BR-US001-06 | MI's age governs eligibility when PO ≠ MI | PO's age checked only against PO-specific limits |
| BR-US001-07 | Ambiguous relationship blocks progress | Any person without a clear relationship to PO must be clarified |
| BR-US001-08 | Age in months → decimal years | e.g. 8 tháng → ≈0.67, validated against the 30-day minimum |

### 3. Trigger

FA sends a free-text message describing 2 or more family members (self + spouse and/or children) inside
an active M-Smart Quick Pitch conversation.

### 4. Pre / Post-condition

- **Pre-condition:** FA is licensed (valid MIT certification per MGAFS-1834); M-Smart session is active.
- **Post-condition:** A structured, agent-confirmed **Family Profile** (PO, MI, up to 3 OI with
  age/gender/relationship, plus PO's income/budget, risk appetite, payment term) is stored in session
  state and made available to US-002.

### 5. AC Scenario Flow

| AC | Type | Given | When | Then |
|---|---|---|---|---|
| AC1 | Happy Path | Message provides every mandatory field for PO/MI/each OI | M-Smart processes the message | Sends a conversational summary of the family structure and stores the confirmed Family Profile; does not proceed until the FA confirms/corrects in chat |
| AC2 | Happy Path | PO/MI + spouse + 2 children (incl. one age in months) in one message | M-Smart builds the Family Profile | Creates 3 distinct, correctly-labeled OI entries; age-in-months → decimal; each OI validated independently |
| AC3 | Validation | Message omits a mandatory field | M-Smart detects the gap | Asks a natural-language follow-up (max 2–3 fields, PO→MI→OI order); never re-asks; never asks about occupation |
| AC4 | Edge Case | A mentioned person has no stated relationship to PO | M-Smart parses the message | Asks which relationship applies; only adds the person once confirmed |
| AC5 | Edge Case | Message implies PO ≠ MI | M-Smart detects the pattern | Sends a conversational role-confirmation message (payer vs. main insured); blocks downstream until the FA replies; asks explicitly if still ambiguous |
| AC6 | Edge Case | FA proactively states occupation class 4 | M-Smart builds the profile | Overrides default class with a non-blocking warning; flow continues |
| AC7 | Edge Case | Message describes more than 3 OI | M-Smart detects the overflow | Informs the FA of the 3-OI limit, proposes the first 3, asks the FA to prioritize |
| AC8 | Edge Case | PO = husband, MI = wife | M-Smart finalizes the profile | Eligibility anchored to MI's age/gender; PO checked only against PO-specific limits; blocks if PO exceeds max PO age |
| AC9 | Edge Case | PO = parent, MI = child (0–17) | M-Smart finalizes the profile | Restricts to juvenile-eligible products, occupancy = 2; blocks entirely if MI is under 30 days old |
| AC10 | Negative Case | PO = adult child, MI = elderly parent exceeding every product's max issue age | M-Smart evaluates eligibility | No pitch generated, redirects to a consultant; if only some products excluded, flags elder-appropriate products instead |
| AC11 | Negative Case | PO ≠ MI and confirmed PO age < 18 | M-Smart evaluates the profile | Hard-blocks Quick Pitch generation, preserves already-collected MI/OI data; "17 sắp 18" still treated as 17 |
| AC12 | Exception Case | FA lacks a valid MIT license | FA attempts to start a Family Quick Pitch conversation | Blocks the flow via existing MGAFS-1834 messaging; no exception for multi-persona |
| AC13 | Integration Failure | NLP extraction returns low-confidence/conflicting role assignment | M-Smart evaluates the extraction result | Never silently assumes a role — asks the FA to clarify; rest of the confidently-extracted profile is retained |

---

## US-002 — Family Product & Rider Recommendation Engine

### 1. Objective

As a Manulife agent (FA) using M-Smart, get the right base product for the main insured and the right
riders for each spouse/child — respecting any product/rider the FA explicitly specifies — so the FA
receives one coherent, itemized family package instead of manually looking up eligibility per member.

### 2. Business Rules

| Rule ID | Rule Name | Description |
|---|---|---|
| BR-US002-01 | Exclusions are person-scoped, never generalized | A stated exclusion applies only to the named member |
| BR-US002-02 | Dual product intent on one policy | Base product (e.g. ILP) for MI + protection-only riders for OI, on one policy |
| BR-US002-03 | Missing rider eligibility does not block the pitch | Exclude that member's coverage, continue with the rest |
| BR-US002-04 | Below-minimum-age child is excluded, not blocking | Rest of the family package proceeds |
| BR-US002-05 | Numeric shorthand parsing | "k" = thousand VND, "triệu"/"tr" = million VND |
| BR-US002-06 | Agent-stated values are authoritative | Used exactly as stated after eligibility validation, never silently replaced |
| BR-US002-07 | Rider dependency check before exclusion | Warn before removing a rider another active rider depends on |

### 3. Trigger

A confirmed **Family Profile** is available from US-001.

### 4. Pre / Post-condition

- **Pre-condition:** US-001 has produced a validated Family Profile (PO, MI, up to 3 OI with
  age/gender/relationship confirmed).
- **Post-condition:** A structured **Recommended Package** (base product for MI + itemized rider list per
  OI, with agent-directed overrides applied and labeled) is stored in session state and handed to US-003.

### 5. AC Scenario Flow

| AC | Type | Given | When | Then |
|---|---|---|---|---|
| AC1 | Happy Path | Confirmed profile: PO=MI, one spouse, one child, all eligible | M-Smart builds the recommendation | Base product + ≥1 rider each for spouse/child, all tagged system-suggested |
| AC2 | Happy Path | Confirmed profile: PO/MI + spouse + 2 children | M-Smart assembles the package | One base-product line + one rider line per OI, individually identified |
| AC3 | Validation | Family Profile has twin children | M-Smart builds rider recommendations | Two separate, independently-checked line items — never one doubled-quantity item |
| AC4 | Edge Case | No eligible rider for the spouse | M-Smart finalizes the recommendation | Doesn't block the package; informs the FA; returns the valid remaining package |
| AC5 | Edge Case | Child's age below every available rider's minimum | M-Smart evaluates eligibility | Excludes the child, informs the FA, continues for the rest |
| AC6 | Edge Case | 2+ children of different ages | M-Smart builds recommendations | Each treated independently; package annotated so youngest is deprioritized first if US-003 must trim |
| AC7 | Negative Case | Exclusion named for exactly one member, another member wants the same product type | M-Smart builds the package | Exclusion applies only to the named member; asks for clarification if ambiguous |
| AC8 | Happy Path | FA explicitly assigns riders per member | M-Smart parses the request | Binds each rider after validating eligibility, labels "theo yêu cầu"; flags conflicts with swap options |
| AC9 | Validation | FA states an explicit sum assured incl. Vietnamese shorthand | M-Smart parses the amount | Correctly interprets shorthand, confirms with the FA, validates against min/max; used exactly as stated |
| AC10 | Edge Case | FA names a specific base product/plan code | M-Smart evaluates the request | Accepts after validation (active, licensed, eligible); proposes a replacement if discontinued |
| AC11 | Exception Case | FA requests a rider removal that has a dependency conflict | M-Smart processes the request | Warns before removing; removes only after confirmation; never mandatory-rider removal; never silently re-adds |
| AC12 | Integration Failure | A product/rider combination has no entry (or incomplete) in its ePOS validation rules/product documentation | M-Smart evaluates eligibility | Never fabricates eligibility — excludes that rider, flags for BA follow-up, returns the rest of the valid package |

---

## US-003 — Family Budget Allocation & Premium Calculation

### 1. Objective

As a Manulife agent (FA) presenting a family insurance package, have M-Smart automatically fit the
package within the customer's stated budget — trimming coverage in a smart, transparent, rule-based way —
and show the resulting premium per family member conversationally, so the FA always gets a valid,
presentable, budget-fit proposal without manually recalculating.

### 2. Business Rules

| Rule ID | Rule Name | Description |
|---|---|---|
| BR-US003-01 | Fixed trim priority order | Spouse rider → child rider (youngest first) → base product |
| BR-US003-02 | Trimming is rule-based, not AI-guessed | Same input always produces the same trimmed output |
| BR-US003-03 | Every trim keeps the package valid | No item below minimum; no rider without a valid base product |
| BR-US003-04 | Agent priority overrides default order | Explicit FA priority changes the trim order for that session |
| BR-US003-05 | Surplus is offered, never auto-applied | FA must explicitly accept any leftover-budget increase |
| BR-US003-06 | Post-trim override is advisory, not a hard block | FA can proceed over budget after an explicit override, with a warning |
| BR-US003-07 | Auto-optimized package must meet the APE floor | ≥10,000,000 VND minimum (MGAFS-2033) |
| BR-US003-08 | No empty rider sections | A member with zero riders shows no blank rider block |
| BR-US003-09 | Vietnamese number formatting | Period as thousands separator |
| BR-US003-10 | Adjustment retains unrelated members | Recalculating one member never resets another's confirmed coverage |
| BR-US003-11 | No raw technical errors shown to FA | Backend errors translated into a friendly, actionable Vietnamese message |

### 3. Trigger

A **Recommended Package** is available from US-002.

### 4. Pre / Post-condition

- **Pre-condition:** US-002 has produced an itemized Recommended Package (base product + per-member
  riders), and the PO's stated monthly budget is known from the Family Profile (US-001).
- **Post-condition:** A **Priced, budget-fit Package** — every line item costed, trimmed if necessary,
  with a clear budget-fit indicator — is stored in session state and handed to US-004.

### 5. AC Scenario Flow

| AC | Type | Given | When | Then |
|---|---|---|---|---|
| AC1 | Happy Path | Total premium ≤ stated budget | M-Smart evaluates the package | Package presented unchanged with "✓ Trong ngân sách"; no trimming runs |
| AC2 | Happy Path | Total premium exceeds budget | M-Smart applies the trimming engine | Trims spouse rider → child riders (youngest first) → base product last resort, never below minimum; informs the FA what/why |
| AC3 | Validation | Trimming has been applied | M-Smart finalizes the trimmed package | Every item still meets minimum rules; no orphaned rider |
| AC4 | Edge Case | FA explicitly states a trim priority | Package needs trimming | Trims in the stated order instead of default; confirms the adjusted order back to the FA |
| AC5 | Happy Path | M-Smart has trimmed the package | Result is presented | Shows original/adjusted value, premium, and status per item, plus new grand total and budget-fit status |
| AC6 | Edge Case | M-Smart has auto-trimmed | FA requests a different arrangement | Applies the override, recalculates; warns (not blocks) if still over budget |
| AC7 | Edge Case | Meaningful surplus remains after trimming | M-Smart finalizes the trimmed package | Informs the FA of the surplus, offers an increase; never auto-applies |
| AC8 | Negative Case | Budget below the minimum premium for every eligible base product | M-Smart evaluates the package | No package generated; informs the FA of the minimum required; offers to adjust budget or goal |
| AC9 | Edge Case | Even the maximally-trimmed package doesn't fit the full family | M-Smart finalizes the recommendation | Presents Ideal vs. Priority-First two-tier configs; if only the base plan fits, outputs single-insured pitch with deferred-member notice |
| AC10 | Happy Path | Finalized (possibly trimmed) package for MI + up to 3 OI | M-Smart calculates and displays premiums | One section per insured, footer with total + budget-fit indicator; no empty rider sections; VN number formatting |
| AC11 | Edge Case | FA requests a change to one member's coverage | M-Smart recalculates | Only that member's section updates; others retain prior values; footer total updates, warns if over budget |
| AC12 | Exception Case | FA requests a sum assured/premium outside the valid range | M-Smart processes the request | Friendly, specific Vietnamese message with the actual min/max; never a raw error code |
| AC13 | Integration Failure | `calculatePremiums` call fails, times out, or is unavailable | M-Smart attempts to calculate premiums | Friendly, retry-capable message; never returns a partial/stale/fabricated price |

---

## US-004 — Family Illustration PDF — Infographic Generation

### 1. Objective

As a Manulife agent (FA) wanting to leave a professional document after a consultation, have M-Smart
generate one illustration PDF with a clear infographic layout (per-member benefits, coverage-timeline
chart, family protection map, fund allocation chart) instead of a plain numeric table — so the customer
understands it immediately and the FA has a stronger pitching tool.

### 2. Business Rules

| Rule ID | Rule Name | Description |
|---|---|---|
| BR-US004-01 | No PII beyond role/age/gender | Never prints a real customer name |
| BR-US004-02 | Fixed per-member accent-color palette | MI/BMBH/Spouse/Child1/Child2 fixed colors, greyscale-distinguishable |
| BR-US004-03 | Donut chart segment limits | ≤5 segments; no segment under 5% of the total |
| BR-US004-04 | No empty rider sections | A member with zero riders shows no blank benefit-chip block |
| BR-US004-05 | Timeline chart ordering integrity | High/low lines never swap; Policy Years strictly ascending |
| BR-US004-06 | Fund allocation chart is ILP-only | Fully omitted for non-ILP base products |
| BR-US004-07 | Vietnamese number formatting | Period as thousands separator |
| BR-US004-08 | Output quality floor | ≥150 DPI, ≤5MB, A4-printable before delivery |

### 3. Trigger

A **Priced, budget-fit Package** is available from US-003 (conversational trigger: "Xem minh họa PDF").

### 4. Pre / Post-condition

- **Pre-condition:** US-003 has produced a complete priced package (per-member products/riders +
  premiums + budget-fit status) for MI and up to 3 OI.
- **Post-condition:** A downloadable PDF illustration document covering the entire family in a single
  file is available to the FA.

### 5. AC Scenario Flow

| AC | Type | Given | When | Then |
|---|---|---|---|---|
| AC1 | Happy Path | Complete priced package; FA requests the PDF | PDF generation runs | Cover page shows logo, title, PO role/age/gender (no real name), FA name/agent code, date, disclaimer; no PII beyond role/age |
| AC2 | Happy Path | Priced package with MI + N OI (N=1,2,3) | Family Coverage Map is generated | MI centered with base product/SA; each OI as a spoke; PO shown separately if PO≠MI; layout adjusts per N, greyscale-legible |
| AC3 | Happy Path | Priced package with per-member premium data | Premium Overview section is generated | Donut chart (≤5 segments, none under 5%) with total premium; dual timeline bar annotates payment vs. protection term |
| AC4 | Happy Path | Each insured's finalized rider list | Per-Member Detail Cards section is generated | Header card + benefit chips per insured; no empty rider list; MI's value table ≥5 rows |
| AC5 | Happy Path | Finalized priced package | Premium Summary Table section is generated | Member/Role/Product/Annual/Monthly columns, subtotal per member, bolded grand total, budget-fit status, footnote |
| AC6 | Validation | Illustration table data (PY 1 → maturity) | Policy Value Chart is rendered (ILP only) | Strictly ascending PY order incl. milestones; high/low lines never swap |
| AC7 | Edge Case | Base product is ILP | Fund Allocation Chart section is generated | One slice per fund summing to 100%, consistent with chat; fully omitted (not empty) if base product is not ILP |
| AC8 | Validation | Finalized PDF content | Document is rendered | All body text Vietnamese with bilingual technical terms; full diacritic support, no encoding errors |
| AC9 | Negative Case | Priced package uses a product/rider/member-count combo with no compliance-approved template | FA requests the illustration PDF | Does not fall back to an unapproved layout — blocks generation, tells the FA it's unsupported |
| AC10 | Happy Path | FA requests the PDF for up to 4 insured | Generation begins | Progress indicator shown; downloadable PDF ready within 30 seconds |
| AC11 | Exception Case | PDF generation in progress | Exceeds 30s or fails | Friendly error message with a "Thử lại" retry option; never returns a partial/corrupted PDF |
| AC12 | Validation | Any successfully generated PDF | File is produced | ≥150 DPI, ≤5MB, A4-printable, header/footer on every page |
| AC13 | Integration Failure | Data handed off from US-003 is missing/inconsistent | PDF generation is attempted | Never renders blank/placeholder/fabricated values — blocks generation, tells the FA to finalize the package in chat first |
