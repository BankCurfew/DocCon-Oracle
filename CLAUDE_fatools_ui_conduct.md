# FA Tools — UI Conduct & Visual Audit Report v1.0

> Playwright scan: 12 pages + 2 dark mode | CSS analysis | Code audit
> Date: 2026-03-19 | Author: DocCon-Oracle

---

## Executive Summary

**Scanned**: 14 screenshots (12 routes + 2 dark mode)
**Public pages visible**: Index, Auth, Manual, API Docs, 404
**Auth-protected** (redirect to login): Dashboard, Profile, Admin, Pending Approval, Analyze Policy, Simulator, Funds

**Overall UI Score: 7/10** — ดีในภาพรวม แต่มีปัญหาสีไม่ตรง brand + font ไม่ตรง AIA guidelines

---

## Issue 1: 🔴 สีแดงหลัก (Primary Red) — 3 ค่าต่างกัน!

นี่คือปัญหาใหญ่ที่สุด — มี "AIA Red" 3 เฉดต่างกันใน codebase:

| Where | Hex | HSL | ใครกำหนด |
|-------|-----|-----|---------|
| **CSS variable `--primary`** | `#D41145` | `344 85% 45%` | index.css:79 |
| **Hardcoded ใน exports** | `#C8102E` | — | export components (20+ instances) |
| **แบงค์บอก** | `#E11337` | — | แบงค์สั่งตรง |
| **AIA Official (Pantone 1935C)** | `#C8102E` | — | AIA Brand Guidelines |
| **Dark mode CSS var** | `#EB134C` | `344 85% 50%` | index.css:160 |

**ปัญหา**: UI แสดงสี `#D41145` (สว่างกว่า AIA official) แต่ export PDF ใช้ `#C8102E` (ถูกต้อง) — ลูกค้าเห็นสีต่างกันระหว่าง app กับ PDF!

**Evidence**:
- `src/index.css:79` → `--primary: 344 85% 45%` = `#D41145`
- `src/components/export/ExportHeader.tsx:38` → `color: '#c8102e'`
- `src/components/export/PlanExportContent.tsx:68` → `primary: '#c8102e'`
- Comment ใน CSS บอก `/* AIA Red #d31145 */` — ไม่ตรงกับ AIA official `#C8102E`

**Screenshot**: ดู `01-index.png` — Hero background สีแดง ≈ `#D41145` ไม่ใช่ `#C8102E`

### Fix Required

```css
/* index.css:79 — เปลี่ยนจาก */
--primary: 344 85% 45%; /* AIA Red #d31145 */

/* เป็น AIA Official */
--primary: 345 85% 42%; /* AIA Red #C8102E (Pantone 1935C) */
```

> **หมายเหตุ**: แบงค์บอก `#E11337` ซึ่งไม่ตรงกับ AIA Official `#C8102E` เช่นกัน — ต้อง confirm กับแบงค์ว่าจะใช้ค่าไหนเป็น single source of truth

---

## Issue 2: 🔴 Font ไม่ตรง AIA Brand Guidelines

| AIA Guidelines กำหนด | FA Tools ใช้จริง | ตรง? |
|---------------------|-----------------|------|
| **TH**: Cordia New Bold / Cordia New | **LINESeedSansTH** (5 weights) | ❌ |
| **EN**: Arial Bold / Arial Regular | **Inter** (Variable font) | ❌ |
| **Prestige**: Helvetica Neue | ไม่มี | ❌ |

**Evidence**:
- `src/index.css:2-27` — Load LINESeedSansTH (Th, Rg, Bd, XBd, He)
- `src/index.css:32-40` — Load Inter Variable
- Playwright scan: ทุกหน้าใช้ `LINESeedSansTH, system-ui, -apple-system, sans-serif`

**Impact**: ผิด AIA brand แต่ LINESeedSansTH อ่านง่ายกว่า Cordia New บน screen — อาจเป็น intentional design choice

### Recommendation

ต้อง confirm กับแบงค์:
- **Option A**: เปลี่ยนเป็น Cordia New + Arial ตาม AIA guidelines (strict compliance)
- **Option B**: ใช้ LINESeedSansTH + Inter ต่อ (better UX, ไม่ strict AIA) — ต้อง document ว่าเป็น intentional deviation

---

## Issue 3: 🟡 Dark Mode — ไม่มีผลจริง

