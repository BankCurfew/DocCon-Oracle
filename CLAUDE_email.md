# Email Conduct Standard — บังคับใช้ทุก Oracle

> **MANDATORY** — ทุก oracle ต้องอ่านและปฏิบัติตามก่อนส่ง email ให้แบงค์
> Reference: ePOS Daily Report 15-16 มี.ค. 2026 (approved by แบงค์)
> Version: 2.0 | Updated: 2026-03-19 | Author: DocCon-Oracle
> Editor PASS: v1.1 (2026-03-18)

---

## ⚠️ สาเหตุที่ต้องบังคับใช้

Email 18-19 มี.ค. 2026 หลุด standard ทั้งหมด:
- ❌ ไม่มี navy/gold theme — ใช้ plain HTML
- ❌ Tables ไม่มี navy header row — ใช้ plain table
- ❌ ไม่มี pill status badges — ใช้ emoji แทน
- ❌ ไม่มี highlight boxes — ใช้ plain text
- ❌ ไม่มี gold underline header
- ❌ Subject format ไม่ consistent (`[ePOS]` vs `[AIA]` vs `[AIA-Oracle]`)

**Reference ที่ถูกต้อง**: ePOS email 15-16 มี.ค. (มี navy/gold HTML styling ครบ)

---

## 1. Body Wrapper (ทุก email ต้องเริ่มด้วย)

```html
<body style="font-family: 'Segoe UI', Arial, sans-serif; color: #1a1a1a; max-width: 640px; margin: 0 auto; padding: 20px; line-height: 1.6;">
```

## 2. Subject Line

Format บังคับ: `[AIA-Oracle] Subject — Date`

| ✅ ถูก | ❌ ผิด |
|--------|--------|
| `[AIA-Oracle] ePOS Daily Report — 19 มี.ค. 2026` | `[AIA] ePOS Daily — 19 มี.ค. 2026 รอบ 2` |
| `[DocCon] Daily Gmail Digest — 19 มี.ค. 2026` | `[ePOS] 18 มี.ค. 2026 — แก้ไข: ...` |
| `[HR] Weekly Performance Review — 21 มี.ค. 2026` | `สรุปรายได้ AIA ePOS — 15 มี.ค.` |

กฎ:
- `[Oracle-Name]` — ใช้ชื่อเต็ม ไม่ย่อ (AIA-Oracle ไม่ใช่ AIA)
- Subject สั้น ชัดเจน
- วันที่ท้ายเสมอ

## 3. Header (h2 + gold underline)

```html
<h2 style="color: #0d2f5e; border-bottom: 2px solid #c5a044; padding-bottom: 8px;">
  ePOS Daily Report — 19 มี.ค. 2026
</h2>
<p style="color: #666; font-size: 13px;">ข้อมูล ณ 19/03/2026 | สรุปโดย AIA-Oracle</p>
```

| ✅ ถูก | ❌ ผิด |
|--------|--------|
| `<h2 style="color: #0d2f5e; border-bottom: 2px solid #c5a044; ...">` | `📋 ePOS Daily Report` (plain text) |

## 4. Section Headers (h3 + semantic colors)

```html
<h3 style="color: #2e7d32;">✅ อนุมัติแล้ว</h3>
<h3 style="color: #e65100;">⏳ คงค้างรอพิจารณา</h3>
<h3 style="color: #0d2f5e;">📊 สรุปภาพรวม</h3>
<h3 style="color: #c62828;">🎯 Action Items</h3>
```

| ✅ ถูก | ❌ ผิด |
|--------|--------|
| `<h3 style="color: #2e7d32;">✅ อนุมัติแล้ว</h3>` | `🏥 ประกันชีวิต (Life Insurance)` (no color) |

## 5. Tables (navy header row — บังคับ)

