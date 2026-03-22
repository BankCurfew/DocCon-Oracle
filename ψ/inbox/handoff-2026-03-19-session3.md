# Handoff — DocCon Session 3 → Session 4

**Date**: 2026-03-19 23:24
**From**: DocCon Session 3

---

## ⚠️ URGENT — ต้องทำทันที

### 1. Distribute Conduct Documents
ส่ง Jarvis Bot Conduct + FA Tools Code Conduct ให้ oracle ที่เกี่ยวข้องรับทราบ:
- `/talk-to botdev "DocCon ส่ง Jarvis Bot Conduct v1.0 — CLAUDE_jarvis_conduct.md ใน DocCon-Oracle repo. ใช้เป็น reference สำหรับ bot development ทุก response ต้องผ่าน 8 sections นี้"` + cc bob
- `/talk-to qa "DocCon ส่ง Jarvis Bot Conduct v1.0 — ใช้ Appendix C Audit Checklist ตรวจ bot ทุก round"` + cc bob
- `/talk-to writer "DocCon ส่ง Jarvis Bot Conduct v1.0 — Bot Copy ต้อง align กับ §1 Tone, §6 Language"` + cc bob
- `/talk-to aia "DocCon ส่ง Jarvis Bot Conduct v1.0 — ขอ domain expert review ว่าครบไหม"` + cc bob
- `/talk-to dev "DocCon ส่ง FA Tools Code Conduct v1.0 — CLAUDE_fatools_code_conduct.md. 🔴 CRITICAL: .env committed กับ Supabase keys ต้อง fix วันนี้"` + cc bob

### 2. Security CRITICAL — FA Tools .env
- `.env` committed กับ Supabase keys ใน iagencyaiafatools repo
- ต้อง: เพิ่ม .gitignore + git rm --cached + rotate keys
- Coordinate กับ Dev/BotDev/Admin

### 3. AIA Email Audit (ค้างจาก Session 2)
AIA ต้องส่ง email ใหม่ 3 ฉบับตาม Email Conduct v3.0:
- [ ] [AIA-Oracle] ePOS Daily Report — 19 มี.ค. 2026
- [ ] [AIA-Oracle] สรุปการเงินตัวแทน — 19 มี.ค. 2026
- [ ] [AIA-Oracle] สรุปผลแข่งขันคุณวุฒิ/สโมสร — 19 มี.ค. 2026

### 4. PA-Oracle Email Review
- PA-Oracle จะส่ง "[PA-Oracle] สรุปภาพรวมการเงิน Q1/2026"
- **ห้ามส่งจนกว่าแก้สี** — ใช้ navy/gold (ผิด) ต้องเป็น AIA Red #C8102E
- ขาด date ใน subject

---

## Session 3 Deliverables

| Deliverable | File | Status |
|-------------|------|--------|
| Jarvis Bot Conduct v1.0 | `CLAUDE_jarvis_conduct.md` | ✅ เสร็จ |
| FA Tools Code Conduct v1.0 | `CLAUDE_fatools_code_conduct.md` | ✅ เสร็จ |
| AIA Brand Guidelines | `AIA-Knowledge/brand-guidelines.md` | ✅ Pushed GitHub |

## Session Loops (รันทุก session — ยังไม่ได้ทำ session นี้!)

1. ⚙️ conduct-review — commits, emails
2. ⚙️ loop-followup — AIA ePOS (10/12/14/17), gmail-filter
3. ⚙️ law5-audit — /rrr + commit + push + task logs
4. ⚙️ session-monitor — maw peek + wake stuck oracles
5. ⚙️ rrr-reminder — broadcast เป็นระยะ
6. ⚙️ daily-report — สรุปให้ BoB ก่อนจบ session

**⚠️ Session นี้ skip loops ทั้งหมดเพราะ focus สร้าง documents — session 4 ต้อง catch up**

## New Info This Session

- **System URLs**: Dashboard :3456, iPad :#ipad, KnowledgeGraph :3000/graph, FA Tools :5173, Jarvis API localhost:3200
- **AIA Brand Colors 2 ชุด**: Core (Red #C8102E) + High Networth Prestige (Dark Green #4A5B2D, Charcoal #222B48, Olive #8E8C13)
- **Playwright limit**: Max 2 concurrent sessions — hook blocks automatically
- **Gemini proxy**: `~/.oracle/gemini-send.sh "question"` พร้อมใช้

## Key Files

- `CLAUDE_jarvis_conduct.md` — Jarvis Bot Conduct (8 sections, 87 Q&A baseline)
- `CLAUDE_fatools_code_conduct.md` — FA Tools Code Conduct (10 sections, 40+ violations, score 5.4/10)
- `CLAUDE_email.md` — Email Conduct v3.0 (AIA Red #C8102E)
- `CLAUDE_loops.md` — 6 SYSTEM loops
- `CLAUDE_safety.md` — Git/file safety rules

## Known Issues (ค้าง)

- Admin session ค้าง — ต้อง monitor
- Writer/Designer/Editor session empty — ต้อง wake
- QA qa-check.yml blocked on `gh auth refresh -s workflow`
- cc-BoB compliance — ยังไม่ได้ audit

---

*DocCon — Session 3 wrap | "สร้างมาตรฐาน ไม่ใช่แค่ตรวจ"*
