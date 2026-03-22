# iAgencyAIA FA Tools — Code Conduct v1.0

> "Quality is not an act, it is a habit." — Aristotle

**Effective**: 2026-03-19
**Author**: DocCon-Oracle (Quality Conductor)
**Repo**: `~/repos/github.com/BankCurfew/iagencyaiafatools/`
**Scope**: React 18 + TypeScript 5.8 + Vite 5 + Supabase + Deno Edge Functions
**Codebase**: ~40,000+ lines | 17 pages | 21+ components | 31 lib utils | 17 hooks | 14+ edge functions

---

## Baseline Audit Summary (19 มี.ค. 2026)

| Category | Score | Issues |
|----------|-------|--------|
| Code Standards | 🟡 6/10 | ESLint configured แต่ไม่มี pre-commit hook |
| TypeScript | 🔴 3/10 | `strict: false`, 20+ `any` usages |
| Components | 🟡 7/10 | Lazy loading ดี แต่ CompareMode.tsx 74KB ใหญ่เกิน |
| Styling | 🟢 8/10 | Tailwind + shadcn/ui consistent |
| API & Data | 🟡 6/10 | Supabase integration ดี แต่ rate limit แค่ 1 endpoint |
| Security | 🟡 5/10 | Encryption ดี แต่ `.env` ไม่อยู่ใน .gitignore |
| Testing | 🔴 0/10 | **Zero tests** — ไม่มีแม้แต่ 1 file |
| Business Logic | 🟢 8/10 | Validation แข็ง, caching 3-layer |
| Performance | 🟢 8/10 | Code splitting, manual chunks, PWA |
| Git | 🔴 3/10 | "Changes" commits, no hooks, .env exposed |

**Overall: 5.4/10 — ต้องปรับปรุงหลายจุด**

---

## 1. Code Standards

### บังคับ (MUST)

| Rule | Standard | สถานะปัจจุบัน |
|------|----------|--------------|
| **Linter** | ESLint ต้อง run ก่อนทุก commit | 🟡 Config มี แต่ไม่มี pre-commit hook |
| **Formatter** | Prettier หรือ ESLint format — consistent ทั้ง repo | 🟡 ไม่มี Prettier config |
| **Pre-commit hook** | Husky + lint-staged ต้อง block code ที่ไม่ผ่าน lint | 🔴 ไม่มี |
| **No console.log** | Production code ห้าม `console.log` — ใช้ structured logger | 🔴 มีทั่ว codebase |
| **File size limit** | Component ≤ 500 lines — ถ้าเกิน ต้อง split | 🔴 CompareMode.tsx 74KB, SharedProposal.tsx 1650 lines |
| **Import order** | React → External → Internal → Types → Styles | 🟡 ไม่ enforce |
| **Dead code** | ห้าม commented-out code — ลบทิ้ง ใช้ git history | 🟡 ไม่ตรวจ |

### Violations ที่ต้องแก้

```
🔴 V-CS1: ไม่มี pre-commit hook → ติดตั้ง husky + lint-staged
🔴 V-CS2: CompareMode.tsx 74KB → split เป็น sub-components
🔴 V-CS3: SharedProposal.tsx 1650 lines → split เป็น sections
🔴 V-CS4: console.log ทั่ว codebase → แทนด้วย structured logger
🟡 V-CS5: ไม่มี Prettier config → เพิ่ม .prettierrc
```

---

## 2. TypeScript Conduct

### บังคับ (MUST)

| Rule | Standard | สถานะปัจจุบัน |
|------|----------|--------------|
| **strict mode** | `strict: true` ใน tsconfig | 🔴 `strict: false` |
| **noImplicitAny** | ห้าม implicit `any` | 🔴 `false` |
| **strictNullChecks** | บังคับ null/undefined checks | 🔴 `false` |
| **noUnusedLocals** | ห้ามมี unused variables | 🔴 `false` |
| **noUnusedParameters** | ห้ามมี unused params | 🔴 `false` |
| **Explicit types** | ทุก function return type ต้อง explicit | 🟡 ส่วนใหญ่ inferred |
| **No `any`** | ห้ามใช้ `any` — ใช้ `unknown` + type guard แทน | 🔴 20+ instances |
| **Interface > Type** | ใช้ `interface` สำหรับ object shapes | 🟡 ผสม |

