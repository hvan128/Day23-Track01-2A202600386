# 02 — Bảng So Sánh Case Thành Công / Thất Bại

**Học viên:** Ngô Hải Văn · 2A202600386
**Nhóm:** [tên TBD] — Phan Thanh Sang, Ngô Hải Văn, Trần Tiến Dũng, Nguyễn Tiến Dũng, Nguyễn Nhị, Phạm Đan Kha
**Block 2 · Deliverable nhóm (mỗi thành viên copy vào repo cá nhân) · 15 điểm**
**Cặp case:** ✅ MoMo (success) ↔ ❌ WeFit / Onaclover (fail) — cả hai case Việt Nam
**Lý do chọn:** Cặp Việt Nam dạy *vanity metric trap* + *unit economics* trực tiếp hơn case nước ngoài. WeFit là sách giáo khoa của *growth metric đẹp + per-unit lỗ → phá sản*

---

## Bảng so sánh chính

| Trục | ✅ MoMo (success) | ❌ WeFit / Onaclover (fail) |
|---|---|---|
| **Domain** | Ví điện tử / super-app | Subscription gym/yoga (network model) |
| **Workflow** | User mở app → giao dịch (thanh toán, chuyển tiền, mua dịch vụ) → MoMo xử lý qua đối tác ngân hàng → trả kết quả + ghi nhận hành vi | User mua gói tháng tập gym/yoga không giới hạn → đặt lịch qua app → check-in tại phòng đối tác → WeFit trả tiền cho phòng tập theo từng lượt |
| **Metric công bố** | MAU, transaction volume, retention theo cohort, số dịch vụ user dùng trong app (cross-sell) | Số user đăng ký, GMV (tổng giá trị gói bán ra), tổng lượt check-in, số phòng tập đối tác |
| **Lớp metric** | Activation + Engagement + Retention + Value indicator | Chỉ Activation + Engagement (không có Value/Unit-economic layer) |
| **Prove** | User quay lại đều, dùng nhiều dịch vụ → ecosystem có dính, có cơ sở để cross-sell | Sản phẩm có demand thật, user dùng nhiệt tình, mạng lưới mở rộng nhanh, gọi vốn được |
| **NOT prove** | Lợi nhuận thật (MoMo lỗ nhiều năm liền); tỉ lệ giao dịch thành công khi hạ tầng ngân hàng đối tác lỗi | Lời/lỗ trên mỗi user. Càng user tích cực càng lỗ — chi phí trả phòng tập / lượt > doanh thu / lượt khi quy về tháng |
| **Missing metric** | Unit economics theo cohort; chi phí khuyến mãi cần để giữ chân 1 user active | **Unit economics**: cost-per-check-in vs revenue-per-check-in; phân bố lượt đi theo user (10% user dùng 80% lượt → kéo lỗ) |
| **Rủi ro lớn nhất** | Mở rộng dịch vụ nhưng không khoá được unit economics → lỗ vĩnh viễn dù scale to | Power-user (10% dùng 80% lượt) phá unit economics — càng tăng "engagement metric" càng lỗ. Đây là vanity metric thuần |
| **Bài học cho dashboard** | Cross-sell + retention là dấu hiệu tốt nhưng không đủ — phải đo cost-to-acquire/retain vs LTV | **MỌI growth metric phải có per-unit metric đi kèm.** Không có per-unit = đang nói dối dashboard |

---

## Map vào AI Adoption Stack (slide 10)

| Layer | MoMo | WeFit |
|---|---|---|
| 7 Value Measurement | Có đo *behavior + retention* nhưng **thiếu per-unit profitability** | Đo *volume* (GMV, check-in) mà **không** đo per-unit → vanity hoàn toàn |
| 8 Normalization | Đã thành "ví mặc định" với nhiều user → có Normalization về behavior | Chưa đạt — đóng cửa 2020 khi đốt hết tiền investor |

---

## Bài học cho Dashboard nhóm

### Bài học chính

> **WeFit là sách giáo khoa của vanity metric**: số user và GMV xanh rực trên dashboard, trong khi metric thật sự quan trọng — *lời/lỗ trên mỗi đơn vị giao dịch* — không được đo, hoặc bị giấu sau con số tổng.

