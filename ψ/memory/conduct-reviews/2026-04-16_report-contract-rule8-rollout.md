---
type: conduct-standard
date: 2026-04-16
author: DocCon-Oracle
source: Researcher-Oracle/ψ/writing/2026-04-16_multi-agent-rtk-eval.md §A.6 M2 + §C.1 #1
approved-by: แบงค์ (2026-04-16)
status: active
---

# Report Contract — Golden Rule #8 (Conduct Reference)

## Why
Vague `/talk-to bob "cc: ..."` reports = silent failures.
- BoB dashboard cannot parse prose
- แบงค์ cannot scan the board
- DocCon audits cannot verify what was actually done
- Multi-Agent Orchestration Book Ch 14: *"Required four iterations — omitting any element caused silent failures"*

## The 4 Required Fields

1. **what** — verb + object
2. **why** OR **source** — reason or ref (eval §X.Y, issue#, etc.)
3. **next** — specific action with ETA OR `done`
4. **ref** — `file:line` when code/files involved

## Length Caps

- `cc: ...` reports → ≤200 chars, single line, separator ` · `
- `STATUS:` updates → ≤500 chars, multi-line structured

## DocCon Audit Procedure

When reviewing oracle threads (daily session check):

1. Pull recent `cc:` messages from each oracle's thread
2. For each message, verify presence of: what / why-or-source / next / (ref if code)
3. Flag violations:
   - Missing field → NOTICE on first occurrence
   - Repeat in same week → WARNING
   - 3rd in same week → escalate to BoB

## Audit Categories

| Category | Check |
|---|---|
| **Empty cc** | `cc: done` with no body → fail |
| **Prose drift** | Sentence form, no separators → fail |
| **No source** | Action without why/ref → fail |
| **Missing next** | Terminal but no `done` marker → fail |
| **Missing ref for code** | Code work without `file:line` → fail |

## Sources

- Global Rule: `~/.claude/CLAUDE.md` Rule #8
- Eval: `Researcher-Oracle/ψ/writing/2026-04-16_multi-agent-rtk-eval.md`
- Approval thread: BoB thread #6 (2026-04-16)

## DocCon Action Items

- [ ] Add CC compliance to daily audit checklist (Section 2 of CLAUDE.md)
- [ ] Build `awk`/`jq` filter on `~/.oracle/maw-log.jsonl` to flag non-compliant cc messages
- [ ] Weekly compliance % report to BoB

---

*"Form follows function. Reports without contract are noise." — DocCon*
