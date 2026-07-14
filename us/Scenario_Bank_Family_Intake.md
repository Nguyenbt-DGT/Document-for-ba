# Scenario Bank — Family Intake & Role Resolution (US-001)

> 15 realistic Vietnamese agent-input scenarios for AI dev/training. Each scenario has the raw agent
> message, the parsed role mapping, and the detected required fields (per US-001 R.01 Mandatory
> Information Matrix: Age/DOB, Gender, Occupation class, Relationship to PO, and for PO only —
> Income/Budget, Risk appetite, Payment term).

---

## Group A — PO = Main Insured (Happy Path)

### A1. Married Couple — Husband PO/MI, Wife as Other Insured

**💬 Agent Message**
> Khách hàng là nam 38t, thu nhập khoảng 50tr/tháng
> muốn mua bảo hiểm cho cả 2 vợ chồng, vợ 35t nữ
> ngân sách tổng 80 triệu, đóng 15 năm,
> quan tâm tiết kiệm + bảo vệ, rủi ro trung bình.
> Gợi ý giải pháp.

**🗂 Parsed Role Mapping**

| Role | Profile | Age / Gender | Source in Message |
|---|---|---|---|
| PO = MI | Chủ HĐ / NĐBH chính | Nam, 38t | "nam 38t" — PO=MI implicit (no flip signal) |
| OI-1 | Vợ (Wife) | Nữ, 35t | "vợ 35t nữ" |

**📋 Detected Required Fields**

| Member | Age/DOB | Gender | Relationship to PO | Occupation Class | Budget / Risk / Term (PO only) | Status |
|---|---|---|---|---|---|---|
| PO/MI | 38t | Nam | — (is PO) | 1 (default) | 80 triệu / Trung bình / 15 năm | Đủ |
| OI-1 (Vợ) | 35t | Nữ | Vợ | 1 (default) | — | Đủ |

---

### A2. Family of 4 — PO/MI + Wife + 2 Children (max OI)

**💬 Agent Message**
> KH nam 42t, có 2 con: 1 bé 8t nam, 1 bé 5t nữ, vợ 39t
> muốn bảo vệ toàn bộ gia đình 4 người trong 1 hợp đồng
> ngân sách khoảng 150 triệu, đóng 20 năm
> ưu tiên bảo vệ bệnh hiểm nghèo cho tất cả, savings cho anh chồng
> risk appetite vừa

**🗂 Parsed Role Mapping**

| Role | Profile | Age / Gender | Source in Message |
|---|---|---|---|
| PO = MI | Chủ HĐ / NĐBH chính | Nam, 42t | "KH nam 42t" |
| OI-1 | Vợ (Wife) | Nữ, 39t | "vợ 39t" |
| OI-2 | Con trai (Son) | Nam, 8t | "1 bé 8t nam" |
| OI-3 | Con gái (Daughter) | Nữ, 5t | "1 bé 5t nữ" |

**📋 Detected Required Fields**

| Member | Age/DOB | Gender | Relationship to PO | Occupation Class | Budget / Risk / Term (PO only) | Status |
|---|---|---|---|---|---|---|
| PO/MI | 42t | Nam | — (is PO) | 1 (default) | ~150 triệu / Vừa / 20 năm | Đủ |
| OI-1 (Vợ) | 39t | Nữ | Vợ | 1 (default) | — | Đủ |
| OI-2 (Con trai) | 8t | Nam | Con | 2 (default, <18) | — | Đủ |
| OI-3 (Con gái) | 5t | Nữ | Con | 2 (default, <18) | — | Đủ — exactly 3 OI (limit reached, not exceeded) |

---

### A3. Couple + Infant (8 months old) — Age-in-Months Edge Case

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
| OI-2 | Con trai (Son) | Nam, 8 tháng | "8 tháng tuổi bé trai" |

**📋 Detected Required Fields**

| Member | Age/DOB | Gender | Relationship to PO | Occupation Class | Budget / Risk / Term (PO only) | Status |
|---|---|---|---|---|---|---|
| PO/MI | 30t | Nam | — (is PO) | 1 (default) | 6tr/tháng / Cao / 10 năm | Đủ |
| OI-1 (Vợ) | 28t | Nữ | Vợ | 1 (default) | — | Đủ |
| OI-2 (Con trai) | 8 tháng → **0.67 tuổi (converted)** | Nam | Con | 2 (default, <18) | — | Đủ — age-in-months converted to decimal |

---

### A4. Couple + Twin Children (Same Age) — Distinct Line Items Required

