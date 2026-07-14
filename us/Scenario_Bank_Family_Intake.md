# Scenario Bank — Family Intake & Role Resolution (US-001)

> Format standard: `.kombai/Agent_Document/BA.md` §4 (Input Scenario Collection & Classification Technique).
> Source: extracted and reformatted from `user-story-family-quick-pitch.html` §"Realistic Agent Prompts &
> Role Mapping" — no new scenario content invented in this pass.
> Consumed by: **US-001** NFR1 — *"LLM role/field extraction must achieve ≥ 95% correct PO/MI/OI
> identification on a 50-scenario Vietnamese test set before this story is considered done."*
> Owned jointly by PO (`.kombai/Agent_Document/PO.md`) and BA (`.kombai/Agent_Document/BA.md`) per §4/§9 of
> their respective agent docs — PO validates business-rule coverage, BA validates AC-testability.

## Status

- **8 of 50** scenarios present (this file). The 50-scenario NFR1 threshold is **not yet met** — this file
  is the foundation, not the final test set.
- **Known gap: C2 is missing.** The source HTML jumps from C1 directly to C3; no C2 (second
  agent-directed-rider scenario) currently exists. Flagging back to PO/BA per BA.md §9.3 rather than
  inventing one silently.
- Each scenario below satisfies the 3 mandatory parts per BA.md §4: 💬 Agent Message, 🗂 Parsed Role
  Mapping, ⚠️ Parsing Challenge.

---

## Group A — PO = Main Insured (1–3 additional profiles) — Happy Path

### A1. Married Couple — Husband PO/MI, Wife as Other Insured

*2 profiles · Savings + Protection · Budget stated directly*

**💬 Agent Message**
> Khách hàng là nam 38t, thu nhập khoảng 50tr/tháng
> muốn mua bảo hiểm cho cả 2 vợ chồng, vợ 35t nữ
> ngân sách tổng 80 triệu, đóng 15 năm,
> quan tâm tiết kiệm + bảo vệ, rủi ro trung bình.
> Gợi ý giải pháp.

**🗂 Parsed Role Mapping**

| Role | Profile | Age / Gender | Source in Message |
|---|---|---|---|
| PO = MI (Main Insured) | Chủ HĐ / NĐBH chính | Nam, 38t | "nam 38t" — PO=MI implicit (no flip signal) |
| Other Insured 1 | Vợ (Wife) | Nữ, 35t | "vợ 35t nữ" |
| Budget | — | 80 triệu | "ngân sách tổng 80 triệu" |
| Payment Term | — | 15 năm | "đóng 15 năm" |
| Product Focus | — | Savings + Protection | "quan tâm tiết kiệm + bảo vệ" |
| Risk Appetite | — | Trung bình (Medium) | "rủi ro trung bình" |

**⚠️ Parsing Challenge**
Message states both income (50tr/tháng) and budget (80 triệu) — per BA.md §3 rule 1, **Budget must win** as
the basis for the recommendation, income is context only. PO=MI must be assumed by default since there is
no explicit flip signal (BA.md §3 rule 2).

---

### A2. Family of 4 — PO/MI + Wife + 2 Children (max OI)

*4 profiles · CI riders for all · Per-person product mix*

**💬 Agent Message**
> KH nam 42t, có 2 con: 1 bé 8t nam, 1 bé 5t nữ, vợ 39t
> muốn bảo vệ toàn bộ gia đình 4 người trong 1 hợp đồng
> ngân sách khoảng 150 triệu, đóng 20 năm
> ưu tiên bảo vệ bệnh hiểm nghèo cho tất cả, savings cho anh chồng
> risk appetite vừa

**🗂 Parsed Role Mapping**

