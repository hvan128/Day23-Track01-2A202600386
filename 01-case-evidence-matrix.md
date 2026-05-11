# 01 — Ghi Chú Thách Thức Cá Nhân + Case Evidence Matrix

**Học viên:** Ngô Hải Văn · 2A202600386
**Block 2 · Deliverable cá nhân · 15 điểm**

---

## Part 1 — Ghi chú thách thức áp dụng AI (Personal Fail Scan)

### Bối cảnh

- **Role**: Dev tại vendor payment processing (OpenWay Way4) phục vụ các NH thương mại VN
- **AI policy hiện tại**: Không cấm, không support, không ai dùng
- **Threshold theo slide 23**: Chưa qua nổi *Usage* → chưa tới *Pilot*

### Tình huống 1 — "AI không biết Way4 + paste code = vi phạm NDA"

| Trường | Nội dung |
|---|---|
| Việc lẽ ra phải đổi | Dev customize Way4 module (Java + PL/SQL) cho bank client — đáng lẽ AI giúp boilerplate, generate test, đọc log |
| Thực tế xảy ra | Cursor/Copilot gợi ý theo Java pattern chuẩn → sai semantics Way4 (wrapper riêng, config DB-driven, transaction model riêng). Phần "có giá trị" (fee rules, BIN config, fraud logic) thì NDA bank không cho paste lên cloud LLM |
| Câu nói nghe được | *"Đọc suggest còn lâu hơn tự gõ"* · *"AI không biết Way4 đâu"* · *"Code này là của bank, không paste được"* |
| Hậu quả | Senior thử 1-2 tuần rồi bỏ. Junior không có ai dạy dùng đúng cách. License có nhưng usage ~ 0 |
| Giả thuyết ban đầu | Trust/Quality + Readiness gap — không có RAG/eval cho Way4 niche, không có per-client AI policy |

### Tình huống 2 — "Permissive policy + on-site bank = adoption tự chết"

| Trường | Nội dung |
|---|---|
| Việc lẽ ra phải đổi | Vendor có chính sách "cho dùng AI" → tưởng đã enable team |
| Thực tế xảy ra | (a) Mỗi dev làm 2-3 dự án song song cho **N bank** với **N NDA khác nhau** → confuse → default "cấm bản thân" cho an toàn. (b) Dự án on-site phải làm trên VDI/jumphost của bank → AI tool không cài được. Cùng người, project A có AI 2x speed, project B (bank-VDI) chậm như 2020 |
| Câu nói nghe được | *"Không rõ bank này có cho dùng AI không, thôi không dùng"* · *"VDI bank cấm cài extension"* · *"Cấp trên không cấm nhưng cũng không bảo dùng cái nào"* |
| Hậu quả | License mua xong nằm chết. Không có owner AI adoption. Không có metric → không ai biết đang fail |
| Giả thuyết ban đầu | Workflow + ADKAR Ability gap — không có owner, không có process, dev tự cấm bản thân do uncertainty |

### Map vào 5 lens

| Lens | Điểm nghẽn |
|---|---|
| **Workflow** | Workflow Way4 dev (customization, code review, NDA-check, on-site) chưa redesign để có AI |
| **ADKAR — Ability** | Desire có, Knowledge/Ability thiếu: dev không biết cách dùng AI cho Way4 niche |
| **Readiness** | Thiếu Way4 RAG/docs cho AI, thiếu per-client AI policy, thiếu on-prem option |
| **Metrics** | Vendor chỉ đo project margin + SLA, không có AI productivity metric → không ai đẩy |
| **Trust/Quality** | Không có cơ chế validate output AI cho payment domain (ISO 8583, EMV, scheme rules) |

### Insight share nhóm

> *"Ở vendor của tôi, công ty **không cấm** AI nhưng adoption ~ 0. Hoá ra 'permissive' không tạo adoption — nó chỉ làm lãnh đạo tưởng đã enable team. Thiếu **owner + workflow redesign + đo lường** thì 'cho dùng' = 'không dùng'."*

### Pattern dự kiến gom với nhóm

1. **Domain niche** — AI training data thiếu, gợi ý sai → trust thấp → bỏ
2. **Multi-tenant NDA** — N khách hàng = N policy → default cấm bản thân
3. **Permissive ≠ Enable** — không cấm không bằng cho dùng có hỗ trợ
4. **Workflow không đổi** — quy trình dev/review/NDA-check không redesign
5. **Không có metric AI** — không đo = không thấy fail = không sửa

---

## Part 2 — Case Evidence Matrix

**Cặp case nhóm chọn:** ✅ MoMo (success VN) ↔ ❌ WeFit / Onaclover (fail VN)
**Lý do chọn:** Case Việt Nam dạy *vanity metric trap + unit economics* trực tiếp nhất