**💬 Agent Message**
> kh nam 34t, vợ 31t nữ, có cặp song sinh 3 tuổi 1 trai 1 gái
> muốn mua bảo hiểm tiết kiệm cho cả nhà, chồng là PO+NĐBH chính
> ngân sách 10tr/tháng, đóng 18 năm, khẩu vị rủi ro trung bình
> rider bảo vệ cho vợ và 2 bé như nhau

**🗂 Parsed Role Mapping**

| Role | Profile | Age / Gender | Source in Message |
|---|---|---|---|
| PO = MI | Chồng (Husband) | Nam, 34t | "kh nam 34t", "chồng là PO+NĐBH chính" |
| OI-1 | Vợ (Wife) | Nữ, 31t | "vợ 31t nữ" |
| OI-2 | Con (song sinh — trai) | Nam, 3t | "cặp song sinh 3 tuổi 1 trai 1 gái" |
| OI-3 | Con (song sinh — gái) | Nữ, 3t | "cặp song sinh 3 tuổi 1 trai 1 gái" |

**📋 Detected Required Fields**

| Member | Age/DOB | Gender | Relationship to PO | Occupation Class | Budget / Risk / Term (PO only) | Status |
|---|---|---|---|---|---|---|
| PO/MI | 34t | Nam | — (is PO) | 1 (default) | 10tr/tháng / Trung bình / 18 năm | Đủ |
| OI-1 (Vợ) | 31t | Nữ | Vợ | 1 (default) | — | Đủ |
| OI-2 (Con — song sinh 1) | 3t | Nam | Con | 2 (default, <18) | — | Đủ — must render as its own line item |
| OI-3 (Con — song sinh 2) | 3t | Nữ | Con | 2 (default, <18) | — | Đủ — same age as OI-2, must NOT be merged into one doubled entry |

---

### A5. Spouse + 3 Children (4 OI) — Over-Limit, Prioritization Needed

**💬 Agent Message**
> a có vợ 36t và 3 con: bé lớn 10t nam, bé giữa 7t nữ, bé út 3t nam
> muốn mua chung 1 hợp đồng bảo vệ cả nhà, a là PO+NĐBH chính, 40t
> ngân sách 9tr/tháng, đóng 15 năm, rủi ro thấp

**🗂 Parsed Role Mapping**

| Role | Profile | Age / Gender | Source in Message |
|---|---|---|---|
| PO = MI | Bản thân (Self) | Nam, 40t | "a là PO+NĐBH chính, 40t" |
| OI-1 | Vợ (Wife) | Nữ, 36t | "vợ 36t" |
| OI-2 | Con lớn | Nam, 10t | "bé lớn 10t nam" |
| OI-3 | Con giữa | Nữ, 7t | "bé giữa 7t nữ" |
| OI-4 (over limit) | Con út | Nam, 3t | "bé út 3t nam" |

**📋 Detected Required Fields**

| Member | Age/DOB | Gender | Relationship to PO | Occupation Class | Budget / Risk / Term (PO only) | Status |
|---|---|---|---|---|---|---|
| PO/MI | 40t | Nam | — (is PO) | 1 (default) | 9tr/tháng / Thấp / 15 năm | Đủ |
| OI-1 (Vợ) | 36t | Nữ | Vợ | 1 (default) | — | Đủ |
| OI-2 (Con lớn) | 10t | Nam | Con | 2 (default, <18) | — | Đủ |
| OI-3 (Con giữa) | 7t | Nữ | Con | 2 (default, <18) | — | Đủ |
| OI-4 (Con út) | 3t | Nam | Con | 2 (default, <18) | — | **Vượt hạn mức 3 OI — cần hỏi anh/chị ưu tiên 3 trong 4 người** |

---

## Group B — PO ≠ Main Insured (Role-Related Edge Cases)

### B1. Husband PO, Wife is Main Insured + 2 Teen Children as Other Insured

**💬 Agent Message**
> KH là chồng 45t muốn mua bảo hiểm tiết kiệm cho vợ 42t
> chồng là người đóng phí, vợ là NĐBH chính
> có 2 con: con lớn 16t nam, con nhỏ 12t nữ — muốn thêm vào cùng hợp đồng
> ngân sách 12tr/tháng, đóng 15 năm, tiết kiệm là chính, khẩu vị rủi ro vừa

**🗂 Parsed Role Mapping**

| Role | Profile | Age / Gender | Source in Message |
|---|---|---|---|
| PO | Chồng (Husband) — **≠ MI** | Nam, 45t | "chồng là người đóng phí" |
| MI | Vợ (Wife) — **different from PO** | Nữ, 42t | "vợ là NĐBH chính" |
| OI-1 | Con lớn (Older child) | Nam, 16t | "con lớn 16t nam" |
| OI-2 | Con nhỏ (Younger child) | Nữ, 12t | "con nhỏ 12t nữ" |

