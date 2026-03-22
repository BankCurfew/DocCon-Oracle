# FA Tools — Prestige Theme Conduct v2.0

> "Prestige คือ theme ไม่ใช่สีเดียว — ต้องครบทุก token" — แบงค์

**Date**: 2026-03-20
**Author**: DocCon-Oracle
**Status**: PROPOSAL — Designer ใส่ค่าสี → แบงค์ approve → FE implement

---

## Concept: Prestige = Full Theme System

Prestige ไม่ใช่แค่ "เปลี่ยนสีแดงเป็นทอง" — เป็น **theme ทั้งระบบ** ที่ต้องกำหนดทุก token:
backgrounds, text, borders, gradients, shadows, badges, cards, buttons, tables ทั้งหมด

**เมื่อ product tier = "Prestige"** → สลับ theme ทั้งชุด ไม่ใช่สลับแค่ primary color

---

## Prestige Theme CSS Variables — Full Specification

### สำหรับ Designer: ใส่ค่า HSL ทุกช่อง

```css
:root {
  /* ========================================
     PRESTIGE THEME — Designer fills values
     ทุกค่าต้องเป็น HSL format: H S% L%
     Reference: AIA High Networth Brand Guidelines
     ======================================== */

  /* --- Core Colors --- */
  --prestige-primary:            ;  /* สีหลัก (ทอง/olive) — badges, filled buttons, active tab */
  --prestige-primary-hover:      ;  /* Hover state ของ primary */
  --prestige-primary-foreground: ;  /* Text บน primary background (ปกติ = white) */

  --prestige-secondary:          ;  /* สีรอง — secondary buttons, subtle elements */
  --prestige-secondary-hover:    ;  /* Hover state ของ secondary */
  --prestige-secondary-foreground: ; /* Text บน secondary background */

  --prestige-accent:             ;  /* Accent — highlights, links, active indicators */
  --prestige-accent-foreground:  ;  /* Text บน accent background */

  /* --- Backgrounds --- */
  --prestige-bg:                 ;  /* Page/section background (เมื่ออยู่ใน prestige context) */
  --prestige-bg-subtle:          ;  /* Subtle background — card tints, row hovers */
  --prestige-bg-muted:           ;  /* Muted background — disabled states, faded areas */

  --prestige-card:               ;  /* Card background */
  --prestige-card-foreground:    ;  /* Card text */

  /* --- Text --- */
  --prestige-text:               ;  /* Primary text color */
  --prestige-text-secondary:     ;  /* Secondary/muted text */
  --prestige-text-accent:        ;  /* Highlighted text (prices, important values) */

  /* --- Borders --- */
  --prestige-border:             ;  /* Default border */
  --prestige-border-strong:      ;  /* Strong border — active cards, focus rings */
  --prestige-border-subtle:      ;  /* Subtle border — dividers, separators */

  /* --- Gradients --- */
  --prestige-gradient-hero:      ;  /* Hero/header background gradient */
                                    /* Format: linear-gradient(135deg, hsl(X), hsl(Y)) */
  --prestige-gradient-card:      ;  /* Card header gradient */
  --prestige-gradient-badge:     ;  /* Badge/pill gradient (if needed, else use solid) */

  /* --- Shadows --- */
  --prestige-shadow-sm:          ;  /* Small shadow — cards, dropdowns */
  --prestige-shadow-md:          ;  /* Medium shadow — floating elements */
  --prestige-shadow-lg:          ;  /* Large shadow — modals, popovers */

  /* --- Status Colors (within prestige context) --- */
  --prestige-success:            ;  /* อนุมัติ, ผ่าน */
  --prestige-success-foreground: ;
  --prestige-warning:            ;  /* คงค้าง, รอ */
  --prestige-warning-foreground: ;
  --prestige-danger:             ;  /* ต้องเร่ง, error */
  --prestige-danger-foreground:  ;

  /* --- Interactive States --- */
  --prestige-ring:               ;  /* Focus ring color */
  --prestige-input:              ;  /* Input border */
  --prestige-input-focus:        ;  /* Input border on focus */

  /* --- Data Visualization --- */
  --prestige-chart-1:            ;  /* Chart color 1 */
  --prestige-chart-2:            ;  /* Chart color 2 */
  --prestige-chart-3:            ;  /* Chart color 3 */
}
```

