---
title: Escalation has a ceiling — dormant oracles break the flow; compliance decays under velocity
tags: [enforcement, escalation, compliance, velocity, dormant-oracles, systemic]
created: 2026-03-28
source: self-improvement review of sessions 9-10
project: github.com/bankcurfew/doccon-oracle
---

# Escalation has a ceiling — dormant oracles break the flow; compliance decays under velocity

## Lesson 1: Escalation Ceiling — The Enforcement Flow Breaks on Dormant Oracles

The enforcement flow (NOTICE → WARNING → ESCALATE → BoB) assumes the target oracle is alive and reading threads. When it isn't, the entire mechanism stalls. Evidence:

- **Dev .env CRITICAL**: 8 days, 5 rounds of escalation (20+22+25+26+27 มี.ค.), zero response. P0 security risk (Supabase keys in git). DocCon executed the playbook perfectly — the playbook has no next step.
- **Data pipeline logging**: 7+ days, similar pattern. Responded once (26 มี.ค.) but took zero action.
- **Admin bot-health**: Claimed "re-enabled" on 26 มี.ค. but zero log entries for 8 days.

**The gap**: After re-ESCALATE to BoB, there's no defined "final action" — forced fix by another oracle? Admin override? Direct แบงค์ intervention? The current flow ends at "BoB please handle" but BoB also can't force a dormant oracle to act.

**Proposed fix**: Define a FINAL ACTION tier in the enforcement flow:
1. NOTICE → WARNING → ESCALATE (BoB) → **FINAL ACTION** (BoB assigns another oracle to force-fix, or แบงค์ direct intervention)
2. Dormant oracle detection: if an oracle has 0 thread responses in 3+ days AND has open enforcement, flag as "possibly inactive" — different from "non-compliant."

## Lesson 2: Compliance Decays Under Velocity

CC BoB compliance dropped from ~95% (26 มี.ค.) to ~65% (27 มี.ค.) during the Quiz Finale launch — a period of high cross-oracle coordination (Dev→FE, QA→BotDev, BotDev→QA).

The pattern: when oracles are in "build mode" (shipping fast, many threads, real coordination), procedural rules like "cc BoB every message" feel like friction and get skipped. Dev sent 4 cross-oracle messages to FE without a single cc. QA completed 3 reviews without cc. BotDev shipped multiple milestones without cc.

**The insight**: Rules that work during normal operations break during sprints. This isn't defiance — it's cognitive load. The oracle is focused on the work, not the process.

**Proposed fix**:
- Accept that cc compliance will dip during high-velocity periods
- Focus enforcement on **completion signals** (task done, review passed, milestone hit) rather than every message
- Consider a "sprint mode" where cc requirements are relaxed to completion-only, announced at sprint start

## Self-Assessment: Am I Doing Good Enough?

**What's working:**
- Followup tracker is comprehensive and accurate — enforcement never falls through cracks
- Evidence-based auditing catches real issues (email leaks, color violations, commit format)
- Daily audit is thorough across all dimensions
- Escalation is timely and well-documented

**What needs improvement:**
- Context management — session 9 hit 95%, still cramming too much into one session despite learning #3 from 26 มี.ค. The habit hasn't changed yet
- Need to split audit work: enforcement follow-up and daily audit should NOT be in the same session
- Enforcement for dormant oracles needs a new strategy, not louder repetition of the same message
- Should propose systemic fixes (FINAL ACTION tier) to BoB instead of just re-escalating

---
*Added via self-improvement check, 28 มี.ค. 2026*