### 3 Quy tắc áp dụng cho dashboard nhóm

1. **Mỗi metric tăng trưởng** (user, GMV, lượt dùng) **phải có metric đơn vị (per-unit)** đi kèm: doanh thu/user, chi phí/user, biên lợi nhuận/giao dịch
2. **Hỏi câu test:** *"Nếu metric này tăng gấp 10, công ty lời hơn hay lỗ nặng hơn?"* — nếu không trả lời được → metric đó chưa đủ
3. **Với mỗi metric, ghi rõ "không chứng minh được ___"** ngay cạnh để chống tự lừa bản thân

### Áp dụng cho dashboard nhóm (AI adoption context)

| Vanity metric AI (giống WeFit) | Per-unit metric đi kèm (như MoMo nên có) |
|---|---|
| "# user dùng AI / tuần" | "Cost per AI-assisted task COMPLETED & PASS review" |
| "Số PR có AI-assist" | "% PR AI-assist không bị rollback / không tạo bank-client issue trong 30d" |
| "Số lượng AI suggestion" | "Accept rate × downstream-quality rate (composite)" |
| "Tổng prompt count" | "Time saved per workflow × no-rework-rate" |
| "GMV-style: AI tokens consumed" | "Value delivered per token / cost per token" |

---

## Câu chốt của nhóm

```markdown
Case thành công (MoMo) dạy nhóm tôi rằng:
  Behavior metric (cross-sell, retention) là dấu hiệu tốt nhưng KHÔNG ĐỦ.
  Thiếu unit economics → có thể grow đẹp 5 năm mà vẫn lỗ vĩnh viễn.
  Áp dụng AI dashboard: adoption metric đẹp ≠ AI tạo giá trị thật.

Case thất bại/cảnh báo (WeFit) dạy nhóm tôi rằng:
  Tất cả growth metric (user, GMV, check-in) tăng đều, dashboard xanh,
  nhưng MỖI lượt check-in lỗ → càng tăng càng phá sản.
  Power-user (10% dùng 80% lượt) là kẻ giết unit economics.
  Áp dụng AI dashboard: power-user AI (gen 1000 prompt/tuần) có thể giấu
  fact là 90% suggestion bị reject hoặc gây rework.

Vì vậy dashboard nhóm tôi phải tránh:
  (1) Đặt growth metric mà không có per-unit metric đi kèm
  (2) Không hỏi "nếu gấp 10, lời hơn hay lỗ nặng hơn?" trước mỗi metric
  (3) Không ghi "metric này KHÔNG chứng minh được ___" ngay cạnh
  (4) Để power-user (dev/team dùng AI nhiều nhất) lấp liếm quality fail của tổng thể
```

---

## 1 bài học nhóm áp dụng NGAY vào dashboard

```markdown
Mọi metric Activation/Engagement (% dev dùng AI, # prompt, # PR có AI-assist)
PHẢI có cột "per-unit metric đi kèm" + cột "không chứng minh được ___".

Cụ thể, thay vì chỉ có "% PR có AI-assist", thêm:
  - Per-unit: "Cost per AI-assisted PR completed = (AI license cost + senior review time) / PR pass review"
  - Không chứng minh được: "AI có thực sự rút ngắn time-to-merge không, hay senior phải review lâu hơn?"

Đây là cách phòng vanity metric trap kiểu WeFit.
```

---

## Câu nói cho phần share lớp

> *"WeFit chết không phải vì AI hay tech kém — chết vì dashboard chỉ đo growth, không đo unit economics. Mọi metric AI adoption dashboard của chúng tôi sẽ đi đôi: 1 growth metric + 1 per-unit metric + 1 'không chứng minh được'. Để chính chúng tôi không tự lừa mình."*

---

## Unresolved questions

- WeFit có công bố số per-unit (cost/check-in) không, hay chỉ media tự phân tích sau khi phá sản?
- MoMo gần đây đã có lãi quý/năm nào chưa? (cập nhật 2026 — last known: vẫn lỗ ròng nhưng break-even từng business line)
- Có case AI Việt Nam (FPT.AI, VinAI, Misa AI) tương tự cho lab tiếp theo không?