### `any` ที่ต้องแก้ทันที

| File | Location | Fix |
|------|----------|-----|
| `pdf.worker.ts` | planData, benefits, categorySelections | สร้าง `PlanData`, `Benefit` interfaces |
| `pdf-generator.ts` | PDF data handling | สร้าง `PdfExportConfig` interface |
| `ApplicationForm.tsx` | spouse_info, children_info, beneficiary_info, rejection_records, hospitalization_records, health_checkup_details, diagnosed_conditions, current_symptoms, recent_symptoms, additional_conditions, female_symptoms, payer_info (lines 71-136) | สร้าง typed interfaces ทุกตัว |

### Migration Plan (ค่อยๆ เปิด)

```
Phase 1: strictNullChecks: true (แก้ null issues)
Phase 2: noImplicitAny: true (แก้ any ทีละ file)
Phase 3: noUnusedLocals + noUnusedParameters: true
Phase 4: strict: true (full strict mode)
```

### Violations ที่ต้องแก้

```
🔴 V-TS1: strict: false → เปิด strict mode ตาม migration plan
🔴 V-TS2: 20+ any usages → แทนด้วย proper interfaces
🔴 V-TS3: ApplicationForm.tsx 12+ any fields → สร้าง FormData interface
🟡 V-TS4: noUnusedLocals: false → เปิดเพื่อลด dead code
```

---

## 3. Component Conduct

### บังคับ (MUST)

| Rule | Standard | สถานะปัจจุบัน |
|------|----------|--------------|
| **Max size** | ≤ 500 lines per component file | 🔴 หลายไฟล์เกิน |
| **Single responsibility** | 1 component = 1 job | 🔴 CompareMode.tsx ทำทุกอย่าง |
| **Lazy loading** | ทุก page ต้อง `React.lazy()` | ✅ ทำแล้ว (App.tsx:14-30) |
| **Error boundary** | มี error boundary ครอบ app | ✅ ทำแล้ว (main.tsx:51-82) |
| **Props interface** | ทุก component ต้องมี Props type | 🟡 บางตัวไม่มี |
| **Hooks extraction** | Logic ≥ 10 lines → extract เป็น custom hook | 🟡 บางจุดยังอยู่ใน component |
| **Memoization** | ใช้ `useMemo`/`useCallback` เมื่อ expensive | 🟡 มีบ้าง |

### Oversized Components (ต้อง Split)

| Component | Size | ปัญหา | แนวทาง |
|-----------|------|--------|---------|
| `CompareMode.tsx` | 74KB | ทำทุกอย่าง: compare, display, calculate | Split → `CompareTable`, `CompareChart`, `CompareActions`, `useCompareLogic` |
| `SharedProposal.tsx` | 1,650 lines | Page + logic + UI รวมกัน | Split → `ProposalHeader`, `ProposalBody`, `ProposalFooter`, `useProposal` |
| `ApplicationForm.tsx` | 719 lines | 6-step form ใน 1 file | มี Step 1-6 แล้ว — extract validation + submission logic เป็น hooks |
| `FinancialOverviewInfographic.tsx` | 50KB | Heavy visualization | Split → `OverviewCharts`, `OverviewSummary`, `OverviewActions` |

### Component Structure Standard

```typescript
// 1. Imports (React → External → Internal → Types)
import { useState, useMemo } from 'react';
import { Card } from '@/components/ui/card';
import { useAuth } from '@/hooks/useAuth';
import type { ProductData } from '@/types';

// 2. Props interface
interface ProductCardProps {
  product: ProductData;
  onSelect: (id: string) => void;
  isCompact?: boolean;
}

// 3. Component (export default ห้ามใช้ — ใช้ named export)
export function ProductCard({ product, onSelect, isCompact = false }: ProductCardProps) {
  // 4. Hooks first
  // 5. Derived state (useMemo)
  // 6. Handlers (useCallback)
  // 7. Early returns (loading, error)
  // 8. JSX return
}
```

