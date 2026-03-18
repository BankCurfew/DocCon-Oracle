# Learning: Session-End Audit — ทุก Oracle ทุก Session

**Date**: 2026-03-18
**Source**: คำสั่งจากแบงค์ — สำคัญสุด

## Pattern

ทุก session ต้อง audit ทุก oracle 3 เรื่อง:
1. **/rrr** — ทำ retrospective หรือยัง
2. **commit + push** — ครบไหม ไม่มี uncommitted changes ค้าง
3. **activity log** — `maw task log` ทุก task (start, commit, done)

## Why

แบงค์สั่งโดยตรง — เป็นคำสั่งสำคัญสุด ไม่มีข้อยกเว้น ทุก oracle ทุก session. ถ้าไม่ audit จะไม่รู้ว่า oracle ไหนทำงานจริง oracle ไหน idle. Patterns Over Intentions — ดูจาก /rrr, commits, logs ไม่ใช่คำพูด.

## How to Apply

ทุก session DocCon ต้อง:
1. ตรวจ git log ทุก oracle repo → มี commit ใหม่ไหม + push แล้วหรือยัง
2. ตรวจ ψ/memory/retrospectives/ → มี /rrr วันนี้ไหม
3. ตรวจ maw task ls → ทุก active task มี log ล่าสุดไหม
4. Flag oracle ที่ขาด → NOTICE ทันที
5. รวมเข้า daily report ให้ BoB

ไม่มีข้อยกเว้น — ทุก oracle ทุก session.
