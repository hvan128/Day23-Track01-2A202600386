# 03 — Product ROI Dashboard: GradeAI

**Học viên:** Ngô Hải Văn · 2A202600386
**Loại bài:** Cá nhân (đổi từ nhóm sang cá nhân theo quyết định nhóm)
**Block 3-5 · 60 điểm · Deadline: 13:00**
**Product:** GradeAI — AI grading assistant cho giáo viên Ngữ Văn THPT VN
**Nguồn data:** Lab 16 (idea reframe) · Lab 17 (PRD) · Lab 18 (finance model) · Lab 19 (pitch + early adopter) · Lab 20 (OKRs + RICE + roadmap) · Lab 21 (risk register v2) · Lab 22 (compliance + marketing audit) · `A20-App-129/gradeai` (production code)

---

## Phần A — Adoption Context

### A.1 Thách thức áp dụng AI

| Trường | Trả lời |
|---|---|
| Thách thức adoption | GV Ngữ Văn THPT VN có pain rõ ràng (15-30h/đợt chấm 60-120 bài) nhưng adoption AI grading rất chậm: GV không tin AI hiểu rubric, sợ học sinh phát hiện feedback do AI viết, sợ sai chính trị/định hướng giáo dục, không quen rời tay-bút sang scan-upload |
| Tình huống xuất phát | Pilot 50 GV (Lab 19) — 80% retention sau pilot, nhưng nhân rộng ra ~40,000 GV TAM gặp barrier "nghe hay nhưng không dùng" |
| Dấu hiệu kẹt | GV xem demo gật đầu nhưng không cài app · GV cài rồi không upload bài lần 2 · GV upload nhưng vẫn ngồi viết tay nhận xét song song |
| Vì sao đáng giải quyết | KR1/OKR Lab 20: 200 GV active ≥2 đợt/quý — không đạt sẽ trigger pivot. Lab 18 base case cần 0.65%/mo adoption rate để LTV/CAC = 4.17 (healthy) |

### A.2 Sản phẩm / Công cụ AI

| Trường | Trả lời |
|---|---|
| Tên sản phẩm | **GradeAI** — Next.js 14 + Supabase + Cloudflare R2 + Google Vision OCR + Claude Haiku 4.5 |
| Người dùng chính | Giáo viên Ngữ Văn THPT Việt Nam (TAM 40,000 GV) |
| Bối cảnh sử dụng | Sau mỗi đợt kiểm tra (2-3 đợt/HK): GV cần chấm 60-120 bài tự luận trong 5-10 ngày |
| Mục tiêu kinh doanh (OKR Q1 sau Seed) | KR1: 200 GV active ≥2 đợt · KR2: MRR 40M VND · KR3: NPS > 50, Churn < 8%/mo |
| Không nằm trong phạm vi | LMS integration (RICE 1280, để phase 2) · Mobile native app (RICE 500, web đủ) · Non-Ngữ Văn subject (focus market trước) |

### A.3 4 Quy trình chính (extract từ production app + PRD Lab 17)

