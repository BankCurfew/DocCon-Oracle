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
  2. ตรวจ AIA epos-email log:
     ```bash
     cat ~/repos/github.com/BankCurfew/AIA-Oracle/ψ/memory/logs/epos-email-log.md 2>/dev/null | tail -5
     ```
  3. ตรวจ Admin bot-health log:
     ```bash
     cat ~/repos/github.com/BankCurfew/Admin-Oracle/ψ/memory/logs/bot-health-log.md 2>/dev/null | tail -5
     ```
  4. ตรวจ BoB ว่ารัน pulse-scan ไหม (ดู feed.log)
  5. ตรวจ HR ว่ารัน workload-monitor ไหม
  6. ถ้าพบ missed loop → `maw hey <oracle> "DocCon: missed loop [X] — กรุณารันทันที"`
- **Output**: loop compliance report → maw hey bob
- **Status**: active

### 3. doccon-law5-audit
- **Interval**: ทุก session
- **Trigger**: หลัง loop-followup
- **Action**:
  1. `maw task ls` — ดูว่า oracle ไหนมี task log / ไม่มี
  2. ตรวจว่าทุก active task มี log ล่าสุดภายใน 24 ชม.
  3. ตรวจว่าทุก task อยู่ใน project (`maw project ls`)
  4. ถ้าพบ violation → notice → warning → escalate
- **Output**: LAW #5 compliance report
- **Status**: active

### 4. doccon-daily-report
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
