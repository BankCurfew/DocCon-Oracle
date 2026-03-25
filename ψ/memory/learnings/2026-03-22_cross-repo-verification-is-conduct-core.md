---
title: Cross-repo verification is the core of conduct review — 3-layer check (engine → display → official doc) catches subtle inconsistencies
tags: [["conduct-review", "verification", "aia-terminology", "quality"]]
created: 2026-03-22
source: rrr: DocCon-Oracle session 8
project: github.com/bankcurfew/doccon-oracle
---

# Cross-repo Verification is the Core of Conduct Review

When reviewing AIA terminology (FA bonus names, conditions, colors), a single-source check is insufficient. The real value of DocCon review is **3-layer verification**:

1. **Engine/data layer** — what the code calculates and labels internally (e.g., "โบนัสคงอยู่")
2. **Display layer** — what the user actually sees (e.g., "โบนัสการคงอยู่" after shortBonusLabel transform)
3. **Official source** — what AIA's signed documents say (e.g., official PDF from อลิสา ลิมะโรจน์)

In session 8, this 3-layer approach caught:
- Engine label missing "การ" that display silently fixed — consistency issue
- CAB condition using raw numbers (FYP >= 1.9M) instead of the FA-familiar term "MDRT" — clarity issue
- Stroke width 1.5px vs brand guideline 3px — specification drift

**Lesson**: "Looks right" is not "is right." Evidence across layers is what makes DocCon reviews credible.

---
*Added via Oracle Learn*