| # | Tên quy trình | Vai trò AI | Điểm người kiểm tra | Khi AI sai thì xử lý |
|---|---|---|---|---|
| 1 | **Upload & OCR scan** — GV chụp/upload ảnh bài viết tay → Google Vision OCR → assign confidence tier (green/yellow/red) | OCR transcribe + confidence scoring | GV xem text OCR cho mỗi bài; review mandatory cho tier red, optional cho yellow | Red tier hiện cảnh báo "OCR confidence thấp, vui lòng kiểm tra"; GV sửa text trực tiếp; bug log → fine-tune Vision API config |
| 2 | **AI Grading by rubric** — Rubric-driven LLM grading (Claude Haiku 4.5 qua OpenRouter/Anthropic) → điểm thành phần + nhận xét nháp theo từng tiêu chí | Generate điểm thành phần + draft nhận xét cá nhân hoá theo rubric | GV review điểm và sửa trước khi finalize · GV có quyền veto/override hoàn toàn | Comment audit log lưu version trước/sau edit · Nếu GV reject > 50% comment AI → flag rubric mismatch · Auto-trigger fine-tune dataset |
| 3 | **Teacher Review & Edit** — GV review điểm + nhận xét → edit inline → finalize → publish cho học sinh | AI gợi ý phrasing alternatives khi GV gõ sửa | GV là người quyết định cuối (no auto-publish) · 100% bài phải được GV xác nhận trước khi xuất | Edit history lưu lại; metric "edit rate" feed về fine-tune; nếu GV phải edit > 40% nội dung → trigger model retrain |
| 4 | **Class Analytics** — Tổng hợp lỗi lớp, phân phối điểm, top mistakes theo rubric → gợi ý bài chữa lỗi tập trung | Aggregate + categorize lỗi · Suggest remediation focus | GV verify insight có đúng thực tế lớp không trước khi dùng cho bài chữa | Insight sai → GV "reject insight" button; aggregate đó loại khỏi sample tuần đó; track precision của insight engine |

### A.4 Chẩn đoán nhanh ADKAR

| Stage | Câu hỏi | Nhận định |
|---|---|---|
| Awareness | GV có biết GradeAI? | OK — Lab 19 early adopter strategy + Champion program đã reach |
| Desire | GV có muốn dùng? | OK lúc demo, nhưng khi đến đợt kiểm tra thật → drop. Sợ học sinh/đồng nghiệp phát hiện dùng AI, sợ trách nhiệm nếu sai |
| Knowledge | GV biết dùng đúng cách? | **THIẾU** — workflow scan-upload không quen, prompt/rubric setup khó, không có hướng dẫn cá nhân hoá |
| **Ability** | GV làm được trong workflow thật? | **THIẾU NHIỀU NHẤT** — không có thời gian học ngay đợt kiểm tra; OCR fail trên chữ xấu; mobile (Lab 16-17 phase 2) chưa có → GV phải về máy bàn |
| Reinforcement | Có cơ chế giữ behavior? | **THIẾU** — không có owner sau onboard; không có in-product nudge khi GV bỏ giữa đợt; Champion chỉ touch ban đầu |

**Barrier chính:**
```markdown
Ability (kết hợp Knowledge gap) — Desire ban đầu cao nhưng không qua được lần dùng thật đầu tiên. Cần: (1) onboarding < 15 phút, (2) OCR fallback rõ khi chữ xấu, (3) Champion 1-1 follow-up sau đợt 1, (4) NPS prompt sau đợt 2 (Sean Ellis cut).
```

### A.5 3 Tactic áp dụng (từ menu 25 tactics)

| Tactic | Barrier | Workflow | Owner | Deadline |
|---|---|---|---|---|
| 1. **Champion-led 1:1 onboarding** — top 10 pilot GV làm champion, kèm 1:1 cho 5 GV mới mỗi người | Ability + Reinforcement | Toàn quy trình (đặc biệt WF1+WF2) | Founder/CPO (Ngô Hải Văn) | Day 0-30 |
| 2. **OCR fallback playbook** — workflow rõ khi OCR red: GV gõ trực tiếp thay vì scan, hoặc dùng feature "type-mode" | Ability (mất time khi OCR fail) | Workflow 1 | OCR-service team | Day 0-30 |
| 3. **Sean Ellis cohort survey** — sau đợt kiểm tra thứ 2 in-product (KR3): *"How would you feel if you could no longer use GradeAI?"* — ≥40% "very disappointed" = PMF | Reinforcement (lock in habit) + signal cho continue/pivot | Toàn product | PM | Day 31-60 |

---

## Phần B — Diagnosis nguyên nhân gốc

### Lăng kính (chọn 3)