| Role | Profile | Age / Gender | Source in Message |
|---|---|---|---|
| PO = MI (Main Insured) | Chủ HĐ / NĐBH chính | Nam, 42t | "KH nam 42t" |
| OI-1 | Vợ (Wife) | Nữ, 39t | "vợ 39t" |
| OI-2 | Con trai (Son 1) | Nam, 8t | "1 bé 8t nam" |
| OI-3 | Con gái (Daughter) | Nữ, 5t | "1 bé 5t nữ" |
| Budget | — | ~150 triệu total (4 people) | "ngân sách khoảng 150 triệu" |
| Product Base | — | Savings (tiết kiệm) | "savings cho anh chồng" |
| Riders — all 4 | — | CI (bệnh hiểm nghèo) | "ưu tiên bảo vệ bệnh hiểm nghèo cho tất cả" |
| Risk | — | Medium | "risk appetite vừa" |

**⚠️ Parsing Challenge**
Hits the **max-3-OI-per-policy limit exactly** (wife + 2 children = 3 OI) — must be accepted, not truncated
or over-flagged (BA.md §3 rule 3 / US-001 BR-US001-04).

---

### A3. Couple + Infant (8 months old) — Age-in-Months Edge Case

*3 profiles · ILP base · Infant rider eligibility*

**💬 Agent Message**
> kh của e: chồng 30t nam, vợ 28t nữ, có 1 con nhỏ 8 tháng tuổi bé trai
> muốn làm bảo hiểm cho cả nhà, chồng là chính, vợ + con đi kèm
> budget 6tr/th, 10 năm đóng phí, khẩu vị rủi ro cao,
> chồng focus Xanh Tương Lai Phí ổn định, vợ + con thêm rider bảo vệ thôi
> k cần save nhiều, chủ yếu bảo vệ rủi ro cho gia đình

**🗂 Parsed Role Mapping**

| Role | Profile | Age / Gender | Source in Message |
|---|---|---|---|
| PO = MI | Chồng (Husband) | Nam, 30t | "chồng 30t nam", "chồng là chính" |
| OI-1 | Vợ (Wife) | Nữ, 28t | "vợ 28t nữ" |
| OI-2 | Con trai (Son) | Nam, 8 tháng (~0.67 tuổi) | "8 tháng tuổi bé trai" |
| Budget | — | 6tr/tháng | "budget 6tr/th" |
| Base — PO/MI | — | Xanh Tương Lai Phí ổn định | "chồng focus Xanh Tương Lai Phí ổn định" |
| Riders — Wife + Child | — | Protection riders only, no base product | "vợ + con thêm rider bảo vệ thôi" |
| Risk Appetite | — | Cao | "khẩu vị rủi ro cao" |

**⚠️ Parsing Challenge**
**Age in months must convert to a decimal age** (8 tháng ≈ 0.67 tuổi) and be validated against the child
rider's minimum entry age (BA.md §3 rule 4 / US-001 BR-US001-08). **Dual product intent on one policy**: ILP
base for MI only, protection-only riders for wife/child — must not generate a separate base policy for
either (BA.md §3 rule 6).

---

## Group B — PO ≠ Main Insured (2–4 profiles) — Role-Related Edge Case

### B1. Husband PO, Wife is Main Insured + 2 Teen Children as Other Insured

*4 profiles · PO ≠ MI · PWPO rider (explicit)*

**💬 Agent Message**
> KH là chồng 45t muốn mua bảo hiểm tiết kiệm cho vợ 42t
> chồng là người đóng phí, vợ là NĐBH chính
> có 2 con: con lớn 16t nam, con nhỏ 12t nữ — muốn thêm vào cùng hợp đồng
> ngân sách 12tr/tháng, đóng 15 năm, tiết kiệm là chính, khẩu vị rủi ro vừa

**🗂 Parsed Role Mapping**