**จากการ scan**: Index page light mode และ dark mode **เหมือนกันทุกประการ**

| Element | Light Mode | Dark Mode | ต่างกัน? |
|---------|-----------|-----------|---------|
| Hero background | Red gradient | Red gradient | ❌ เหมือนกัน |
| Card background | Semi-transparent white | Semi-transparent white | ❌ เหมือนกัน |
| Text color | White | White | ❌ เหมือนกัน |
| Footer | Dark background | Dark background | ❌ เหมือนกัน |

**Auth page**: Light vs Dark — เหมือนกันทุกประการเช่นกัน

**Evidence**: Compare `01-index.png` vs `13-index-dark.png` — identical

**CSS**: Dark mode vars exist (index.css:160) แต่ hero/auth ใช้ hardcoded colors ไม่ใช้ CSS variables

### Fix Required

ถ้าจะ support dark mode:
- Hero section ต้องปรับ: dark mode ใช้ deeper red หรือ dark background + red accents
- Auth form: dark mode ใช้ dark card background แทน semi-transparent red
- ถ้าไม่ support: ลบ dark mode toggle / CSS vars ออกเพื่อลด confusion

---

## Issue 4: 🟡 Button Styles — 2 แบบปนกัน

### Cookie Consent Bar (ทุกหน้า)

| Button | Style | Background | Border |
|--------|-------|-----------|--------|
| "ตั้งค่าคุกกี้" | Outline | Transparent | Border visible |
| "ยอมรับทั้งหมด" | Filled | Red/Pink `#D41145` | None |

**Verdict**: OK — primary/secondary button pattern ถูกต้อง (filled = primary action, outline = secondary)

### Manual Page — 21 buttons

Sidebar navigation ใช้ active state สีชมพู/แดง background — consistent ดี

### API Docs — 35 buttons

ใช้ teal/green badges สำหรับ GET methods — **ปนกับ red brand** แต่เป็น convention ของ API docs (GET=green, POST=blue, etc.) — acceptable

### Auth Page

"เข้าสู่ระบบ" button: **white background + red text** — ไม่ใช่ red button + white text

**Issue**: ปุ่มหลักของ auth page ไม่ prominent พอ — ควรเป็น filled red button

---

## Issue 5: 🟡 Spacing & Layout

### Index Page (Landing)

| Element | Observation |
|---------|------------|
| Hero section | ✅ Good padding, centered |
| Feature cards (iQuick, iPlan, iCompare) | ✅ 3-column grid, consistent spacing |
| Card content | ✅ Icon → Title → Description → CTA consistent |
| Footer | ✅ Proper spacing |
| Cookie banner | ⚠️ Overlaps with bottom row of cards — blocks "เริ่มใช้งาน" links |

### Manual Page

| Element | Observation |
|---------|------------|
| Sidebar width | ✅ Consistent ~250px |
| Content area | ✅ Proper padding |
| Accordion items | ✅ Even spacing |
| Active item highlight | ✅ Red background, good contrast |

### API Docs

| Element | Observation |
|---------|------------|
| Left sidebar | ✅ Category list with counts |
| Right content | ✅ Structured API reference |
| Code blocks | ✅ Proper font (monospace) |
| **Auth + Error Cards** | ⚠️ Cards at bottom-left feel disconnected from main content |

### 404 Page

| Element | Observation |
|---------|------------|
| Layout | ⚠️ Too empty — centered text only, no illustration, feels unfinished |
| "Return to Home" link | Red text (brand color) — OK |
| Background | Light gray — doesn't match dark theme of other pages |

---

## Issue 6: 🟡 Icon Styles

| Page | Icon Style | Consistent? |
|------|-----------|-------------|
| Index (feature cards) | Lucide outline icons | ✅ |
| Manual sidebar | Lucide outline icons | ✅ |
| API Docs | Text badges (GET, POST) | ✅ (different context OK) |
| Cookie banner | Cookie icon (outline) | ✅ |

**Verdict**: Icons are consistent — all Lucide outline style. No mixing issue found.

---

## Issue 7: 🟡 Professional Polish

### สิ่งที่ดูไม่ professional:

| # | Issue | Page | Severity |
|---|-------|------|----------|
| 1 | **Cookie banner บัง content** — ปิดบัง row ที่ 2 ของ feature cards | Index | 🟡 |
| 2 | **404 page ว่างเกินไป** — แค่ "404" + "Oops! Page not found" + link ไม่มี illustration | 404 | 🟡 |
| 3 | **Auth page ปุ่ม login ไม่ prominent** — white button on pink background ไม่โดดเด่น | Auth | 🟡 |
| 4 | **Mixed language** — "Don't have an account? Sign up" อยู่กับ "ยังไม่มีบัญชี? สมัครเลย" ซ้ำกัน 2 ภาษา | Auth | 🔴 |
| 5 | **Footer ยาวมาก** — มี disclaimer + copyright + version + ข้อความกฎหมายยาว 2 บรรทัด | Index | 🟡 |
| 6 | **"เข้าสู่ระบบ" placeholder ภาษาอังกฤษ** — "Your email address" / "Your password" ไม่ match labels ภาษาไทย | Auth | 🟡 |

---

## Full Issue Summary

| # | Category | Issue | Severity | Fix |
|---|----------|-------|----------|-----|
| UI-1 | **Color** | Primary red ใน UI (`#D41145`) ≠ export (`#C8102E`) ≠ AIA official | 🔴 | Unify to single hex |
| UI-2 | **Color** | Dark mode primary (`#EB134C`) สว่างกว่า light mode | 🟡 | ปรับ dark mode var |
| UI-3 | **Font** | ใช้ LINESeedSansTH ไม่ใช่ Cordia New (AIA brand) | 🔴 | Confirm กับแบงค์ |
| UI-4 | **Font** | ใช้ Inter ไม่ใช่ Arial (AIA brand) | 🔴 | Confirm กับแบงค์ |
| UI-5 | **Dark Mode** | Light/Dark mode เหมือนกันทุกประการ (hero, auth) | 🟡 | Fix หรือ ลบ dark mode |
| UI-6 | **Button** | Auth login button ไม่ prominent (white on pink) | 🟡 | เปลี่ยนเป็น filled red |
| UI-7 | **Spacing** | Cookie banner บัง feature cards row 2 | 🟡 | ย้าย banner ขึ้นบน หรือ push content |
| UI-8 | **Language** | Auth: English + Thai signup links ซ้ำกัน 2 ภาษา | 🔴 | เลือกภาษาเดียว |
| UI-9 | **Language** | Auth: label ไทย แต่ placeholder อังกฤษ | 🟡 | ทำให้ consistent |
| UI-10 | **Polish** | 404 page ว่างเปล่า ไม่มี illustration | 🟡 | เพิ่ม graphic + branding |
| UI-11 | **Polish** | Footer ข้อความกฎหมายยาวเกินไป | 🟡 | ย่อ + ซ่อนใน expandable |
| UI-12 | **Prestige** | Prestige mode ใช้ `#7a6a4f` ไม่ใช่ AIA Dark Green `#4A5B2D` | 🟡 | ตรวจ vs brand guidelines |

---

## Color Discrepancy Decision Required

แบงค์ต้อง decide:

| Option | Primary Red | Source |
|--------|-----------|--------|
| **A** | `#C8102E` | AIA Official (Pantone 1935C) — ใช้ใน exports แล้ว |
| **B** | `#D41145` | Current CSS var — สว่างกว่า AIA official |
| **C** | `#E11337` | แบงค์บอกในคำสั่ง — สว่างกว่าทั้ง 2 ค่า |

**DocCon Recommendation**: ใช้ `#C8102E` (AIA Official) ทุกที่ — เป็น Pantone 1935C ตาม brand guidelines ที่แบงค์ส่งมาเอง

---

## Screenshots Reference

| File | Page | Key Finding |
|------|------|-------------|
| `01-index.png` | Landing | Red hero, 6 feature cards, cookie banner |
| `02-auth.png` | Login | Mixed language, white login button |
| `08-manual.png` | Manual | Sidebar nav, good structure |
| `11-api-docs.png` | API Reference | 25 endpoints, good layout |
| `12-notfound.png` | 404 | Empty, unprofessional |
| `13-index-dark.png` | Landing (dark) | Identical to light mode |
| `14-auth-dark.png` | Auth (dark) | Identical to light mode |

Screenshots saved at: `/tmp/fatools-ui-audit/`

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| v1.0 | 2026-03-19 | Initial audit — 12 issues found |

---

*"Design is not just what it looks like. Design is how it works — and how consistent it feels." — DocCon*