| Lăng kính | Câu hỏi | Chẩn đoán | Evidence / observation |
|---|---|---|---|
| **Workflow** | Workflow chấm bài có đổi không, hay chỉ thêm AI? | **Chưa redesign đủ** — Pilot 50 GV vẫn có người upload xong rồi ngồi viết tay nhận xét song song "cho chắc". Trust gap giữa AI draft và GV final | Lab 19 pilot data + early adopter interviews |
| **ADKAR** | GV kẹt stage nào? | **Ability + Reinforcement** — Desire OK demo nhưng không qua lần dùng thật đầu. Không có nudge sau đợt 1 | Lab 22 marketing audit warned: 80% retention chưa qua 6-8 tuần |
| **Trust/Quality** | GV có tin output? | **Một phần** — Trust theo bài/tier khác nhau. Bài top student: GV tin AI hơn. Bài median/low: GV vẫn muốn tự chấm vì sợ AI "không hiểu ngữ cảnh tiếng Việt" | Rubric mismatch rate, edit rate trên Pilot |

### Nguyên nhân gốc

| # | Nguyên nhân | Vì sao đây là root cause | Case nào hỗ trợ |
|---|---|---|---|
| 1 | **Ability gap ở lần dùng thật đầu tiên** — Onboarding qua được nhưng đợt kiểm tra thật (under deadline pressure) GV bỏ vì OCR fail/UX không quen | Nếu sửa được → retention sau đợt 1 jump → KR1 đạt. Đây là điểm leverage cao nhất | **WeFit** — user dùng nhiệt tình demo nhưng power-user 10% phá unit economics; **MoMo** — retention chỉ thật khi user qua nhiều lần dùng (cross-sell) |
| 2 | **Vanity metric trap nguy cơ cao** — Hiện tracking "active GV count" theo MAU. Một GV mở app 1 lần/đợt = active. KR1 thực ra phải đo "active ≥2 đợt" mới có ý nghĩa | Nếu đo vanity → có thể grow đẹp 3 tháng nhưng unit economics lỗ (CAC 300K, ARPU 200K — chưa hồi vốn nếu churn > 8%) | **WeFit** — 10% user dùng 80% lượt phá unit economics, dashboard xanh nhưng phá sản |

---

## Phần C — Solution Roadmap 30/60/90 ngày

| Mốc | Việc cần làm | Owner | Dấu hiệu hoàn thành | Rủi ro |
|---|---|---|---|---|
| **0-30d** | (1) Champion 1:1 onboarding cho 50 GV mới (mở rộng từ 50→100) · (2) OCR fallback playbook ship · (3) Baseline OCR accuracy theo điều kiện thực (lớp học, chữ xấu) · (4) Edit rate tracking cho mỗi workflow | Founder + OCR-service team | 100 GV onboarded; OCR baseline đo; edit rate dashboard live | Champion bandwidth (top 10 chỉ kèm được 50); OCR baseline kém hơn lab |
| **31-60d** | (1) Class Analytics MVP (WF4) ship · (2) Sean Ellis in-product survey sau đợt 2 · (3) Cohort churn analysis (đợt 1 → đợt 2 dropout root cause) · (4) Comment edit rate < 40% target | PM + Engineering | Sean Ellis ≥40% response collected; analytics MVP có 50% GV dùng | NPS thấp do nhận xét vẫn "AI-feel"; analytics nhìn không actionable |
| **61-90d** | (1) Decision gate — Continue/Pivot/Kill dựa KR1+KR2+KR3 · (2) MRR check (target 40M VND = 200 GV × 200K) · (3) Cá nhân hoá phong cách chấm (RICE 1750) bắt đầu · (4) Lab 21 risk mitigation top 3 ship | PM + Founder | KR1 200 GV; KR2 MRR ≥30M (75% target); Sean Ellis ≥40% | KR1 ngắt ở 120-150 GV; CAC tăng vì marketing tube cạn |

### 3 tactic chi tiết