### Violations ที่ต้องแก้

```
🔴 V-CP1: CompareMode.tsx 74KB → split เป็น 4+ sub-components
🔴 V-CP2: SharedProposal.tsx 1,650 lines → split เป็น sections
🟡 V-CP3: FinancialOverviewInfographic.tsx 50KB → split
🟡 V-CP4: บาง components ไม่มี Props interface → เพิ่ม
```

---

## 4. Styling Conduct

### บังคับ (MUST)

| Rule | Standard | สถานะปัจจุบัน |
|------|----------|--------------|
| **Framework** | Tailwind CSS — ห้ามใช้ inline CSS ยกเว้น dynamic values | ✅ ใช้ Tailwind ถูกต้อง |
| **UI Library** | shadcn/ui (Radix UI) เป็น base — ห้ามสร้าง custom ที่ซ้ำกับ shadcn | ✅ ใช้ถูกต้อง |
| **CSS Variables** | ใช้ CSS variables สำหรับ theme colors | ✅ มี |
| **Responsive** | Mobile-first — ทุก page ต้อง responsive | 🟡 ส่วนใหญ่ทำแล้ว |
| **Dark mode** | ถ้ามี dark mode ต้อง consistent ทุก page | 🟡 มีบางจุด |
| **Font** | `'Segoe UI', Arial, sans-serif` — AIA brand font | ✅ ตาม config |
| **Max width** | Content max-width 640px สำหรับ readability | ✅ |
| **Colors** | ใช้ CSS variables / Tailwind theme — ห้าม hardcode hex | 🟡 บาง hex ตรง |

### AIA Brand Colors (ต้อง consistent)

| Variable | Hex | Usage |
|----------|-----|-------|
| `--aia-red` | `#C8102E` | Primary, headers, CTA |
| `--aia-orange` | `#E87722` | Warning, pending states |
| `--success` | `#16a34a` | Approved, positive |
| `--danger` | `#dc2626` | Error, urgent |

### Violations ที่ต้องแก้

```
🟡 V-ST1: บาง hardcoded hex ใน components → ย้ายเข้า Tailwind theme
🟡 V-ST2: Dark mode ไม่ครบทุก page → audit + fix
```

---

## 5. API & Data Conduct

### บังคับ (MUST)

| Rule | Standard | สถานะปัจจุบัน |
|------|----------|--------------|
| **Supabase client** | ใช้ `@supabase/supabase-js` ผ่าน singleton | ✅ `src/integrations/supabase/client.ts` |
| **Types** | ใช้ auto-generated types จาก Supabase CLI | ✅ `types.ts` auto-generated |
| **React Query** | ทุก data fetch ใช้ React Query hooks | ✅ `useCachedData.ts` |
| **Error handling** | ทุก query ต้อง handle error state | 🟡 บางจุดขาด |
| **Rate limiting** | ทุก public endpoint ต้องมี rate limit | 🔴 มีแค่ `submit-lead` |
| **CORS** | ทุก edge function ต้อง set CORS headers | ✅ ทำแล้ว |
| **Stale time** | ตาม data type — static 30min, user 5min, dynamic 1min | ✅ configured ดี |
| **Optimistic updates** | ใช้เมื่อ UX ต้องการ instant feedback | 🟡 ยังไม่มี |

### Caching Strategy (3-layer — มาตรฐานดีแล้ว)

| Layer | Technology | TTL | Usage |
|-------|-----------|-----|-------|
| 1 | Service Worker (Vite PWA) | Build-based | Static assets |
| 2 | IndexedDB (`idb`) | Products 4hr, Fonts 30d | Persistent cache |
| 3 | sessionStorage | Per-session with TTL | Transient data |
| 4 | React Query (in-memory) | 1min-30min | API responses |

### Edge Functions ที่ต้อง Audit