### Dark Mode Overrides

```css
.dark {
  /* Designer: ปรับ lightness สำหรับ dark mode */
  --prestige-primary:            ;  /* +5-10% lightness จาก light */
  --prestige-bg:                 ;  /* Dark background */
  --prestige-card:               ;  /* Dark card */
  --prestige-text:               ;  /* Light text for dark bg */
  --prestige-border:             ;  /* Subtle border for dark */
  /* ... ทุก token ต้องมี dark variant ... */
}
```

---

## Mapping: Current → New Variables

| Current Variable | New Variable | Notes |
|-----------------|-------------|-------|
| `--prestige` | `--prestige-primary` | Rename for clarity |
| `--prestige-80` | ลบ — ใช้ opacity แทน | `bg-prestige-primary/80` |
| `--prestige-60` | ลบ — ใช้ opacity แทน | `bg-prestige-primary/60` |
| `--prestige-40` | `--prestige-bg-subtle` | Designer กำหนดค่าใหม่ |
| `--prestige-20` | `--prestige-bg-muted` | Designer กำหนดค่าใหม่ |
| `--prestige-charcoal` | `--prestige-text` | Primary text |
| `--prestige-charcoal-80` | `--prestige-text-secondary` | Secondary text |
| `--prestige-charcoal-60` | ลบ — ซ้ำกับ text-secondary | |
| `--prestige-dark-green` | `--prestige-accent` | Accent color |
| `--prestige-warm-grey` | `--prestige-bg` | Background |
| `--prestige-foreground` | `--prestige-primary-foreground` | Text on primary |
| `--gradient-prestige` | `--prestige-gradient-hero` | Rename |
| `--shadow-prestige` | `--prestige-shadow-md` | Rename |
| (ไม่มี) | `--prestige-secondary` | **ใหม่** — Designer กำหนด |
| (ไม่มี) | `--prestige-border` | **ใหม่** — Designer กำหนด |
| (ไม่มี) | `--prestige-card` | **ใหม่** — Designer กำหนด |
| Hardcoded `#7A6A4F` | ลบ — ใช้ `--prestige-primary` | **12 instances ต้องแก้** |

---

## Tailwind Config Update

```typescript
// tailwind.config.ts — prestige section
prestige: {
  // Core
  primary:              "hsl(var(--prestige-primary))",
  "primary-hover":      "hsl(var(--prestige-primary-hover))",
  "primary-foreground": "hsl(var(--prestige-primary-foreground))",
  secondary:            "hsl(var(--prestige-secondary))",
  "secondary-hover":    "hsl(var(--prestige-secondary-hover))",
  "secondary-foreground":"hsl(var(--prestige-secondary-foreground))",
  accent:               "hsl(var(--prestige-accent))",
  "accent-foreground":  "hsl(var(--prestige-accent-foreground))",
  // Backgrounds
  bg:                   "hsl(var(--prestige-bg))",
  "bg-subtle":          "hsl(var(--prestige-bg-subtle))",
  "bg-muted":           "hsl(var(--prestige-bg-muted))",
  card:                 "hsl(var(--prestige-card))",
  "card-foreground":    "hsl(var(--prestige-card-foreground))",
  // Text
  text:                 "hsl(var(--prestige-text))",
  "text-secondary":     "hsl(var(--prestige-text-secondary))",
  "text-accent":        "hsl(var(--prestige-text-accent))",
  // Borders
  border:               "hsl(var(--prestige-border))",
  "border-strong":      "hsl(var(--prestige-border-strong))",
  "border-subtle":      "hsl(var(--prestige-border-subtle))",
  // Status
  success:              "hsl(var(--prestige-success))",
  warning:              "hsl(var(--prestige-warning))",
  danger:               "hsl(var(--prestige-danger))",
},
```

---

## Usage Patterns for FE

### Card Component

