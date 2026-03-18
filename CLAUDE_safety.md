# DocCon-Oracle — Safety Rules

> อ่านก่อนทำ git/file operation ทุกครั้ง

## Git Safety

### ห้ามเด็ดขาด
- `git push --force` — violates "Nothing is Deleted"
- `git reset --hard` — destroys uncommitted work
- `git commit --amend` — breaks hash for other agents
- `rm -rf` — ต้อง archive ไม่ใช่ลบ
- Commit secrets (.env, credentials, API keys)

### ต้องทำเสมอ
- Branch naming: `feature/xxx`, `fix/xxx`, `docs/xxx`
- Commit format: `type: description` (feat/fix/refactor/docs/chore)
- Pull before push: `git pull --rebase origin main`
- Review before commit: `git diff --staged`

## File Safety

### ห้ามแก้ไข
- `.env` files — ไม่แตะ ไม่อ่าน ไม่ commit
- `credentials.json`, `*.pem`, `*.key` — ไม่แตะเด็ดขาด
- Files outside DocCon-Oracle repo — ใช้ `git -C` อ่านได้ แต่ห้ามแก้

### ก่อนลบไฟล์
1. ตรวจว่าไม่มี reference ที่อื่น
2. Archive ก่อนลบ (copy to ψ/archive/)
3. Commit archive ก่อน commit deletion

## Oracle-v2 Rules

### MCP Tools — ใช้อย่างระวัง
- `oracle_learn` — ตรวจ content ก่อน save (ไม่บันทึก secrets)
- `oracle_supersede` — ใช้แทน delete (preserves chain)
- `oracle_trace` — log ทุก discovery

### Inter-Oracle Communication
- ใช้ `maw hey` เท่านั้น — ห้ามแก้ไฟล์ใน repo oracle อื่นโดยตรง
- cc BoB ทุกครั้งที่คุยข้าม oracle
- ตอบ maw hey ภายใน 2 นาที

## Conduct-Specific Safety

### เมื่อตรวจ oracle อื่น
- **อ่านอย่างเดียว** — ใช้ `git -C`, `cat`, `rg` ดู repo อื่น
- **ห้ามแก้ไข** — DocCon ไม่มีสิทธิ์แก้ repo อื่น
- **Evidence-based** — ต้องมี proof (log, commit, file) ก่อน flag violation
- **ข้อเท็จจริง** — "commit X ไม่มี type prefix" ไม่ใช่ "commit X ไม่ดี"

---

*"Measure twice, cut once." — DocCon Safety*
