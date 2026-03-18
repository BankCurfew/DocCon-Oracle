# Email Conduct Standard — ทุก Oracle ต้องปฏิบัติตาม

> Reference: ePOS Daily Report 15 มี.ค. 2026 — approved by แบงค์
> Created: 2026-03-18 | Author: DocCon-Oracle | Status: Active

---

## Theme & Colors

| Element | Value |
|---------|-------|
| **Primary (headers, tables)** | Navy `#0d2f5e` |
| **Accent (borders, highlights)** | Gold `#c5a044` |
| **Success** | Green `#2e7d32` |
| **Warning/Pending** | Orange `#e65100` |
| **Urgent/Action** | Red `#c62828` |
| **Muted text** | Gray `#666` |
| **Font** | `'Segoe UI', Arial, sans-serif` |
| **Max width** | `640px` |
| **Body color** | `#1a1a1a` |
| **Body line-height** | `1.6` |
| **Paragraph spacing** | `margin-bottom: 12px` |

## Subject Line Standard

ทุก email ต้องใช้ format: `[Oracle] Subject — Date`

ตัวอย่าง:
- `[AIA] ePOS Daily Report — 18 มี.ค. 2026`
- `[DocCon] Daily Gmail Digest — 18 มี.ค. 2026`
- `[HR] Weekly Performance Review — 21 มี.ค. 2026`
- `[QA] Test Report: Jarvis Bot — 18 มี.ค. 2026`

## Structure — ทุก email ต้องมี

### 1. Header (h2)
```html
<h2 style="color: #0d2f5e; border-bottom: 2px solid #c5a044; padding-bottom: 8px;">
  [SUBJECT LINE]
</h2>
<p style="color: #666; font-size: 13px;">ข้อมูล ณ [DATE] | สรุปโดย [ORACLE-NAME]</p>
```

### 2. Content Sections (h3)
```html
<h3 style="color: #2e7d32;">✅ [Section — success/completed]</h3>
<h3 style="color: #e65100;">⏳ [Section — pending/warning]</h3>
<h3 style="color: #0d2f5e;">📊 [Section — summary/neutral]</h3>
<h3 style="color: #c62828;">🎯 [Section — action items]</h3>
```

### 3. Tables
```html
<table style="width: 100%; border-collapse: collapse; font-size: 14px;">
  <tr style="background: #0d2f5e; color: white;">
    <th style="padding: 8px; text-align: left;">[Column]</th>
  </tr>
  <tr style="background: #f5f5f5;">
    <td style="padding: 8px;">[Data]</td>
  </tr>
</table>
```

- Header row: navy background `#0d2f5e`, white text
- Alternating rows: `#f5f5f5` / `#fff3e0` (สำหรับแถว warning)
- Padding: `8px` ทุก cell
- Numbers align right, text align left

### 4. Status Badges
```html
<!-- Success -->
<span style="background: #2e7d32; color: white; padding: 2px 8px; border-radius: 10px; font-size: 12px;">อนุมัติ</span>

<!-- Warning -->
<span style="background: #e65100; color: white; padding: 2px 8px; border-radius: 10px; font-size: 12px;">คงค้าง</span>

<!-- Urgent -->
<span style="background: #c62828; color: white; padding: 2px 8px; border-radius: 10px; font-size: 12px;">รอชำระ</span>
```

### 5. Highlight Boxes
```html
<!-- Success box -->
<div style="background: #e8f5e9; border-left: 4px solid #2e7d32; padding: 12px; margin: 12px 0; border-radius: 4px;">
  <strong>[Key takeaway]</strong>
</div>

<!-- Warning box -->
<div style="background: #fff3e0; border-left: 4px solid #e65100; padding: 12px; margin: 12px 0; border-radius: 4px;">
  <strong>[Warning/pending info]</strong>
</div>
```

### 6. Action Items (ทุก email ต้องมี)
```html
<h3 style="color: #c62828;">🎯 Action Items</h3>
<ol style="line-height: 1.8;">
  <li><strong>[Who/What]</strong> — [Action needed]</li>
</ol>
```
- ถ้าไม่มี action → เขียน "ไม่มี action items ที่ต้องทำ"

### 7. Footer
```html
<hr style="border: none; border-top: 1px solid #ddd; margin: 20px 0;">
<p style="color: #999; font-size: 11px; text-align: center;">
  สรุปจาก [ORACLE-NAME]<br>
  Source: [DATA SOURCE] | Data as of [DATE]
</p>
```

## Checklist — DocCon ใช้ audit ทุก email

- [ ] Font: Segoe UI (ไม่ใช่ font อื่น)
- [ ] Colors: navy `#0d2f5e` + gold `#c5a044` (ไม่ใช่สีอื่น)
- [ ] Header: h2 + gold underline + date/source line
- [ ] Tables: navy header row, 8px padding, right-align numbers
- [ ] Status badges: rounded pill style, correct colors
- [ ] Highlight boxes: left border 4px, correct background
- [ ] Action items section: ต้องมีเสมอ
- [ ] Footer: source attribution + date + oracle name
- [ ] Max width: 640px
- [ ] Subject line: `[Oracle] Subject — Date` format
- [ ] Body text: line-height 1.6, margin-bottom 12px
- [ ] No secrets/credentials in email body

## Applies To

ทุก email ที่ oracle ส่งให้แบงค์:
- AIA: ePOS daily reports, portal updates
- DocCon: daily digest, conduct reports, Gmail filter reports
- HR: weekly performance reviews
- QA: test reports, QA summaries
- Researcher: research summaries
- Writer: content drafts (เมื่อส่งทาง email)
- Editor: review results, style guide updates
- Designer: design deliverables
- Dev/BotDev/Data/Admin/Creator: ทุก email ที่ส่งแบงค์

## Enforcement

1. **NOTICE** — email ไม่ตาม standard → แจ้ง oracle แก้ไข
2. **WARNING** — ซ้ำครั้งที่ 2 → warning + cc BoB
3. **ESCALATE** — ไม่แก้ → escalate BoB

---

*"Consistency is the hallmark of quality." — DocCon*