| Tactic | Map Stack layer | Apply on |
|---|---|---|
| Champion-led 1:1 (top 10 GV kèm 5 mỗi người) | Layer 5 Human Change + Layer 6 Rollout | Toàn pilot |
| OCR fallback type-mode | Layer 2 Org Absorption (data readiness) + Layer 4 Workflow Redesign | WF1 |
| Sean Ellis in-product survey | Layer 7 Value Measurement (true PMF signal) | Toàn product |

---

## Phần D — Product ROI Dashboard

### D.1 Chỉ số toàn sản phẩm (B.1) — mỗi growth metric đi kèm per-unit (chống WeFit trap)

| Lớp đo | Chỉ số | Per-unit pair (KHÔNG được thiếu) | Baseline | Target 90d | Data source | Owner | "Không chứng minh được ___" | Red-team risk | Sửa v2 |
|---|---|---|---:|---:|---|---|---|---|---|
| **Activation (KR1 Leading)** | # GV active dùng ≥2 đợt kiểm tra trong quý | Cost per active GV qua 2 đợt = CAC / activation-to-2nd-batch-rate | 50 (pilot) | **200** | Supabase grading_sessions table + cohort SQL | Founder | Không chứng minh GV qua đợt 2 tự nguyện hay vì miễn phí thử nghiệm | (chờ red-team) | (chờ red-team) |
| **Value (KR2 Lagging)** | MRR (Monthly Recurring Revenue) | Contribution margin per active GV = (ARPU 200K − COGS 75K) − allocated CAC payback | 0 (chưa charge) | **40M VND** | Stripe billing | Founder | MRR tăng có thể vì discount, không vì willingness to pay thật | | |
| **Quality/Trust (KR3 Protect)** | NPS + Churn rate | NPS theo cohort (đợt 1 / đợt 2 / đợt 3+) + churn theo cohort | NPS pilot ~chưa đo công khai | **NPS >50, Churn <8%/mo** | In-product Sean Ellis survey + Stripe churn | PM | NPS cao có thể bias từ champion cohort, không represent mass-market | | |

### D.2 Chỉ số theo từng workflow (B.2)

#### Workflow 1 — Upload & OCR scan

| Lớp | Metric | Per-unit pair | Baseline | Target | Data source | Owner | "Không chứng minh ___" |
|---|---|---|---:|---:|---|---|---|
| Activation | % GV upload ≥1 bài trong tuần đầu sau onboard | Cost per active uploader | TBD pilot | 80% | Supabase | OCR team | Có chắc GV upload bài THẬT của lớp hay test 1 bài mẫu? |
| Engagement | Avg pages/session | Pages-per-cost (Cloudflare R2 + Vision API per page) | TBD | 30+ | Cloudflare R2 + Vision API logs | OCR team | High avg có thể từ 1-2 power-user lấp liếm |
| Productivity | OCR transcription time/page | $ Vision API cost per page accepted | TBD | <5s, <$0.005 | Vision API logs | OCR team | Time fast có ý nghĩa nếu accuracy giữ được |
| Quality | % OCR confidence GREEN (auto-pass) | (Pages green) / (Pages total) — error rate per page | TBD | ≥70% green | Confidence tier log | OCR team | GREEN không chứng minh OCR đúng — chỉ chứng minh confidence cao |
| Trust | % bài GV phải gõ lại manual sau OCR red | Manual rework rate | TBD | <15% | Edit history log | OCR team | Đo manual rework không tính bài GV bỏ giữa chừng |
| Value | Time saved per essay vs full manual transcribe | $ value saved per essay vs cost per essay | manual ~3-5p | -70% | Time-tracking + cost log | OCR team | Time saved chưa = teacher hài lòng |

#### Workflow 2 — AI Grading by rubric

