# 04 — Reflection cá nhân

**Học viên:** Ngô Hải Văn · 2A202600386
**Product được analyze:** GradeAI
**Deadline:** 24h sau lớp · 10 điểm · 150-200 từ
**Prompt:** *"1 chỉ số hoặc 1 giả định về áp dụng AI tôi sẽ sửa là gì? Vì sao?"*

> Yêu cầu: pick 1 metric trong dashboard, giải thích cơ chế vanity/cheat, đề xuất prevention concrete, cite 1 case từ Block 2.

---

## Pre-draft (sửa sau khi nghe red-team)

**Chỉ số sẽ sửa:** *"# GV active dùng GradeAI ≥2 đợt kiểm tra/quý"* (KR1 product-level)

**Vì sao đây là vanity/cheat (lesson từ WeFit):**
KR1 hiện đếm GV mở app + grade ≥1 bài trong 2 đợt. Vấn đề: GV champion (top 10) có thể grade rất nhiều bài lấp liếm mass-market GV chỉ thử 1 bài rồi bỏ — y hệt WeFit pattern *10% user dùng 80% lượt*. Dashboard sẽ xanh ở 200 GV nhưng unit economics có thể đỏ: nếu power-user là champion miễn phí + mass-market churn 15%, MRR thực < 30M (target 40M) và CAC payback dài ra.

**Sửa thành (composite + per-unit):**
- **Outcome**: % GV qua đợt-1 → ĐỢT-2 active (cohort retention) ≥ 60%, KHÔNG đếm champion cohort
- **Per-unit**: Contribution margin per active GV = (ARPU 200K − COGS 75K − allocated CAC 300K/payback 2.4mo) ≥ +50K VND
- **Quality gate**: Sean Ellis ≥ 40% "very disappointed without GradeAI" trên cohort mass-market

**Cơ chế prevention:**
- Tách cohort champion / mass-market trong dashboard
- "% GV active không tính champion" + "Mass-market retention curve" phải bằng/hơn champion
- KR1 không hiển thị tổng count mà hiển thị **distribution** (top-10/median/long-tail)

**Case evidence:**
WeFit đo lượt check-in tăng đều nhưng power-user 10% phá unit economics → mỗi check-in lỗ → phá sản. Áp vào GradeAI: nếu chỉ đo tổng GV active mà không tách cohort, champion lấp liếm mass-market churn → grow đẹp 90 ngày nhưng pivot bắt buộc Quý 2 vì MRR không catch up với burn.

---

## Anti-pattern cần tránh

- KHÔNG copy-paste reflection giữa thành viên → 0đ tất cả người liên quan
- KHÔNG viết "đã review lại" / "sẽ training thêm" mà không có thay đổi metric cụ thể
- PHẢI cite case từ Block 2 (WeFit hoặc MoMo)
- PHẢI có *prevention mechanism* cụ thể (ai own, đo bằng gì, threshold action)

---

## Final draft slot (sẽ chốt sau Block 5)

```markdown
[Viết final 150-200 từ ở đây, trong 24h sau lớp.
Sửa pre-draft trên dựa câu hỏi red-team đã thực sự nhận được.]
```