| Role | Profile | Age / Gender | Source in Message |
|---|---|---|---|
| Policy Owner (PO) | Chồng (Husband) — **≠ MI** | Nam, 45t | "chồng là người đóng phí" |
| Main Insured (MI) | Vợ (Wife) — **different from PO** | Nữ, 42t | "vợ là NĐBH chính" |
| OI-1 | Con lớn (Older child) | Nam, 16t | "con lớn 16t nam" |
| OI-2 | Con nhỏ (Younger child) | Nữ, 12t | "con nhỏ 12t nữ" |
| Budget | — | 12tr/tháng | "ngân sách 12tr/tháng" |
| Product Base | — | Savings / ILP | "tiết kiệm là chính" |
| Riders | — | Attached for both kids + MI | "muốn thêm vào cùng hợp đồng" |

**⚠️ Parsing Challenge**
**PO ≠ MI must be detected and conversationally confirmed** before any eligibility calculation runs
(US-001 R.03/AC5) — eligibility parameters must anchor to **MI's (wife's) age**, not PO's (BA.md §3 rule 2,
US-001 BR-US001-06). Budget must be allocated across 4 insured, not applied to one person.

---

### B2. Parent Pays, Child is Main Insured — Education + Protection Plan

*3 profiles · PO = Parent · Child MI · Critical pronoun trap*

**💬 Agent Message**
> khách hàng là bố 40t mua cho con gái 10t
> bố là PO, con là NĐBH chính
> bố muốn để dành cho con đi học sau này + bảo vệ
> có thêm bé trai 7t là em, cũng muốn thêm vào
> budget khoảng 3tr/tháng, nộp 10 năm thôi vì bố bận
> rủi ro thấp, k muốn đầu tư nhiều

**🗂 Parsed Role Mapping**

| Role | Profile | Age / Gender | Source in Message |
|---|---|---|---|
| Policy Owner (PO) | Bố (Father) — **≠ MI** | Nam, 40t | "bố 40t", "bố là PO" |
| Main Insured (MI) | Con gái (Daughter) — **child MI** | Nữ, 10t | "bố là PO, con là NĐBH chính" |
| OI-1 ⚠️ | Con trai (Son, younger sibling of MI) | Nam, 7t | "bé trai 7t là **em**" → "em" = younger sibling, **not the agent** |
| Budget | — | 3tr/tháng | "budget khoảng 3tr/tháng" |
| Product | — | Education savings + protection (child-eligible only) | "để dành cho con đi học sau này" |
| Risk Appetite | — | Thấp (Low) | "rủi ro thấp, k muốn đầu tư nhiều" |

**⚠️ Parsing Challenge — 🔴 CRITICAL Pronoun Collision**
*"bé trai 7t là em"* — here "em" means **younger sibling of the MI (daughter)**, not the FA/agent
themselves. If the LLM parses "em" as referring to the agent, OI-1 is silently lost — this is the
highest-severity parsing failure mode called out in US-001 §4.3 (Impact and Risk). Also: **child MI at 10
tuổi** rules out adult-only base products (e.g. RUV14, min age 18) — only child-eligible plan codes apply
(US-001 R.06).

---

### B3. Adult Daughter Pays, Elderly Mother is MI — Elder Care + Husband Savings

*3 profiles · PO ≠ MI · MI age boundary · Per-OI product exclusion*

**💬 Agent Message**
> KH là con gái 27t, nữ
> mua bảo hiểm bảo vệ cho mẹ 50t
> con gái là PO đóng phí, mẹ là người được bảo hiểm chính
> chồng con gái 34t nam cũng muốn thêm vào để tiết kiệm cho anh ấy
> budget 5tr/th, 10 năm đóng, khẩu vị rủi ro trung bình
> mẹ chỉ cần bảo vệ thôi k cần tiết kiệm, bệnh lý nghiêm trọng, SKMN nội trú là đủ
> không cần ILP cho mẹ

**🗂 Parsed Role Mapping**

