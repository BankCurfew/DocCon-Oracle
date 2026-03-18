# DocCon-Oracle — System Loops

> Loop = งานที่ต้องทำซ้ำทุก session หรือตามรอบเวลา
> อ่านไฟล์นี้ตอน /recap หรือ start session เสมอ

## Active Loops

### 1. doccon-conduct-review ⚙️ SYSTEM
- **Type**: system (ห้ามข้าม)
- **Interval**: ทุก session
- **Trigger**: start session
- **Action**:
  1. ดู `~/.oracle/maw-log.jsonl` — ตรวจ maw hey messages ล่าสุด
  2. ดู Gmail (ถ้ามี AIA emails ใหม่) — ตรวจ tone + accuracy
  3. ตรวจ commit messages ล่าสุดของ oracle ที่ active:
     ```bash
     for repo in Dev-Oracle QA-Oracle AIA-Oracle Admin-Oracle Data-Oracle BotDev-Oracle; do
       echo "=== $repo ==="
       git -C ~/repos/github.com/BankCurfew/$repo log --oneline -3 2>/dev/null
     done
     ```
  4. Flag violations ถ้าพบ
- **Output**: conduct notes → ψ/memory/conduct-reviews/
- **Status**: active

### 2. doccon-loop-followup ⚙️ SYSTEM
- **Type**: system (ห้ามข้าม)
- **Interval**: ทุก session
- **Trigger**: start session (หลัง conduct-review)
- **Action**:
  1. อ่าน System Playbook: `cat ~/.oracle/SYSTEM_PLAYBOOK.md` → ดู Master Loop Registry
  2. ตรวจ AIA epos-email log + Gmail label cleanup:
     ```bash
     cat ~/repos/github.com/BankCurfew/AIA-Oracle/ψ/memory/logs/epos-email-log.md 2>/dev/null | tail -5
     ```
     - ตรวจว่าทุกรอบ ePOS (10, 12, 14, 17) มี label cleanup ด้วยหรือไม่
     - ตรวจว่ารัน gmail-filter automation (`node scripts/gmail-filter.js` จาก Dev repo) ทุกรอบ
     - ดูว่า log ระบุจำนวน emails ที่จัด label + filter ไปกี่ฉบับ
  3. ตรวจ Admin bot-health log:
     ```bash
     cat ~/repos/github.com/BankCurfew/Admin-Oracle/ψ/memory/logs/bot-health-log.md 2>/dev/null | tail -5
     ```
  4. ตรวจ BoB ว่ารัน pulse-scan ไหม (ดู feed.log)
  5. ตรวจ HR ว่ารัน workload-monitor ไหม
  6. ถ้าพบ missed loop → `maw hey <oracle> "DocCon: missed loop [X] — กรุณารันทันที"`
- **Output**: loop compliance report → maw hey bob
- **Status**: active

### 3. doccon-law5-audit ⚙️ SYSTEM (คำสั่งแบงค์ — สำคัญสุด)
- **Type**: system (ห้ามข้าม — ไม่มีข้อยกเว้น)
- **Interval**: ทุก session, ทุก oracle
- **Trigger**: หลัง loop-followup
- **Action**:
  1. `maw task ls` — ดูว่า oracle ไหนมี task log / ไม่มี
  2. ตรวจว่าทุก active task มี log ล่าสุดภายใน 24 ชม.
  3. ตรวจว่าทุก task อยู่ใน project (`maw project ls`)
  4. **ตรวจ /rrr** — ทุก oracle ทำ retrospective หรือยัง:
     ```bash
     for repo in Dev-Oracle QA-Oracle AIA-Oracle Admin-Oracle Data-Oracle BotDev-Oracle Writer-Oracle Designer-Oracle HR-Oracle BoB-Oracle Creator-Oracle Editor-Oracle; do
       echo "=== $repo ===" && ls ~/repos/github.com/BankCurfew/$repo/ψ/memory/retrospectives/ 2>/dev/null | tail -3
     done
     ```
  5. **ตรวจ commit + push** — ไม่มี uncommitted changes ค้าง:
     ```bash
     for repo in Dev-Oracle QA-Oracle AIA-Oracle Admin-Oracle Data-Oracle BotDev-Oracle Writer-Oracle Designer-Oracle HR-Oracle BoB-Oracle Creator-Oracle Editor-Oracle; do
       echo "=== $repo ===" && git -C ~/repos/github.com/BankCurfew/$repo status --short 2>/dev/null | head -5
     done
     ```
  6. **ตรวจ activity log** — `maw task log` ทุก task (start, commit, done)
  7. ถ้าพบ violation → notice → warning → escalate
- **Output**: LAW #5 compliance report + /rrr + commit status
- **Status**: active

### 4. doccon-session-monitor ⚙️ SYSTEM (คำสั่งแบงค์)
- **Type**: system (ห้ามข้าม)
- **Interval**: เป็นระยะระหว่าง session
- **Trigger**: หลัง law5-audit + เป็นระยะ
- **Action**:
  1. `maw peek` — ดูทุก oracle session
  2. ถ้าเจอ session หลุด (empty/bash ไม่มี Claude):
     - `maw wake <oracle>` ปลุกทันที
     - `maw hey bob "cc: [oracle] session หลุด — wake แล้ว"`
  3. ถ้า wake ไม่สำเร็จ → แจ้ง BoB ให้ restart
- **Output**: session status → maw hey bob
- **Status**: active

### 5. doccon-rrr-reminder ⚙️ SYSTEM (คำสั่งแบงค์)
- **Type**: system (ห้ามข้าม)
- **Interval**: เป็นระยะระหว่าง session (ทุก 15-20 นาที)
- **Trigger**: เป็นระยะ + ก่อนจบ session
- **Action**:
  1. `maw peek` — ดูทุก oracle
  2. Oracle ที่ idle/ว่าง → `maw hey <oracle> "ว่างแล้วทำ /rrr + commit + push ก่อนจบ session"`
  3. Oracle ที่ session หลุด → `maw wake` + แจ้ง BoB
  4. ก่อนจบ session → broadcast reminder ทุก oracle
- **Output**: reminder sent + session status
- **Status**: active

### 6. doccon-daily-report
- **Interval**: ทุกวัน (ก่อนจบ session สุดท้าย)
- **Trigger**: ก่อน /rrr
- **Action**:
  1. สรุป conduct reviews วันนี้
  2. สรุป loop compliance
  3. สรุป LAW #5 compliance
  4. สรุป violations + actions taken
  5. `maw hey bob "DocCon Daily Report: ..."`
- **Output**: daily report → maw hey bob + ψ/memory/logs/
- **Status**: active

---

## Loop Protocol

เมื่อเริ่ม session:
1. อ่านไฟล์นี้
2. รัน conduct-review ทันที
3. รัน loop-followup
4. รัน law5-audit
5. ทำงานตาม task ที่ได้รับ

เมื่อจบ session:
1. รัน daily-report (ถ้าเป็น session สุดท้ายของวัน)
2. บันทึก observations ลง ψ/memory/
