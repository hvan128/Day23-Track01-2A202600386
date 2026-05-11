# 01 — Case Evidence Matrix

**Học viên:** Ngô Hải Văn · 2A202600386
**Loại:** Cá nhân — phân tích 1 case được giao
**Case được giao:** ✅ **MoMo** (success VN)
**Block 2 · 15 điểm**

> Note: Team đã chọn cặp case MoMo ↔ WeFit. Bản này phân tích MoMo theo template Part A (cá nhân). So sánh MoMo vs WeFit + bài học cho dashboard ở file `02-case-comparison.md` (nhóm).

---

## Case Evidence Matrix — MoMo

| Trường | Trả lời |
|---|---|
| **Case** | MoMo — ví điện tử / super-app Việt Nam (~31M users công bố, top-2 fintech VN cùng ZaloPay) |
| **AI/Product được dùng trong workflow nào?** | User mở app → giao dịch (thanh toán, chuyển tiền, mua dịch vụ) → MoMo xử lý qua đối tác ngân hàng → trả kết quả + ghi nhận hành vi cross-sell |
| **Người dùng chính là ai?** | End-user (consumer VN) thanh toán/chuyển tiền hàng ngày; merchant nhận thanh toán |
| **Họ đo metric gì?** | MAU, transaction volume, retention theo cohort, số dịch vụ user dùng trong app (cross-sell rate) |
| **Metric thuộc layer nào?** | **Activation + Engagement + Retention** (behavior layer). *Thiếu* Value layer (per-unit profitability) |
| **Metric chứng minh được gì?** | (1) User quay lại đều — habit có hình thành; (2) Dùng nhiều dịch vụ trong app — ecosystem có dính, cross-sell có cơ sở; (3) Mạng lưới merchant + bank đối tác đủ lớn để chứng minh sản phẩm có demand thực |
| **Metric chưa chứng minh được gì?** | (1) **Lợi nhuận thật** — MoMo lỗ ròng nhiều năm liền dù MAU và transaction volume tăng; (2) Tỉ lệ giao dịch thành công khi hạ tầng ngân hàng đối tác lỗi (depend vào partner); (3) Cost-to-acquire vs LTV thật theo cohort |
| **Thiếu metric nào?** | (a) Unit economics theo cohort (margin per transaction × frequency per user); (b) Chi phí khuyến mãi cần để giữ chân 1 user active; (c) Contribution margin per service line trong super-app — service nào "gánh" và service nào "ăn theo" |
| **Rủi ro lớn nhất** | Mở rộng dịch vụ + tăng MAU nhưng **không khoá được unit economics** → có thể lỗ vĩnh viễn dù scale rất to. Phụ thuộc vào subsidies từ investor để duy trì "ecosystem ghost" |
| **Bài học cho dashboard nhóm** | Cross-sell + retention là dấu hiệu tốt nhưng **KHÔNG đủ** để prove product có tạo giá trị thật. Mỗi growth metric phải đi kèm per-unit metric (margin/contribution per user, per transaction). Áp vào dashboard AI: "# GV active" + "# bài graded" cần đi kèm "contribution margin per active GV qua 2 đợt" |

---

## Map vào AI Adoption Stack (slide 10)

| Layer | MoMo |
|---|---|
| 1 Capability Shock | Smartphone penetration + QR payment đẩy ví điện tử thành mainstream VN |
| 2 Org Absorption | Bank partnership + merchant network — readiness có |
| 4 Workflow Redesign | User workflow thanh toán thực sự đổi — không còn cần ATM/cash cho giao dịch nhỏ |
| 7 Value Measurement | **Behavior metric mạnh** (MAU/Retention/Cross-sell) nhưng **thiếu Value/Profit layer** |
| 8 Normalization | Đạt — "Có MoMo không?" thành câu mặc định ở quán cafe / chợ |

---

## Câu chốt cho phần share lớp

> *"MoMo là case thành công về **behavior + ecosystem stickiness** — nhưng cảnh báo cho dashboard nhóm tôi: behavior metric đẹp không tự động nghĩa là tạo giá trị thật. Lỗ ròng nhiều năm dù MAU tăng cho thấy: chưa khoá được unit economics thì grow nhanh = burn nhanh. Áp vào AI adoption: không chỉ đo có bao nhiêu người dùng AI, mà phải đo mỗi người dùng có tạo contribution margin dương không."*

---

## Tự kiểm tra

- [x] Không chỉ kể chuyện case
- [x] Có nêu metric cụ thể (MAU, transaction volume, retention cohort, cross-sell)
- [x] Có nói metric chứng minh được gì (3) và chưa chứng minh được gì (3)
- [x] Có ít nhất 1 bài học áp dụng vào dashboard nhóm (per-unit metric pair)