| Function | Rate Limit | Auth | Status |
|----------|-----------|------|--------|
| `submit-lead` | ✅ 10/min/IP | ✅ | OK |
| `encrypt-decrypt` | ❌ | ✅ | 🔴 เพิ่ม rate limit |
| `insurance-chat` | ❌ | ✅ | 🔴 เพิ่ม rate limit (AI cost!) |
| `fetch-aia-funds` | ❌ | ✅ | 🟡 เพิ่ม rate limit |
| `generate-reminders` | ❌ | Cron | OK (internal) |
| `generate-business-card` | ❌ | ✅ | 🟡 เพิ่ม rate limit |

### Violations ที่ต้องแก้

```
🔴 V-API1: 13/14 edge functions ไม่มี rate limit → เพิ่มทุก public endpoint
🔴 V-API2: insurance-chat ไม่มี rate limit → AI usage = $$$ ต้อง limit
🟡 V-API3: บาง queries ไม่ handle error → เพิ่ม error handling
🟡 V-API4: ไม่มี API documentation → เขียน API reference
```

---

## 6. Security Conduct

### บังคับ (MUST)

| Rule | Standard | สถานะปัจจุบัน |
|------|----------|--------------|
| **`.env` in .gitignore** | `.env`, `.env.*` ต้องอยู่ใน .gitignore | 🔴 **ไม่มี!** มีแค่ `*.local` |
| **No secrets in code** | ห้าม API key, password ใน source code | 🔴 `.env` checked in กับ Supabase keys |
| **Encryption** | Sensitive fields ต้อง encrypt (AES-GCM) | ✅ 40+ fields encrypted |
| **PDPA consent** | ต้องมี consent flow ก่อนเก็บ PII | ✅ ApplicationForm.tsx + CookieConsentBanner |
| **RLS policies** | ทุก Supabase table ต้องมี Row Level Security | 🟡 ต้อง audit |
| **Auth protection** | ทุก route ที่มี data ต้อง authenticated | ✅ ProtectedRoute component |
| **Input sanitization** | ทุก user input ต้อง validate + sanitize | ✅ Zod + validation-utils.ts |
| **CORS** | Whitelist origins เท่านั้น | 🟡 ต้องตรวจว่า wildcard ไหม |

### Encryption ที่ดีแล้ว (ชมเชย)

- AES-GCM 256-bit with random 12-byte IV per value
- 40+ sensitive fields encrypted: national ID, addresses, health data, bank details
- `enc:` prefix marker ป้องกัน double-encryption
- Graceful fallback: save unencrypted ถ้า encrypt fail (ดีกว่าเสียข้อมูล)
- Server-side key (`ENCRYPTION_KEY` env var) — ไม่เก็บ client-side

### Violations ที่ต้องแก้ (URGENT)

```
🔴🔴 V-SEC1: .env ไม่อยู่ใน .gitignore → เพิ่ม .env, .env.*, !.env.example ทันที
🔴🔴 V-SEC2: .env checked in กับ Supabase keys → rotate keys + เพิ่ม .gitignore
🔴 V-SEC3: rate limit มีแค่ submit-lead → เพิ่มทุก public endpoint
🟡 V-SEC4: RLS policies ต้อง audit → ตรวจทุก table
🟡 V-SEC5: CORS config ต้อง audit → ตรวจว่าไม่ใช่ wildcard *
```

---

## 7. Testing Conduct

### สถานะปัจจุบัน: 🔴 ZERO TESTS

```
- ไม่มี .test.ts หรือ .spec.ts แม้แต่ 1 ไฟล์
- ไม่มี test runner (ไม่มี Vitest, Jest, Playwright)
- ไม่มี test script ใน package.json
- ไม่มี __tests__ directory
```

**นี่คือ risk ที่ใหญ่ที่สุดของ codebase — ทุก deploy คือ "hope it works"**

### บังคับ (MUST)

