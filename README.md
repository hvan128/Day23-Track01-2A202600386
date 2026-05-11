# Day 23 Track 01 — Ngô Hải Văn

**Mã học viên:** 2A202600386
**Họ tên:** Ngô Hải Văn
**Nhóm:** [tên nhóm TBD]
**Course repo:** https://github.com/VinUni-AI20k/Day23-Track01-AI-Adoption
**Buổi học:** 2026-05-11 · 09:00-13:00

## Thành viên nhóm

| # | Họ tên | Mã học viên |
|---|---|---|
| 1 | Phan Thanh Sang | 2A202600280 |
| 2 | Ngô Hải Văn | 2A202600386 |
| 3 | Trần Tiến Dũng | 2A202600314 |
| 4 | Nguyễn Tiến Dũng | 2A202600219 |
| 5 | Nguyễn Nhị | 2A202600505 |
| 6 | Phạm Đan Kha | 2A202600253 |

---

## Files (theo template assignment v2)

| File | Loại | Deadline | Trạng thái |
|---|---|---|---|
| `01-case-evidence-matrix.md` | Cá nhân (1 case được giao) | Cuối Block 2 | ✅ Done (gồm cả 2 case + personal context — chờ team chia case) |
| `02-case-comparison.md` | Nhóm (mỗi thành viên copy) | Cuối Block 2 | ✅ Done — MoMo ↔ WeFit |
| `03-product-roi-dashboard.md` | **Cá nhân (đổi từ nhóm)** | 13:00 | ✅ v1 — GradeAI với data thực Lab 16-22 |
| `04-reflection.md` | Cá nhân | 24h sau lớp | 📝 Skeleton |

---

## Scope dashboard (Phần D — 60 điểm)

**Product**: **GradeAI** — AI grading assistant cho giáo viên Ngữ Văn THPT VN

**Nguồn data thực** (no fake numbers):
- **Lab 16** (`/2A202600386_NgoHaiVan_Lab16/`) — Idea reframe, customer card, JTBD needs
- **Lab 17** PRD — Functional spec
- **Lab 18** (`/2A202600386_NgoHaiVan_Lab18/`) — Financial model: ARPU 200K, TAM 40K GV, LTV/CAC 4.17, COGS 75K, Gross Margin 62.5%, NPV +246M VND
- **Lab 19** (`/2A202600386_NgoHaiVan_LAB19/`) — Pitch memo (50 GV pilot, 80% retention), early adopter strategy
- **Lab 20** (`/2A202600386_NgoHaiVan_LAB20/`) — OKRs Q1 (KR1 200 GV active, KR2 MRR 40M, KR3 NPS>50 Churn<8%), RICE matrix (top: OCR 2188, Analytics 2025, Personalization 1750)
- **Lab 21** (`/2A202600386_NgoHaiVan_LAB21/`) — Risk register v2, incident playbook
- **Lab 22** (`/2A202600386_NgoHaiVan_LAB22/`) — Marketing claims audit, compliance
- **Production app** (`/A20-App-129/gradeai/`) — Next.js 14 + Supabase + Cloudflare R2 + Google Vision OCR + Claude Haiku 4.5

**4 workflows extracted từ production app**:
1. Upload & OCR scan (Google Vision → confidence tier green/yellow/red)
2. AI Grading by rubric (Claude Haiku → điểm thành phần + nhận xét nháp)
3. Teacher Review & Edit (GV finalize, no auto-publish)
4. Class Analytics (lỗi lớp, phân phối điểm, gợi ý bài chữa)

---

## Case nhóm chọn (Block 2)

- **Success (VN):** MoMo — ecosystem hoạt động, cross-sell tốt, nhưng **lợi nhuận thật chưa prove được**
- **Fail (VN):** WeFit / Onaclover — vanity metric trap kinh điển: user + GMV tăng nhưng **unit economics lỗ → phá sản**

**Bài học chính áp vào GradeAI**: Mọi growth metric (# GV active, # bài graded, MRR) PHẢI có per-unit metric đi kèm. Test: *"Nếu metric này tăng gấp 10, công ty lời hơn hay lỗ nặng hơn?"*

---

## Personal context — Background story

Dev tại vendor payment processing (OpenWay Way4) phục vụ NH thương mại VN. Quan sát: vendor có "permissive AI policy" nhưng adoption ~ 0 — senior bỏ Cursor/Copilot vì AI không biết Way4 niche, junior dùng giấu qua phone. Insight này motivate góc nhìn cho dashboard GradeAI: *"Không cấm ≠ enable"* — bài học áp vào GradeAI là phải có Champion + 1:1 onboarding + workflow redesign cụ thể, không chỉ "cho GV cài app".

---

## Submit checklist

- [ ] Repo public trước Block 5 end (13:00)
- [x] `01-case-evidence-matrix.md` ready (chờ team chia case nếu cần)
- [x] `02-case-comparison.md` synced với team (MoMo ↔ WeFit)
- [x] `03-product-roi-dashboard.md` v1 — GradeAI, individual scope
- [ ] `03` v2 sau red-team Block 4-5
- [ ] Link repo share Discord `#day23-submissions`
- [ ] `04-reflection.md` trong 24h
- [ ] Repo public ≥ 2 tuần