| Trường | ✅ Case thành công (MoMo) | ❌ Case thất bại (WeFit / Onaclover) |
|---|---|---|
| Case | MoMo — ví điện tử / super-app Việt Nam | WeFit (Onaclover) — subscription gym/yoga network |
| AI/Product dùng trong quy trình nào? | User mở app → giao dịch (thanh toán, chuyển tiền, mua dịch vụ) → MoMo xử lý qua đối tác ngân hàng → trả kết quả + ghi nhận hành vi | User mua gói tháng tập gym/yoga không giới hạn → đặt lịch qua app → check-in tại phòng đối tác → WeFit trả tiền cho phòng tập theo từng lượt |
| Người dùng chính là ai? | End-user (consumer) thanh toán/chuyển tiền hàng ngày | End-user (consumer) đặt lịch tập gym/yoga |
| Họ đo chỉ số gì? | MAU, transaction volume, retention theo cohort, số dịch vụ user dùng (cross-sell) | Số user đăng ký, GMV (giá trị gói bán ra), tổng lượt check-in, số phòng tập đối tác |
| Chỉ số đó thuộc lớp nào? | Activation + Engagement + Retention (behavior layer) | Chỉ Activation + Engagement (thiếu Value layer) |
| Chỉ số đó chứng minh được gì? | User quay lại đều, dùng nhiều dịch vụ → ecosystem có dính, có cơ sở để cross-sell | Sản phẩm có demand thật, user dùng nhiệt tình, mạng lưới mở rộng nhanh, gọi vốn được |
| Chỉ số đó chưa chứng minh được gì? | Lợi nhuận thật (MoMo lỗ nhiều năm liền); tỉ lệ giao dịch thành công khi hạ tầng ngân hàng đối tác lỗi | Lời/lỗ trên mỗi user. Càng user tích cực càng lỗ — chi phí trả phòng tập/lượt > doanh thu/lượt khi quy về tháng |
| Thiếu chỉ số nào? | Unit economics theo cohort; chi phí khuyến mãi cần để giữ chân 1 user active | **Unit economics**: cost-per-check-in vs revenue-per-check-in; phân bố lượt đi theo user (10% user dùng 80% lượt → kéo lỗ) |
| Rủi ro lớn nhất | Mở rộng dịch vụ nhưng không khoá được unit economics → lỗ vĩnh viễn dù scale to | Power-user (10% dùng 80% lượt) phá unit economics — càng tăng engagement càng lỗ. Vanity metric thuần |
| Bài học cho dashboard nhóm | Cross-sell + retention là dấu hiệu tốt nhưng KHÔNG đủ — phải đo cost-to-acquire/retain vs LTV | **MỌI growth metric phải có per-unit metric đi kèm**. Không có per-unit = đang nói dối dashboard |

### Bài học chính cho lab AI adoption

> **WeFit là sách giáo khoa của vanity metric**: số user và GMV xanh rực trên dashboard, trong khi metric thật sự quan trọng — *lời/lỗ trên mỗi đơn vị giao dịch* — không được đo, hoặc bị giấu sau con số tổng.

### 3 Quy tắc áp dụng cho dashboard

1. **Mỗi metric tăng trưởng** (user, GMV, lượt dùng) **phải có metric đơn vị (per-unit)** đi kèm: doanh thu/user, chi phí/user, biên lợi nhuận/giao dịch
2. **Hỏi câu test:** *"Nếu metric này tăng gấp 10, công ty lời hơn hay lỗ nặng hơn?"* — không trả lời được → metric đó chưa đủ
3. **Với mỗi metric, ghi rõ "không chứng minh được ___"** ngay cạnh để chống tự lừa bản thân

### Áp dụng vào AI adoption metrics (vanity → per-unit pair)

| Vanity metric AI (giống WeFit) | Per-unit metric đi kèm (như MoMo nên có) |
|---|---|
| "# user dùng AI / tuần" | "Cost per AI-assisted task COMPLETED & PASS review" |
| "Số PR có AI-assist" | "% PR AI-assist không rollback / không tạo bank-client issue trong 30d" |
| "Số AI suggestion" | "Accept rate × downstream-quality rate (composite)" |
| "Tổng prompt count" | "Time saved per workflow × no-rework rate" |
| "GMV-style: AI tokens consumed" | "Value delivered per token / cost per token" |

---

## Câu chốt (cho phần share lớp)

```markdown
Case thành công (MoMo) dạy nhóm tôi rằng:
  Behavior metric (cross-sell, retention) là dấu hiệu tốt nhưng KHÔNG ĐỦ.
  Thiếu unit economics → có thể grow đẹp 5 năm mà vẫn lỗ vĩnh viễn.
  Áp dụng AI dashboard: adoption metric đẹp ≠ AI tạo giá trị thật.

Case thất bại/cảnh báo (WeFit) dạy nhóm tôi rằng:
  Tất cả growth metric (user, GMV, check-in) tăng đều, dashboard xanh,
  nhưng MỖI lượt check-in lỗ → càng tăng càng phá sản.
  Power-user (10% dùng 80% lượt) là kẻ giết unit economics.
  Áp dụng AI dashboard: power-user AI có thể giấu fact là 90% suggestion
  bị reject hoặc gây rework.

Vì vậy dashboard nhóm tôi phải tránh:
  (1) Growth metric mà không có per-unit metric đi kèm
  (2) Không hỏi "nếu gấp 10, lời hơn hay lỗ nặng hơn?" trước mỗi metric
  (3) Không ghi "metric này KHÔNG chứng minh được ___" ngay cạnh
  (4) Để power-user (dev dùng AI nhiều nhất) lấp liếm quality fail của tổng thể
```

---

## Tự kiểm tra

- [x] Không chỉ kể chuyện case
- [x] Có nêu chỉ số cụ thể (98% adoption · $62M chi · 0 patient)
- [x] Có nói chỉ số chứng minh được gì và không chứng minh được gì
- [x] Có ít nhất 1 bài học áp dụng vào dashboard nhóm
- [x] Có ghi chú thách thức cá nhân (2 tình huống)
- [x] Map vào 5 lens (Workflow / ADKAR / Readiness / Metrics / Trust)