```html
<table style="width: 100%; border-collapse: collapse; font-size: 14px;">
  <tr style="background: #0d2f5e; color: white;">
    <th style="padding: 8px; text-align: left;">ลูกค้า</th>
    <th style="padding: 8px; text-align: right;">ทุนประกัน</th>
    <th style="padding: 8px; text-align: right;">เบี้ย/ปี</th>
    <th style="padding: 8px; text-align: center;">สถานะ</th>
  </tr>
  <tr style="background: #f5f5f5;">
    <td style="padding: 8px;">น.ส. ณัฐณิชา ปัญญามโน<br><span style="color: #888; font-size: 12px;">U887623354 | 5ILWH5</span></td>
    <td style="padding: 8px; text-align: right; font-weight: bold;">20,000,000</td>
    <td style="padding: 8px; text-align: right; font-weight: bold; color: #2e7d32;">1,093,161</td>
    <td style="padding: 8px; text-align: center;"><span style="background: #2e7d32; color: white; padding: 2px 8px; border-radius: 10px; font-size: 12px;">มีผลบังคับ</span></td>
  </tr>
  <tr style="background: #fff3e0;">
    <td style="padding: 8px;">นาง สุดารัตน์ ชาญเฉลิมชัย<br><span style="color: #888; font-size: 12px;">T249134180 | 20PLAND</span></td>
    <td style="padding: 8px; text-align: right;">100,000</td>
    <td style="padding: 8px; text-align: right;">99,911</td>
    <td style="padding: 8px; text-align: center;"><span style="background: #e65100; color: white; padding: 2px 8px; border-radius: 10px; font-size: 12px;">UW คงค้าง</span></td>
  </tr>
</table>
```

กฎ:
- Header row: **navy `#0d2f5e`** background, white text — บังคับ
- Data rows: `#f5f5f5` (ปกติ) / `#fff3e0` (warning/pending)
- Padding: `8px` ทุก cell
- Numbers: align right + bold
- ชื่อลูกค้า: เลขกรมธรรม์ใน `<span>` ขนาดเล็กด้านล่าง

| ✅ ถูก | ❌ ผิด |
|--------|--------|
| `<tr style="background: #0d2f5e; color: white;">` | `<table>` plain ไม่มี styling |
| Navy header + pill badges | Emoji-only status (🔴 🟡 ✅) |

## 6. Status Badges (pill style — บังคับ)

```html
<!-- Success/Approved -->
<span style="background: #2e7d32; color: white; padding: 2px 8px; border-radius: 10px; font-size: 12px;">มีผลบังคับ</span>

<!-- Warning/Pending -->
<span style="background: #e65100; color: white; padding: 2px 8px; border-radius: 10px; font-size: 12px;">UW คงค้าง</span>

<!-- Urgent -->
<span style="background: #c62828; color: white; padding: 2px 8px; border-radius: 10px; font-size: 12px;">รอชำระเบี้ย</span>
```

| ✅ ถูก | ❌ ผิด |
|--------|--------|
| `<span style="background: #2e7d32; ...">มีผลบังคับ</span>` | `✅ มีผลบังคับ` (text only) |
| `<span style="background: #c62828; ...">รอชำระ 7d</span>` | `🔴 รอชำระ 7d` (emoji only) |

## 7. Highlight Boxes

```html
<!-- Success summary -->
<div style="background: #e8f5e9; border-left: 4px solid #2e7d32; padding: 12px; margin: 12px 0; border-radius: 4px;">
  <strong>รายได้อนุมัติใหม่: ฿1,093,161/ปี</strong>
</div>

<!-- Warning/Urgent -->
<div style="background: #fff3e0; border-left: 4px solid #e65100; padding: 12px; margin: 12px 0; border-radius: 4px;">
  <strong>เบี้ยคงค้างรวม: ฿1,099,911</strong>
</div>

<!-- Urgent alert -->
<div style="background: #ffebee; border-left: 4px solid #c62828; padding: 12px; margin: 12px 0; border-radius: 4px;">
  <strong>⚠️ URGENT: ณัฐณิชา รอชำระเบี้ย 1,000,000 บาท — ค้าง 7 วัน</strong>
</div>
```

## 8. Action Items (บังคับทุก email)

