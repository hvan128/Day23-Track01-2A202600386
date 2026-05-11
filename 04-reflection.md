# 04 — Reflection cá nhân

**Học viên:** Ngô Hải Văn · 2A202600386
**Product:** GradeAI
**Block 5+24h · 10 điểm**
**Prompt:** *"1 chỉ số hoặc 1 giả định về áp dụng AI tôi sẽ sửa là gì? Vì sao?"*

---

## Final reflection

Chỉ số tôi sẽ sửa là **KR1 trong dashboard GradeAI** — "# giáo viên active dùng ≥2 đợt kiểm tra/quý" (target 200 GV trong 90 ngày).

Lúc draft v1 tôi cho đây là leading indicator tốt. Sau red-team mới thấy nó là vanity. KR1 đếm tổng số GV active mà không phân biệt champion (top 10 pilot, retention 80%) và mass-market (GV mới qua paid acquisition). Một dashboard 200 GV có thể che giấu: 30 champion × 90% retention + 170 mass-market × 30% retention = 153 — đẹp trên paper nhưng unit economics đỏ. CAC 300K của 170 mass-market đã đốt ~51M, MRR thật chỉ ~30M, payback kéo dài lê thê.

Đây chính xác là pattern **WeFit** — 10% user dùng 80% lượt, GMV và check-in tăng đều nhưng mỗi lượt lỗ → phá sản. Cho đến khi đọc case, tôi không nhìn ra cohort-split là vấn đề sống còn cho AI adoption.

Sửa: tách cohort. Dashboard hiển thị "champion retention" vs "mass-market retention" hai dòng riêng. KR1 chỉ PASS khi **mass-market cohort retention ≥ 60%**, KHÔNG cộng champion. Thêm distribution view (top10/median/long-tail) cho mọi metric Engagement. Decision rule: mass-market < 50% sau 60 ngày → pause paid acquisition, fix onboarding trước khi scale.