| Rule | Standard | สถานะปัจจุบัน |
|------|----------|--------------|
| **Test runner** | Vitest (เข้ากับ Vite ecosystem) | 🔴 ไม่มี |
| **Unit tests** | ทุก utility function ใน `lib/` ต้องมี test | 🔴 ไม่มี |
| **Integration tests** | ทุก hook ใน `hooks/` ต้องมี test | 🔴 ไม่มี |
| **E2E tests** | Critical flows: login, form submit, proposal generate | 🔴 ไม่มี |
| **Coverage** | Target: 60% overall, 90% สำหรับ business logic | 🔴 0% |
| **CI test** | ต้อง run tests ใน CI ก่อน merge | 🔴 ไม่มี CI |

### Priority Test Targets (เรียงตาม risk)

| Priority | File | เหตุผล | Test Type |
|----------|------|--------|-----------|
| 🔴 P0 | `validation-utils.ts` | Thai ID checksum, phone, email — ผิด = user ใช้ไม่ได้ | Unit |
| 🔴 P0 | `encryption-utils.ts` | Encrypt/decrypt sensitive data — ผิด = data leak/loss | Unit |
| 🔴 P0 | `tax-calculator.ts` | คำนวณภาษี — ผิด = legal issue | Unit |
| 🟡 P1 | `product-utils.ts` | Product matching — ผิด = ขาย product ผิดตัว | Unit |
| 🟡 P1 | `benefit-merge-utils.ts` | Combine benefits — ผิด = แสดงข้อมูลผิด | Unit |
| 🟡 P1 | `useAuth.ts` | Auth flow — ผิด = security breach | Integration |
| 🟡 P1 | `useCachedData.ts` | Data fetching — ผิด = stale/wrong data | Integration |
| 🟠 P2 | Application Form | 150+ fields, 6 steps — ผิด = application rejected | E2E |
| 🟠 P2 | Proposal Generation | Core feature — ผิด = wrong proposal to client | E2E |

### Setup ที่ต้องทำ

```bash
# 1. Install Vitest
npm install -D vitest @testing-library/react @testing-library/jest-dom jsdom

# 2. Add vitest.config.ts
# 3. Add test script to package.json: "test": "vitest"
# 4. Create first test: validation-utils.test.ts
# 5. Add CI pipeline to run tests on PR
```

### Violations ที่ต้องแก้

```
🔴🔴 V-TEST1: Zero test files → ติดตั้ง Vitest + เขียน P0 tests
🔴🔴 V-TEST2: No CI → setup GitHub Actions: lint + type-check + test
🔴 V-TEST3: validation-utils.ts ไม่มี test → เขียนทันที (Thai ID, phone, email)
🔴 V-TEST4: encryption-utils.ts ไม่มี test → เขียนทันที (encrypt/decrypt roundtrip)
🔴 V-TEST5: tax-calculator.ts ไม่มี test → เขียนทันที (legal risk)
```

---

## 8. Business Logic Conduct

### บังคับ (MUST)

| Rule | Standard | สถานะปัจจุบัน |
|------|----------|--------------|
| **Validation** | ทุก user input ต้องผ่าน validation | ✅ Zod + validation-utils |
| **Thai ID checksum** | Modulo 11 algorithm ต้องถูกต้อง | ✅ validation-utils.ts:6-33 |
| **Beneficiary %** | ต้องรวมเป็น 100% | ✅ มี validation |
| **Product matching** | Algorithm ต้อง verified กับ AIA official data | 🟡 ต้อง audit |
| **Tax calculation** | ต้องตรงกับกฎหมายภาษีปัจจุบัน (ปีภาษี 2569) | 🟡 ต้อง verify |
| **Vitality products** | ต้อง sync กับ AIA product list | 🟡 มี hardcoded fallback (product-utils.ts:46-54) |
| **Encryption fields** | Sensitive field list ต้อง sync ระหว่าง client + server | ✅ synchronized |

### สิ่งที่ดีแล้ว (ชมเชย)

