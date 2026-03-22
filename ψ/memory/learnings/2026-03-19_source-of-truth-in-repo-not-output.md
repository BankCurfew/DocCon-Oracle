# Learning: Source of Truth อยู่ใน Repo ไม่ใช่ Output

**Date**: 2026-03-19
**Source**: Email Conduct v2.0→v3.0 — ใช้สีผิด 2 รอบ

## Pattern

เมื่อต้องสร้าง standard หรือ audit output → ดู source of truth ใน repo (templates, config, CLAUDE.md) ก่อนเสมอ ไม่ใช่ reverse-engineer จาก output (email, rendered HTML, screenshots).

## Why

ผม reverse-engineer สีจาก Draft email (navy/gold) แทนที่จะดู CLAUDE_templates.md ใน AIA repo (AIA Red #C8102E). Draft ≠ สิ่งที่ส่งจริง. Gmail API strip HTML ออก ดูสีจริงไม่ได้. เสียเวลาแก้ 3 รอบ + แบงค์ต้องบอก 2 ครั้ง.

## How to Apply

เมื่อต้อง audit หรือสร้าง standard:
1. **ดู repo templates ก่อน** — CLAUDE_templates.md, config files, style guides
2. **ดู repo code ก่อน** — ไม่ใช่ output ที่ render แล้ว
3. **Gmail API strip HTML** — ห้ามเอาสีจาก API output ต้องดู source code
4. **Draft ≠ Sent** — ตรวจ label ให้แน่ใจว่าเป็น email ที่ส่งจริง ไม่ใช่ draft