| Lớp | Metric | Per-unit pair | Baseline | Target | Data source | Owner | "Không chứng minh ___" |
|---|---|---|---:|---:|---|---|---|
| Activation | % GV submit ≥1 batch grading trong tuần đầu | Cost per first-grading-batch | TBD | 75% | grading_sessions | PM | First batch không = adoption thật, cần đợt 2 |
| Engagement | Avg essays graded / session | Cost/essay graded (Haiku API + retraining) | TBD | 30+ | LLM API log | PM | Power-user lấp liếm — phân phối theo GV |
| Productivity | Time-per-essay (upload→graded draft) | Token cost/essay | manual ~15-20p | <5p | App telemetry | PM | Nhanh hơn ≠ chất lượng giữ được |
| **Quality** | **% AI comment GV adopt không sửa** | Edit rate inverse | TBD | ≥60% no-edit | Edit history | PM + AI team | GV không sửa có thể vì lười, không vì AI đúng |
| **Trust** | Edit rate (% chữ GV phải sửa) | $ retraining cost per high-edit comment | TBD | <40% edit | Comment diff | AI team | Low edit không = học sinh nhận về thấy hữu ích |
| Value | Hours saved per đợt kiểm tra vs full manual | $ value GV/hour × hours saved vs subscription cost | manual 15-30h | -50% (giảm xuống 7-15h) | Time survey | PM | Hours saved không = GV sẽ resubscribe |

#### Workflow 3 — Teacher Review & Edit

| Lớp | Metric | Per-unit pair | Baseline | Target | Data source | Owner | "Không chứng minh ___" |
|---|---|---|---:|---:|---|---|---|
| Activation | % GV finalize ≥1 bài qua review | Cost per finalized essay | TBD | 90% | Finalize log | PM | Finalize 1 bài không = qua trust threshold |
| Engagement | Avg edit cycles per essay before finalize | — | TBD | <2 cycles | Edit log | PM | Few cycles có thể vì GV không đủ thời gian review kỹ |
| Productivity | Time-per-essay (graded→finalized) | — | TBD | <2p | App telemetry | PM | Fast finalize có thể = rubber-stamping AI |
| **Quality (CRITICAL)** | **% bài học sinh complain hoặc xin review lại điểm** | $ cost of dispute resolution per disputed essay | TBD | <5% | Student feedback form (post-launch) | PM | Học sinh không complain ≠ feedback chất lượng |
| Trust | % GV tick "AI helped me grade better" sau đợt 2 | — | TBD | ≥70% | In-product survey | PM | Self-report bias |
| Value | NPS post đợt 2 | NPS / churn rate (composite retention proxy) | TBD | ≥50 | Sean Ellis cohort | PM | NPS từ champion bias |

#### Workflow 4 — Class Analytics

| Lớp | Metric | Per-unit pair | Baseline | Target | Data source | Owner | "Không chứng minh ___" |
|---|---|---|---:|---:|---|---|---|
| Activation | % GV mở Analytics dashboard sau đợt 1 | Engineering cost / unique GV mở dashboard | 0 (chưa ship) | 50% | Frontend analytics | PM | Mở 1 lần ≠ dùng decision-making |
| Productivity | Time tổng hợp lỗi lớp (analytics vs manual) | $ cost engineering vs hours saved | manual 1-2h | <10p | App + survey | PM | Time saved không = GV làm bài chữa tốt hơn |
| **Quality** | % insight Analytics GV verify đúng thực tế lớp | — | TBD | ≥75% verified | "Reject insight" log | PM | Verified không = GV act trên insight |
| Value | % GV tạo ≥1 bài chữa lỗi từ Analytics insight | $ value of remedial lesson per disadvantaged student | 0 | 30% | App + survey | PM | Tạo bài chữa không = học sinh học tốt hơn |

---

## Phần E — Dashboard Mock (6 tiles)