**📋 Detected Required Fields**

| Member | Age/DOB | Gender | Relationship to PO | Occupation Class | Budget / Risk / Term (PO only) | Status |
|---|---|---|---|---|---|---|
| PO | 45t | Nam | — (is PO) | 1 (default) | 12tr/tháng / Vừa / 15 năm | Đủ |
| MI | 42t | Nữ | Vợ | 1 (default) | — | Đủ — **PO≠MI, cần xác nhận hội thoại trước khi tính phí** |
| OI-1 (Con lớn) | 16t | Nam | Con | 1 (default, ≥18? — verify: <18 → class 2) | — | Đủ |
| OI-2 (Con nhỏ) | 12t | Nữ | Con | 2 (default, <18) | — | Đủ |

---

### B2. Parent Pays, Child is Main Insured — Education + Protection Plan

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
| PO | Bố (Father) — **≠ MI** | Nam, 40t | "bố 40t", "bố là PO" |
| MI | Con gái (Daughter) — **child MI** | Nữ, 10t | "bố là PO, con là NĐBH chính" |
| OI-1 | Con trai (Son, younger sibling **of the MI**) | Nam, 7t | "bé trai 7t là **em**" → "em" = younger sibling of the daughter, **not the agent** |

**📋 Detected Required Fields**

| Member | Age/DOB | Gender | Relationship to PO | Occupation Class | Budget / Risk / Term (PO only) | Status |
|---|---|---|---|---|---|---|
| PO | 40t | Nam | — (is PO) | 1 (default) | 3tr/tháng / Thấp / 10 năm | Đủ |
| MI | 10t | Nữ | Con | 2 (default, <18) | — | Đủ — child-eligible products only |
| OI-1 (Con trai) | 7t | Nam | Con (em của MI) | 2 (default, <18) | — | Đủ — **must not be dropped by misreading "em" as the agent** |

---