```html
<h3 style="color: #c62828;">🎯 Action Items</h3>
<ol style="line-height: 1.8;">
  <li><strong>น.ส. ณัฐณิชา</strong> (U887623367) — อนุมัติเบื้องต้นแล้ว <strong>รอชำระเบี้ย ฿1,000,000</strong> → ติดตามลูกค้าด่วน</li>
  <li><strong>นาง สุดารัตน์</strong> (T249134180) — UW คงค้าง → ติดตามผลพิจารณา</li>
</ol>
```

ถ้าไม่มี action: `<p>ไม่มี action items ที่ต้องทำ</p>`

## 9. Footer

```html
<hr style="border: none; border-top: 1px solid #ddd; margin: 20px 0;">
<p style="color: #999; font-size: 11px; text-align: center;">
  สรุปจาก AIA-Oracle | Confidential — Do not forward<br>
  Source: ePOS (agent.aia.co.th) | Data as of 19/03/2026
</p>
```

---

## Color Reference (ห้ามใช้สีอื่น)

| Element | Hex | Usage |
|---------|-----|-------|
| Navy | `#0d2f5e` | Headers, table header row, neutral sections |
| Gold | `#c5a044` | Header underline border |
| Green | `#2e7d32` | Success badges, approved items, success boxes |
| Orange | `#e65100` | Warning badges, pending items, warning boxes |
| Red | `#c62828` | Urgent badges, action items header, urgent boxes |
| Light green bg | `#e8f5e9` | Success highlight box background |
| Light orange bg | `#fff3e0` | Warning highlight box background + warning rows |
| Light red bg | `#ffebee` | Urgent highlight box background |
| Gray row | `#f5f5f5` | Normal data rows |
| Gray text | `#666` | Subtext, dates |
| Small text | `#888` | กรมธรรม์ numbers, secondary info |
| Muted footer | `#999` | Footer text |

---

## DocCon Audit Checklist

ทุก email ที่ oracle ส่งให้แบงค์ DocCon จะตรวจ:

- [ ] **Body wrapper**: `font-family: 'Segoe UI'`, `max-width: 640px`, `line-height: 1.6`
- [ ] **Subject**: `[Oracle-Name] Subject — Date`
- [ ] **Header**: h2 navy + gold underline `border-bottom: 2px solid #c5a044`
- [ ] **Tables**: navy header row `background: #0d2f5e; color: white`
- [ ] **Status badges**: pill style `border-radius: 10px` + correct colors
- [ ] **Highlight boxes**: `border-left: 4px solid` + correct background
- [ ] **Action items**: h3 red + ordered list — ต้องมีเสมอ
- [ ] **Footer**: source + date + oracle name
- [ ] **No emoji-only status** — ต้องใช้ pill badges
- [ ] **No plain tables** — ต้องมี navy header
- [ ] **No secrets/credentials**

## Violations Found (19 มี.ค. 2026)

| Email | Violations |
|-------|-----------|
| `[AIA] ePOS Daily — 19 มี.ค. รอบเช้า` | Subject format ผิด, ไม่มี navy/gold theme, plain tables, emoji-only status, ไม่มี highlight boxes |
| `[AIA] ePOS Daily — 19 มี.ค. รอบ 2` | เหมือนข้างบน |
| `[ePOS] 18 มี.ค. — แก้ไข` | Subject format ผิด (ไม่มี Oracle name), ไม่มี theme |
| `[AIA] ePOS Daily — 18 มี.ค. รอบ 17:00` | Subject format ผิด, ไม่มี theme |

## Enforcement

1. **NOTICE** — email ไม่ตาม standard → แจ้ง oracle พร้อมระบุจุดที่ผิด
2. **WARNING** — ซ้ำครั้งที่ 2 → warning + cc BoB + ส่ง reference email ตัวอย่าง
3. **ESCALATE** — ไม่แก้ → escalate BoB

## Applies To

**ทุก oracle** ที่ส่ง email ให้แบงค์ — ไม่มีข้อยกเว้น:
AIA, DocCon, HR, QA, Researcher, Writer, Editor, Designer, Dev, BotDev, Data, Admin, Creator

---

*"Consistency is the hallmark of quality." — DocCon*
*Reference: ePOS Daily Report 15 มี.ค. 2026 — the gold standard*