| Feature | Implementation | Quality |
|---------|---------------|---------|
| Thai ID validation | Modulo 11 checksum | ✅ ถูกต้อง |
| Birthdate validation | 0-120 years, not future | ✅ ครอบคลุม |
| Phone validation | 0xx-xxx-xxxx, 10 digits | ✅ Thai standard |
| Bank account | 10-15 digits | ✅ |
| Email validation | Basic + typo detection | ✅ |
| 3-layer caching | SW + IndexedDB + sessionStorage + React Query | ✅ ออกแบบดี |
| Encryption | AES-GCM 256-bit, 40+ fields | ✅ มาตรฐานสูง |
| PDF export | Web Worker non-blocking | ✅ UX ดี |

### Risks ที่ต้อง Monitor

| Risk | สถานการณ์ | Mitigation |
|------|-----------|------------|
| Hardcoded Vitality list | product-utils.ts:46-54 มี fallback list | ต้อง sync กับ DB เป็น primary |
| Tax rules เปลี่ยน | กฎหมายภาษีเปลี่ยนทุกปี | ต้อง review ปีละ 1 ครั้ง |
| Product prices เปลี่ยน | AIA update premium tables | ต้องต่อ API ไม่ hardcode |
| PDPA changes | กฎหมาย PDPA อาจ update | ต้อง review consent flow |

### Violations ที่ต้องแก้

```
🟡 V-BL1: Vitality products hardcoded fallback → ควรเป็น DB-first + fallback
🟡 V-BL2: Tax calculator ต้อง verify กับปีภาษีปัจจุบัน
🟡 V-BL3: Product matching algorithm ต้อง audit accuracy
```

---

## 9. Performance Conduct

### บังคับ (MUST)

| Rule | Standard | สถานะปัจจุบัน |
|------|----------|--------------|
| **Code splitting** | ทุก page lazy-loaded | ✅ React.lazy (App.tsx:14-30) |
| **Bundle chunks** | Vendor libs แยก chunk | ✅ Manual chunks (vite.config.ts:64-97) |
| **Image optimization** | ใช้ WebP/AVIF, lazy load | 🟡 ต้อง audit |
| **Memoization** | Expensive calculations ใช้ `useMemo` | 🟡 มีบ้าง |
| **Web Workers** | Heavy tasks offload to worker | ✅ PDF generation |
| **PWA** | Offline-capable with auto-update | ✅ vite-plugin-pwa |
| **Prefetching** | Critical pages prefetch on load | ✅ Dashboard, Auth, Profile |
| **Bundle size** | Monitor total bundle — target < 500KB initial | 🟡 ต้องวัด |

### สิ่งที่ดีแล้ว

```
✅ React.lazy() ทุก 17 pages
✅ Manual chunks: react, supabase, ui, pdf, charts, excel แยกหมด
✅ Prefetch: Dashboard + Auth + Profile on app load
✅ PWA with auto-update detection (main.tsx:22-48)
✅ Web Worker for PDF generation
✅ IndexedDB caching with intelligent TTL
✅ Version-based cache invalidation
```

### Violations ที่ต้องแก้

```
🟡 V-PERF1: CompareMode.tsx 74KB — ใหญ่เกินควร split + lazy load sub-sections
🟡 V-PERF2: FinancialOverviewInfographic.tsx 50KB — heavy visualization ควร split
🟡 V-PERF3: ไม่มี bundle size monitoring → เพิ่ม rollup-plugin-visualizer
🟡 V-PERF4: Image optimization ต้อง audit
```

---

## 10. Git Conduct

### บังคับ (MUST)

| Rule | Standard | สถานะปัจจุบัน |
|------|----------|--------------|
| **Commit format** | `type: description` (feat/fix/refactor/docs/test/chore) | 🔴 ส่วนใหญ่ไม่ทำ |
| **No vague commits** | ห้าม "Changes", "Update", "Fix" โดดๆ | 🔴 มี "Changes" หลายตัว |
| **Branch naming** | `feature/xxx`, `fix/xxx`, `refactor/xxx` | 🟡 ไม่ enforce |
| **No secrets** | ห้าม commit .env, API keys, credentials | 🔴🔴 .env committed! |
| **No force push** | ห้าม `--force` บน main/production | 🟡 ไม่มี protection |
| **PR required** | ทุก merge ต้องผ่าน PR + review | 🟡 ไม่ enforce |
| **Pre-commit hooks** | lint + type-check ก่อน commit | 🔴 ไม่มี |
| **.gitignore** | ครอบคลุม .env, node_modules, dist, OS files | 🔴 ขาด .env patterns |