### B3. Adult Daughter Pays, Elderly Mother is MI — Elder Care + Husband Savings

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
| PO | Con gái (Daughter) — **≠ MI** | Nữ, 27t | "con gái là PO đóng phí" |
| MI | Mẹ (Mother) — **elder MI** | Nữ, 50t | "mẹ là người được bảo hiểm chính" |
| OI-1 | Chồng của PO (PO's husband — nested relationship) | Nam, 34t | "chồng con gái 34t nam" |

**📋 Detected Required Fields**

| Member | Age/DOB | Gender | Relationship to PO | Occupation Class | Budget / Risk / Term (PO only) | Status |
|---|---|---|---|---|---|---|
| PO | 27t | Nữ | — (is PO) | 1 (default) | 5tr/tháng / Trung bình / 10 năm | Đủ |
| MI | 50t | Nữ | Mẹ | 1 (default) | — | Đủ — ILP excluded for MI only, protection products only |
| OI-1 (Chồng của PO) | 34t | Nam | Chồng của PO (nested — not MI's son-in-law) | 1 (default) | — | Đủ — still eligible for ILP savings |

---

### B4. Ambiguous Relationship — Unnamed Person, No Stated Relationship

**💬 Agent Message**
> kh nam 33t muốn mua bảo hiểm cho gia đình
> có thêm 1 người nữa, 25 tuổi, cũng muốn tham gia
> ngân sách 4tr/tháng, đóng 12 năm, rủi ro vừa

**🗂 Parsed Role Mapping**

| Role | Profile | Age / Gender | Source in Message |
|---|---|---|---|
| PO = MI | Bản thân (Self) | Nam, 33t | "kh nam 33t" |
| Unresolved | Người thứ 2 (unnamed) | 25t, giới tính chưa rõ | "1 người nữa, 25 tuổi" — no relationship word, no gender word |

**📋 Detected Required Fields**

| Member | Age/DOB | Gender | Relationship to PO | Occupation Class | Budget / Risk / Term (PO only) | Status |
|---|---|---|---|---|---|---|
| PO/MI | 33t | Nam | — (is PO) | 1 (default) | 4tr/tháng / Vừa / 12 năm | Đủ |
| Unresolved person | 25t | **THIẾU** | **THIẾU — chưa xác định vợ/chồng hay con** | — | — | **Cần làm rõ trước khi thêm vào Family Profile: "Người 25 tuổi là vợ/chồng hay con của khách hàng ạ?"** |

---

### B5. PO States Age "17 sắp 18" — Minor PO Hard Block

**💬 Agent Message**
> e năm nay 17 tuổi sắp 18 rồi, muốn tự đứng tên mua bảo hiểm cho bản thân
> ngân sách 2tr/tháng, đóng 10 năm, rủi ro thấp

**🗂 Parsed Role Mapping**

| Role | Profile | Age / Gender | Source in Message |
|---|---|---|---|
| PO = MI (proposed) | Bản thân (Self) | 17 tuổi (**"sắp 18" ≠ 18**) | "e năm nay 17 tuổi sắp 18 rồi" |

**📋 Detected Required Fields**

| Member | Age/DOB | Gender | Relationship to PO | Occupation Class | Budget / Risk / Term (PO only) | Status |
|---|---|---|---|---|---|---|
| PO/MI (proposed) | 17t (treated as 17, not 18) | Chưa nêu | — (is PO) | 1 (default) | 2tr/tháng / Thấp / 10 năm | **Chặn — BMBH phải đủ 18 tuổi; cần cung cấp ngày sinh chính xác để xác nhận** |

---

## Group C — Agent-Directed Product / Rider Control

### C1. ILP Base + Per-Person Rider Matrix with Different Amounts

**💬 Agent Message**
> tạo giải pháp:
> - chồng 36t nam: Xanh Tương Lai 20 năm, thêm BLNT gd cuối 500tr + TTTBVV 500tr
> - vợ 33t nữ: rider BLNT cuối và sớm, k cần TPD
> - con 6t nam: rider SKMN 200k/ngày
> budget tổng khoảng 60 triệu
> chồng là PO + NĐBH chính, vợ + con là OI
> khẩu vị rủi ro cao.

**🗂 Parsed Role Mapping**

| Role | Profile | Age / Gender | Source in Message |
|---|---|---|---|
| PO = MI | Chồng (Husband) | Nam, 36t | "chồng là PO + NĐBH chính" |
| OI-1 | Vợ (Wife) | Nữ, 33t | "vợ 33t nữ" |
| OI-2 | Con | Nam, 6t | "con 6t nam" |

**📋 Detected Required Fields**

| Member | Age/DOB | Gender | Relationship to PO | Occupation Class | Budget / Risk / Term (PO only) | Status |
|---|---|---|---|---|---|---|
| PO/MI | 36t | Nam | — (is PO) | 1 (default) | 60 triệu / Cao / 20 năm | Đủ — riders: CI 500tr, TPD 500tr |
| OI-1 (Vợ) | 33t | Nữ | Vợ | 1 (default) | — | Đủ — riders: CI only, **TPD excluded (scoped to OI-1 only)** |
| OI-2 (Con) | 6t | Nam | Con | 2 (default, <18) | — | Đủ — rider: HCB **200.000 VND/ngày** (not 200 VND) |

---

### C2. Agent Names Explicit Base Product + Requests Rider Exclusion

**💬 Agent Message**
> dùng RUV10 cho tiết kiệm của a, 41t nam, đóng 20 năm
> thêm rider tai nạn ban đầu nhưng sau này nếu cần thì bỏ rider tai nạn đi, giữ WOP thôi
> vợ 38t nữ đi kèm rider CI 300tr
> ngân sách 8tr/tháng, rủi ro cao

**🗂 Parsed Role Mapping**

| Role | Profile | Age / Gender | Source in Message |
|---|---|---|---|
| PO = MI | Bản thân (Self) | Nam, 41t | "của a, 41t nam" |
| OI-1 | Vợ (Wife) | Nữ, 38t | "vợ 38t nữ" |

**📋 Detected Required Fields**

| Member | Age/DOB | Gender | Relationship to PO | Occupation Class | Budget / Risk / Term (PO only) | Status |
|---|---|---|---|---|---|---|
| PO/MI | 41t | Nam | — (is PO) | 1 (default) | 8tr/tháng / Cao / 20 năm | Đủ — **base product = RUV10 (theo yêu cầu agent)**, cần validate: sản phẩm còn active + agent đủ MIT + phù hợp tuổi |
| OI-1 (Vợ) | 38t | Nữ | Vợ | 1 (default) | — | Đủ — rider CI 300tr |
| — | — | — | — | — | — | **Rider request "bỏ rider tai nạn" cần cảnh báo trước: WOP phụ thuộc vào rider tai nạn, xoá sẽ mất cả WOP** |

---

### C3. Mixed Products — ILP for Father + Term Life for Wife (Not ILP) + 2 Children

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

**🗂 Parsed Role Mapping**

| Role | Profile | Age / Gender | Source in Message |
|---|---|---|---|
| PO = MI | Bố (Father) | Nam, 38t | "bố là PO" |
| OI-1 | Mẹ (Mother) | Nữ, 35t | "mẹ 35t nữ" |
| OI-2 | Con trai | Nam, 9t | "con trai 9t" |
| OI-3 | Con gái | Nữ, 5t | "con gái 5t" |

**📋 Detected Required Fields**

| Member | Age/DOB | Gender | Relationship to PO | Occupation Class | Budget / Risk / Term (PO only) | Status |
|---|---|---|---|---|---|---|
| PO/MI | 38t | Nam | — (is PO) | 1 (default) | 15tr/th tổng (sub-budget 6–8tr/th cho ILP) / Vừa / 20 năm | Đủ — **giữ cả 2 tín hiệu ngân sách (sub-budget + tổng)** |
| OI-1 (Mẹ) | 35t | Nữ | Mẹ | 1 (default) | — | Đủ — **loại trừ ILP chỉ áp dụng cho OI-1**, term life 500tr thay thế |
| OI-2 (Con trai) | 9t | Nam | Con | 2 (default, <18) | — | Đủ — rider giáo dục đáo hạn năm con 18t (9 năm tới) |
| OI-3 (Con gái) | 5t | Nữ | Con | 2 (default, <18) | — | Đủ — HCB **150.000 VND/ngày** |

---

### C4. Single Self-Only, Occupation Class 4 Volunteered + Explicit Amounts

**💬 Agent Message**
> kh nam 29t, độc thân, chưa có gia đình, tự mua bảo hiểm cho bản thân
> nghề nghiệp hạng 4 (thợ hàn), muốn CI 300 triệu, TPD 200 triệu
> ngân sách 3tr/tháng, đóng 15 năm, khẩu vị rủi ro cao

**🗂 Parsed Role Mapping**

| Role | Profile | Age / Gender | Source in Message |
|---|---|---|---|
| PO = MI | Bản thân (Self) | Nam, 29t | "kh nam 29t" |
| — | Không có vợ/con | — | "độc thân, chưa có gia đình" — valid configuration, do not prompt for spouse |

**📋 Detected Required Fields**

| Member | Age/DOB | Gender | Relationship to PO | Occupation Class | Budget / Risk / Term (PO only) | Status |
|---|---|---|---|---|---|---|
| PO/MI | 29t | Nam | — (is PO) | **4 (agent-volunteered, not default)** | 3tr/tháng / Cao / 15 năm | Đủ — **hiển thị cảnh báo không chặn**: "Nghề nghiệp hạng 4 — một số sản phẩm đính kèm có thể không áp dụng" |
| — | — | — | — | — | — | Rider amounts: CI 300 triệu, TPD 200 triệu — dùng đúng như agent nêu, không tự tính lại |

---

### C5. Incomplete Message — Spouse and Child Mentioned Without Age/Gender

**💬 Agent Message**
> kh nữ 35t, có chồng và 1 con nhỏ
> muốn mua bảo hiểm tiết kiệm cho cả nhà, chị là PO+NĐBH chính
> ngân sách 7tr/tháng, đóng 15 năm, rủi ro vừa

**🗂 Parsed Role Mapping**

| Role | Profile | Age / Gender | Source in Message |
|---|---|---|---|
| PO = MI | Bản thân (Self) | Nữ, 35t | "kh nữ 35t", "chị là PO+NĐBH chính" |
| OI-1 | Chồng (Husband) | Tuổi chưa nêu; giới tính Nam (suy ra từ "chồng") | "có chồng" |
| OI-2 | Con nhỏ (Child) | Tuổi và giới tính chưa nêu | "1 con nhỏ" |

**📋 Detected Required Fields**

| Member | Age/DOB | Gender | Relationship to PO | Occupation Class | Budget / Risk / Term (PO only) | Status |
|---|---|---|---|---|---|---|
| PO/MI | 35t | Nữ | — (is PO) | 1 (default) | 7tr/tháng / Vừa / 15 năm | Đủ |
| OI-1 (Chồng) | **THIẾU** | Nam (suy ra) | Chồng | 1 (default) | — | **Thiếu tuổi — cần hỏi thêm** |
| OI-2 (Con nhỏ) | **THIẾU** | **THIẾU** | Con | 2 (default, <18, chờ xác nhận) | — | **Thiếu tuổi + giới tính — hỏi gộp tối đa cùng lượt với OI-1, không hỏi lại thông tin đã có (PO đã đủ)** |

---

*15/15 scenarios (5 per group). Ready for review.*