```text
┌────────────────────────────────────────────────┐  ┌────────────────────────────────────────────────┐
│ TILE 1: PRODUCT HEALTH (KR1)                    │  │ TILE 2: WF1 — OCR Quality                       │
│ Number: # GV active ≥2 đợt / quý                │  │ Number: % OCR GREEN tier                        │
│ Trend: 50 → 200 (90d)                           │  │ Trend: TBD → ≥70%                               │
│ Status: AMBER (50/200) → GREEN @ ≥140 (70%)     │  │ Status: GREEN if ≥70%, RED if <50%              │
│ Action: champion 1:1; OCR fallback              │  │ Action <50%: ship type-mode fallback urgent     │
└────────────────────────────────────────────────┘  └────────────────────────────────────────────────┘

┌────────────────────────────────────────────────┐  ┌────────────────────────────────────────────────┐
│ TILE 3: WF2 — AI Grading Quality                │  │ TILE 4: WF3 — Trust (CRITICAL)                  │
│ Number: % comment AI GV adopt no-edit           │  │ Number: % bài học sinh complain/dispute điểm   │
│ Trend: TBD → ≥60%                               │  │ Trend: TBD → <5%                                │
│ Status: GREEN ≥60%, RED <40%                    │  │ Status: RED ≥10% → halt scale immediately       │
│ Action: rubric retrain if RED                   │  │ Action RED: pause sale, root cause within 48h   │
└────────────────────────────────────────────────┘  └────────────────────────────────────────────────┘

┌────────────────────────────────────────────────┐  ┌────────────────────────────────────────────────┐
│ TILE 5: VALUE (KR2)                             │  │ TILE 6: DECISION (90d gate)                     │
│ Number: MRR (VND) — Stripe                      │  │ Sean Ellis %: TBD → ≥40% = PMF                  │
│ Trend: 0 → 40M                                  │  │ Continue / Pivot / Kill: TBD                    │
│ Status: GREEN ≥30M, AMBER 15-30M, RED <15M      │  │ Metric mạnh nhất: KR1 + KR3 (active 2 đợt + NPS)│
│ Action <15M: pause ads, fix pricing/conversion  │  │ Action: scale to LMS phase if all 3 GREEN       │
└────────────────────────────────────────────────┘  └────────────────────────────────────────────────┘
```

---

## Phần F — Red-team + sửa v2 (Block 4-5)

### Mình đi phản biện nhóm khác

| Vai được giao | Nhóm bị phản biện | 3 câu hỏi / rủi ro |
|---|---|---|
| (điền sau khi assign) | | 1.<br>2.<br>3. |

### Mình bị phản biện (4 vai × dashboard GradeAI)

#### Pre-draft câu hỏi 4 vai có khả năng bị hỏi (để tôi sẵn sàng)

**CFO** (ROI, attribution):
- "MRR 40M có dựa trên 200 GV × 200K — nhưng ARPU 200K từ WTP nào? Có A/B test không?"
- "CAC 300K assumption — early adopter qua champion có thể CAC = 0, mass market có thể CAC = 600K. Dashboard hiện đo CAC blended?"
- "Lab 18 cho LTV/CAC 4.17. Nếu churn thực ≥ 12%/mo (thay vì 10% giả định), LTV sập về 1M → LTV/CAC = 3.3. Lúc nào trigger pivot?"

**User**:
- "Edit rate target <40% có thể ép GV không sửa kể cả khi nên sửa, vì sợ ‘kéo metric đẹp'. Có cách nào tránh được?"
- "Champion 1:1 không scale — sau 200 GV, ai onboard? Tactic chỉ work ở pilot scale"
- "Học sinh có biết bài được AI chấm không? Đạo đức + WCT?"

**Risk**:
- "OCR fail trên chữ xấu = trải nghiệm tệ → cancel → churn spike. Failure path hiện tại có đo "% GV bỏ giữa upload" không?"
- "Lab 22 cảnh báo claim "3× nhanh" chưa A/B test 200 GV. Marketing claim trên landing nếu sai = vi phạm Khoản 2 Điều 198 (>50M VND profit)"
- "Anthropic rate limit mùa thi (Lab 21 V1) — Q4 + cuối học kỳ nhiều thi, có failover model? OpenRouter fallback test chưa?"