| Role | Profile | Age / Gender | Source in Message |
|---|---|---|---|
| Policy Owner (PO) | Con gái (Daughter) — **≠ MI** | Nữ, 27t | "con gái là PO đóng phí" |
| Main Insured (MI) | Mẹ (Mother) — **elder MI, age boundary** | Nữ, 50t | "mẹ là người được bảo hiểm chính" |
| OI-1 | Chồng của PO (PO's husband — nested relationship) | Nam, 34t | "chồng con gái 34t nam" |
| Budget | — | 5tr/tháng | "budget 5tr/th" |
| Product — MI | — | Protection only · **ILP excluded for MI only** | "không cần ILP cho mẹ" |
| Product — OI-1 | — | Savings — husband can still have ILP | "muốn thêm vào để tiết kiệm cho anh ấy" |
| Riders — MI | — | CI + RHC8I | "bệnh lý nghiêm trọng, SKMN nội trú" |

**⚠️ Parsing Challenge**
**Product exclusion is scoped to MI only**: "không cần ILP cho mẹ" must block ILP for the mother without
generalizing it to OI-1 (the husband), who still wants ILP savings (BA.md §3 rule 5 / US-001 R.03 in
US-002). **Nested relationship graph**: "chồng con gái" = the PO's own husband, not the MI's son-in-law
(BA.md §3 rule 7). MI age (50t) approaches the eligibility boundary for several products (US-001 R.06,
adult-child/elderly-parent branch).

---

## Group C — Agent-Directed Riders (FA explicitly specifies product/rider)

### C1. ILP Base + Per-Person Rider Matrix with Different Amounts

*3 profiles · CI/TPD/HCB per member · "200k/ngày" numeric trap*

**💬 Agent Message**
> tạo giải pháp:
> - chồng 36t nam: Xanh Tương Lai 20 năm, thêm BLNT gd cuối 500tr + TTTBVV 500tr
> - vợ 33t nữ: rider BLNT cuối và sớm, k cần TPD
> - con 6t nam: rider SKMN 200k/ngày
> budget tổng khoảng 60 triệu
> chồng là PO + NĐBH chính, vợ + con là OI
> khẩu vị rủi ro cao.

**🗂 Parsed Role Mapping + Rider Matrix**

| Role | Age / Gender | Product | Riders | Coverage / Amount |
|---|---|---|---|---|
| PO = MI (Chồng) | Nam, 36t | ILP tiết kiệm, 20 năm | CI + TPD | CIL 500tr & TPD 500tr |
| OI-1 (Vợ) | Nữ, 33t | Rider only | CI only (**TPD excluded**) | CIL, CIE |
| OI-2 (Con) | Nam, 6t | Rider only | RHC8I (nội trú) | 200.000 VND/ngày |
| Budget | — | — | — | 60 triệu total |
| Risk (ILP) | — | — | — | Medium |

**⚠️ Parsing Challenge — 🔴 CRITICAL Numeric Unit Trap**
*"HCB 200k/ngày"*: "200k" = 200 **nghìn** (thousand) VND/day = **200.000 VND/ngày**. Not 200 VND, not 200
triệu — the most common numeric misparse in Vietnamese insurance chat (US-002 BR-US002-05). *"k cần TPD"*
is an **OI-1-scoped exclusion only** — must not carry over to any other member (BA.md §3 rule 5).

---

### ~~C2~~ — Missing from source

No C2 scenario exists in `user-story-family-quick-pitch.html` (numbering jumps from C1 to C3). Reported as
an open gap rather than fabricated — PO/BA should decide the intended C2 theme (e.g. an agent-directed
**base-product swap** or **rider exclusion with dependency conflict**, per US-002 R.09/R.10, neither of
which currently has a natural-language scenario example).

---

### C3. Mixed Products — ILP for Father + Term Life for Wife (Not ILP) + 2 Children

*4 profiles · Dual budget signals · Per-OI product exclusion · Education maturity*

**💬 Agent Message**
> gia đình 4 người:
> - bố 38t nam: ILP tiết kiệm 20 năm, khẩu vị rủi ro vừa, phí khoảng 6-8tr/th
>   thêm CI 500tr cho bố
> - mẹ 35t nữ: term life 500tr/20 năm (k phải ILP nha), CI 300tr
> - con trai 9t: rider giáo dục + CI 200tr (để sau này 18t con lấy tiền học)
> - con gái 5t: CI 150tr + HCB 150k/ngày
>
> bố là PO. bố + mẹ + 2 con đều là insured trong cùng 1 hợp đồng
> tổng budget 15tr/th, đóng 20 năm
> gợi ý sản phẩm phù hợp cho từng người nhé

**🗂 Parsed Role Mapping + Product Matrix**

| Role | Age / Gender | Product | Riders | Special |
|---|---|---|---|---|
| PO = MI (Bố) | Nam, 38t | ILP tiết kiệm, 20 năm | CI 500tr | Sub-budget: 6–8tr/th |
| OI-1 (Mẹ) | Nữ, 35t | **Term Life 500tr / 20 năm (not ILP)** | CI 300tr | Hard product exclusion for OI-1 only |
| OI-2 (Con trai) | Nam, 9t | Education rider | CI 200tr | Maturity at age 18 → 9 years from now |
| OI-3 (Con gái) | Nữ, 5t | Riders only | CI 150tr + HCB 150.000 VND/ngày | — |

**⚠️ Parsing Challenge**
*"k phải ILP nha"* is a **hard product exclusion scoped to OI-1 (mẹ) only** — the father still gets ILP
(BA.md §3 rule 5). **Dual budget signals**: "phí khoảng 6-8tr/th" for the father's ILP portion vs. "tổng
budget 15tr/th" for all 4 members — both must be preserved, remaining ~7–9tr/th applies to mẹ + 2 con.
**Education rider maturity**: "18t con lấy tiền học" with a 9-year-old child implies a 9-year rider term —
must be computed, not left as a vague label.

---

## Parsing Failure Risk Matrix

| Risk Type | Scenarios | Severity |
|---|---|---|
| PO = MI wrongly assumed when PO ≠ MI | B1, B2, B3 | 🔴 CRITICAL |
| "em" parsed as the agent instead of a sibling (pronoun collision) | B2 | 🔴 CRITICAL |
| "200k/ngày" parsed as 200 VND instead of 200.000 VND | C1, C3 | 🔴 CRITICAL |
| Budget stated as a range/total for all persons, misapplied per-person | A1, C1, C3 | ⚠️ HIGH |
| Age in months not converted to decimal | A3 | ⚠️ HIGH |
| Per-person rider/product exclusion applied globally | B3, C1, C3 | ⚠️ HIGH |
| Sub-budget vs. total-budget conflict | C3 | ⚠️ HIGH |
| Child MI → adult-only product recommended (e.g. RUV14) | B2 | ⚠️ HIGH |
| Education rider maturity term miscalculated | C3 | 🔵 MEDIUM |

## Coverage Check Against BA.md §3 Business Rules

| Rule (BA.md §3) | Illustrated in |
|---|---|
| 1. Budget over Income | A1 |
| 2. PO = MI by default / explicit flip only | A1 (default), B1/B2/B3 (flip) |
| 3. Max 3 OI per policy | A2 |
| 4. Age in months → decimal | A3 |
| 5. Narrowly-scoped exclusions | B3, C1, C3 |
| 6. Dual product intent, single policy | A3, B1 |
| 7. Nested relationships | B3 |
| 8. Age-boundary product filtering | B3 |
| 9. Budget overflow trimming | *not yet illustrated — needs a new scenario* |
| 10. License Gate (MIT) | *not yet illustrated — needs a new scenario* |
| 11. Minimal ask-back | *not yet illustrated — needs a new scenario* |
| 12. Vietnamese formatting | all scenarios (implicit) |

**Open items for the next PO/BA pass:** add scenarios for rules 9–11 above, resolve the missing C2, and
continue extending toward the 50-scenario NFR1 threshold.