```tsx
// ❌ Before — hardcoded + inconsistent
<div className={isPrestige ? "border-2 border-prestige shadow-lg shadow-prestige/20" : "border-2 border-primary"}>
  <div className={isPrestige ? "bg-prestige" : "bg-primary"}>

// ✅ After — theme-based
<div className={isPrestige
  ? "border-2 border-prestige-border-strong shadow-prestige-shadow-md"
  : "border-2 border-primary"
}>
  <div className={isPrestige ? "bg-prestige-primary" : "bg-primary"}>
```

### Badge/Pill

```tsx
// ❌ Before
<span className="bg-prestige text-white">

// ✅ After
<span className="bg-prestige-primary text-prestige-primary-foreground">
```

### Background Tints

```tsx
// ❌ Before — magic opacity numbers
<div className="bg-prestige/10">
<div className="bg-prestige/5">

// ✅ After — semantic tokens
<div className="bg-prestige-bg-subtle">    /* hover, selected */
<div className="bg-prestige-bg-muted">     /* faded, disabled */
```

### Export/PDF (inline styles)

```tsx
// ❌ Before — hardcoded hex
primary: isPrestige ? '#7a6a4f' : '#c8102e'

// ✅ After — read from CSS at render time
const prestigeColor = getComputedStyle(document.documentElement)
  .getPropertyValue('--prestige-primary').trim();
primary: isPrestige ? `hsl(${prestigeColor})` : '#c8102e'
```

### Gradient

```tsx
// ❌ Before — hardcoded gradient
style={{ background: 'linear-gradient(135deg, #A3915D, #8B7D4E)' }}

// ✅ After — CSS variable
style={{ background: 'var(--prestige-gradient-hero)' }}
// or Tailwind:
className="bg-[image:var(--prestige-gradient-hero)]"
```

---

## Gradient Direction Rule 🔴 (กฎเหล็กจากแบงค์)

> **Gradient ต้องเป็น อ่อนไปเข้ม เสมอ — ห้ามเข้มไปอ่อน!**

ใช้กับทั้ง app — ไม่ใช่แค่ Prestige แต่ทุก gradient ทุก theme

### Rule

```
✅ ถูก: light color → dark color  (อ่อนไปเข้ม)
❌ ผิด: dark color → light color  (เข้มไปอ่อน)
```

### ตัวอย่าง

```css
/* ✅ ถูก — อ่อนไปเข้ม */
background: linear-gradient(135deg, hsl(44 28% 57%), hsl(44 28% 45%));
background: linear-gradient(to bottom, #D41145, #980C32);
background: linear-gradient(135deg, var(--prestige-primary-light), var(--prestige-primary));

/* ❌ ผิด — เข้มไปอ่อน */
background: linear-gradient(135deg, hsl(44 28% 45%), hsl(44 28% 57%));
background: linear-gradient(to bottom, #980C32, #D41145);
```

### Gradient Token Naming

```css
/* Gradient vars ต้องกำหนด from (อ่อน) → to (เข้ม) */
--prestige-gradient-hero:  linear-gradient(135deg, [lighter], [darker]);
--prestige-gradient-card:  linear-gradient(135deg, [lighter], [darker]);
--gradient-primary:        linear-gradient(135deg, [lighter red], [darker red]);
--gradient-hero:           linear-gradient(135deg, [lighter red], [darker red]);
```

### Current Violations (ต้องตรวจ)

| File | Current Gradient | Direction | Status |
|------|-----------------|-----------|--------|
| `index.css:125` | `hsl(344 85% 45%) → hsl(344 85% 55%)` | เข้ม→อ่อน | ❌ กลับด้าน |
| `index.css:126` | `hsl(344 85% 45%) → hsl(344 85% 55%)` | เข้ม→อ่อน | ❌ กลับด้าน |
| `index.css:127` | `hsl(44 28% 50%) → hsl(44 24% 57%)` | เข้ม→อ่อน | ❌ กลับด้าน |
| `BusinessCardTemplatePrestige.tsx:32` | `#A3915D → #8B7D4E → #66655B` | อ่อน→เข้ม | ✅ ถูก |
| `CompareResultsSummary.tsx:289` | `#D31145 → #980C32` | อ่อน→เข้ม | ✅ ถูก |