**Workflow Owner**:
- "Data source ghi 'Supabase grading_sessions' — ai pull report? Hằng tuần? Hay chỉ khi sếp hỏi?"
- "Khi tile #4 (student dispute) RED → action 48h root cause — owner cụ thể nào? PM 1 mình kham không?"
- "Sean Ellis survey cohort theo đợt — GV làm 1 đợt rồi nghỉ thì có hiện trong cohort tháng đó không?"

### Ít nhất 2 thay đổi cụ thể v1 → v2

| # | V1 vấn đề | V2 sửa | Vì sao tốt hơn |
|---|---|---|---|
| 1 | (điền sau red-team) | | |
| 2 | | | |

---

## Phần G — Decision Memo (Part D)

```markdown
# Memo Quyết Định Cuối — GradeAI

1. Nhóm khuyến nghị: **Continue with constraints** — pilot 100 GV → 200 GV
   trong 90d. Decision gate Day 90 dựa KR1 + KR3 + Sean Ellis ≥40%.

2. Chỉ số mạnh nhất để bảo vệ quyết định:
   COMPOSITE = (% GV active ≥2 đợt) × (Sean Ellis ≥40% "very disappointed without GradeAI") × (Churn <8%/mo)
   → composite này đo CẢ adoption + PMF signal + retention thật.
   Không vanity-able. Không attribute lệch (không phải MAU đơn).

3. Chỉ số/giả định đã sửa sau red-team:
   (chờ Block 4-5)

4. Trước khi scale (sau Day 90), phải:
   1. Sean Ellis ≥40% confirmed trên cohort đợt-2 (không phải pilot bias)
   2. OCR GREEN-tier ≥70% trên dataset thật (không chỉ lab)
   3. Student dispute rate <5% — verify quality đến học sinh không chỉ GV
   4. Lab 21 risk V1 (Anthropic rate limit mùa thi) có failover route test
   5. CAC blended (champion + paid) trong khoảng 250-400K — Lab 18 base case
```

---

## AI Support Log

| AI giúp gì? | AI sai/chung chung ở đâu? | Sửa thủ công thế nào |
|---|---|---|
| Synthesize Lab 16-22 + production app docs thành dashboard structure | Không tự generate được số baseline thực — phải plug vào số từ pilot 50 GV (chưa có file public) | Đánh dấu "TBD pilot" + cite Lab 18 base case làm proxy |
| Suggest per-unit metric pairs (theo lesson WeFit) | Suggest generic — cần context payment vendor sau đổi sang grading | Map lại từng metric theo workflow GradeAI cụ thể |

---

## Checklist trước khi nộp

- [x] Challenge rõ — không phải "AI adoption khó" chung chung
- [x] 1 product cụ thể (GradeAI)
- [x] 4 workflow chính, mỗi cái có vai trò AI + người kiểm + failure path
- [x] 2 root cause với evidence (Ability gap đợt-1, Vanity metric risk)
- [x] 30/60/90 roadmap + owner cụ thể
- [x] Dashboard có product + workflow level metrics
- [x] KHÔNG chỉ usage — có Quality/Trust/Value layer cho mọi workflow
- [x] Mỗi growth metric có per-unit pair (chống WeFit trap)
- [x] Mỗi metric có "không chứng minh được ___"
- [x] Data source + owner cụ thể (Founder/PM/OCR-team/AI-team)
- [ ] Red-team risk + ≥2 sửa v1→v2 (chờ Block 4-5)
- [x] Memo có Continue/Pivot/Kill draft

---

## Unresolved questions

- Pilot 50 GV có data baseline cụ thể nào public được (OCR accuracy, edit rate, time-per-essay) hay vẫn TBD?
- Stripe billing đã setup chưa hay vẫn pre-revenue? — quyết định KR2 measurement timing
- Sean Ellis survey ngôn ngữ tiếng Việt — copy-test với 5 GV trước khi roll mass?
- Lab 22 đã rà soát claim "3× nhanh" chưa A/B test — có lùi marketing claim không trong khi chờ data 200 GV?