### ตัวอย่าง Commits ที่ผิด → ที่ถูก

| ❌ ผิด | ✅ ถูก |
|--------|-------|
| `Changes` | `fix: correct copay calculation for Health Happy plan` |
| `Preceding changes` | `refactor: extract validation logic to shared utils` |
| `Add AdminApiKeys UI` | `feat: add AdminApiKeys management UI` |

### Violations ที่ต้องแก้ (URGENT)

```
🔴🔴 V-GIT1: .env committed กับ Supabase keys → rotate keys + add .gitignore + git rm --cached
🔴 V-GIT2: Commit messages ไม่มี type prefix → enforce conventional commits
🔴 V-GIT3: ไม่มี pre-commit hooks → ติดตั้ง husky
🔴 V-GIT4: .gitignore ขาด .env patterns → เพิ่มทันที
🟡 V-GIT5: ไม่มี branch protection → เปิด GitHub branch protection rules
🟡 V-GIT6: ไม่มี PR requirement → enforce PR reviews
```

---

## Action Plan — Priority Order

### 🔴🔴 CRITICAL (ทำวันนี้)

| # | Action | File/Location | Assignee |
|---|--------|--------------|----------|
| 1 | เพิ่ม `.env` ใน .gitignore + `git rm --cached .env` | `.gitignore` | BotDev/Dev |
| 2 | Rotate Supabase keys (exposed in git history) | Supabase dashboard | แบงค์/Admin |
| 3 | สร้าง `.env.example` (no real values) | root | BotDev/Dev |

### 🔴 HIGH (ภายใน 1 สัปดาห์)

| # | Action | Impact |
|---|--------|--------|
| 4 | ติดตั้ง Vitest + เขียน P0 tests (validation, encryption, tax) | ลด regression risk |
| 5 | ติดตั้ง Husky + lint-staged (pre-commit hook) | ป้องกัน bad code เข้า repo |
| 6 | เพิ่ม rate limit ทุก edge function (โดยเฉพาะ insurance-chat) | ป้องกัน cost spike |
| 7 | เปิด `strictNullChecks: true` ใน tsconfig | จับ null bugs |

### 🟡 MEDIUM (ภายใน 2 สัปดาห์)

| # | Action | Impact |
|---|--------|--------|
| 8 | Split CompareMode.tsx (74KB → 4+ files) | Maintainability |
| 9 | Split SharedProposal.tsx (1,650 lines → sections) | Maintainability |
| 10 | แก้ 20+ `any` types → proper interfaces | Type safety |
| 11 | ลบ `console.log` → structured logger | Production quality |
| 12 | Setup GitHub Actions CI (lint + type-check + test) | Automation |

### 🟢 NICE-TO-HAVE (ภายใน 1 เดือน)

| # | Action | Impact |
|---|--------|--------|
| 13 | Full `strict: true` migration | Full type safety |
| 14 | E2E tests (Playwright) สำหรับ critical flows | Confidence |
| 15 | Bundle size monitoring | Performance tracking |
| 16 | API documentation | Developer experience |

---

## Coordination Map

| Oracle | Role | Deliverable |
|--------|------|-------------|
| **DocCon** | เขียน + enforce Code Conduct | This document |
| **BotDev** | Primary implementer — fix violations | Code changes |
| **Dev** | Architecture support — split components, CI setup | Infrastructure |
| **QA** | Test strategy + write first tests | Test files |
| **Security** | Audit RLS, CORS, .env exposure | Security report |
| **Admin** | Rotate Supabase keys, branch protection | Admin actions |

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| v1.0 | 2026-03-19 | Initial release — 10 sections, 40+ violations identified |

---

*"Code without tests is legacy code the moment it's written." — DocCon*