---

## Forbidden Patterns 🔴

| # | Pattern | Why | Fix |
|---|---------|-----|-----|
| F1 | `'#7a6a4f'` hardcoded | ไม่ sync กับ CSS var | ใช้ `var(--prestige-primary)` |
| F2 | `'#A3915D'` hardcoded | เหมือนกัน — hardcoded hex | ใช้ CSS var |
| F3 | `bg-prestige/10` magic opacity | ไม่ semantic | ใช้ `bg-prestige-bg-subtle` |
| F4 | Red gradient ปนกับ prestige | ปนคนละ theme | แยก theme ชัดเจน |
| F5 | Prestige color บน non-prestige product | ใช้ผิด context | เฉพาะ tier "Prestige" |
| F6 | Export hex ≠ UI hex | ลูกค้าเห็นสีต่างกัน | อ่านจาก CSS var เดียวกัน |
| **F7** | **Gradient เข้มไปอ่อน** | **กฎเหล็กแบงค์** | **กลับด้าน: อ่อนไปเข้มเสมอ** |

---

## Implementation Phases

| Phase | Work | Owner | Blocked By |
|-------|------|-------|-----------|
| **1. Designer fills values** | ใส่ HSL ทุก token ใน template ด้านบน | Designer | — |
| **2. แบงค์ approve** | Review ว่าสีถูกต้อง สวย premium | แบงค์ | Phase 1 |
| **3. Update CSS vars** | แก้ index.css: rename + เพิ่ม vars ใหม่ | FE | Phase 2 |
| **4. Update Tailwind config** | เพิ่ม prestige sub-tokens | FE | Phase 3 |
| **5. Migrate components** | แก้ 404 references: old var → new var | FE | Phase 4 |
| **6. Kill hardcoded hex** | ลบ `#7A6A4F` ทั้ง 12 instances | FE | Phase 4 |
| **7. QA visual regression** | Screenshot compare ก่อน/หลัง | QA | Phase 6 |

---

## Designer Deliverable Template

Designer ส่งกลับ file นี้ พร้อมค่า HSL ครบทุกช่อง:

```yaml
# Prestige Theme — Designer Output
# ทุกค่าเป็น HSL: "H S% L%"

core:
  primary:              ""  # สีหลัก (gold/olive)
  primary-hover:        ""  # Hover
  primary-foreground:   ""  # Text on primary
  secondary:            ""  # สีรอง
  secondary-hover:      ""
  secondary-foreground: ""
  accent:               ""  # Accent/links

backgrounds:
  bg:                   ""  # Page background
  bg-subtle:            ""  # Tint/hover (แทน opacity 10-20%)
  bg-muted:             ""  # Disabled/faded
  card:                 ""  # Card background
  card-foreground:      ""  # Card text

text:
  primary:              ""  # Main text
  secondary:            ""  # Muted text
  accent:               ""  # Highlighted values

borders:
  default:              ""  # Normal border
  strong:               ""  # Active/focus border
  subtle:               ""  # Dividers

gradients:
  hero:                 ""  # "linear-gradient(135deg, hsl(X), hsl(Y))"
  card:                 ""
  badge:                ""  # or "none" for solid

shadows:
  sm:                   ""  # "0 2px 8px -2px hsl(X / 0.1)"
  md:                   ""  # "0 4px 16px -4px hsl(X / 0.2)"
  lg:                   ""  # "0 8px 24px -4px hsl(X / 0.3)"

status:
  success:              ""  # อนุมัติ
  success-foreground:   ""
  warning:              ""  # คงค้าง
  warning-foreground:   ""
  danger:               ""  # ต้องเร่ง
  danger-foreground:    ""

interactive:
  ring:                 ""  # Focus ring
  input:                ""  # Input border
  input-focus:          ""  # Input focused

charts:
  color-1:              ""
  color-2:              ""
  color-3:              ""
```

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| v1.0 | 2026-03-19 | Initial — color audit + 7 rules |
| v2.0 | 2026-03-20 | **Full theme system** — 40+ tokens, mapping table, Designer template |

---

*"A theme is a promise of consistency. Break one token, break the promise." — DocCon*
