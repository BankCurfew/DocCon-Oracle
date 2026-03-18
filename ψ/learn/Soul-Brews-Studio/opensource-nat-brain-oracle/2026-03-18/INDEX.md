# Soul-Brews-Studio Oracle Architecture — Learning Module

**Extracted**: 2026-03-18 @ 13:00 UTC+7
**Source**: `/home/mbank/repos/github.com/Soul-Brews-Studio/opensource-nat-brain-oracle`
**Purpose**: Document Nat's Oracle system for DocCon-Oracle reference

---

## Files in This Module

### 1. **1253_CODE-SNIPPETS.md** (1,057 lines)
Comprehensive code patterns, templates, and direct excerpts from Soul-Brews source.

**Covers**:
- Oracle Constitution (CLAUDE.md Golden Rules)
- Core Philosophy (3 + 2 principles, Oracle Stack)
- Memory Structure (ψ/ architecture with git status)
- Memory: Resonance (WHO I am — identity, personality, working style)
- Memory: Learnings (distilled patterns in 3 tiers)
- Memory: Retrospectives (complete template structure)
- Workflows & Short Codes (ccc → nnn → gogogo → rrr)
- Subagent Architecture (12 available agents + delegation rules)
- Safety Rules (12 golden rules + multi-agent worktree safety)
- 16 Key Patterns Discovered (from delegation hierarchy to bash constraints)

### 2. **1253_ARCHITECTURE.md** (650+ lines)
System design patterns and infrastructure layout.

### 3. **1253_QUICK-REFERENCE.md** (500+ lines)
Quick lookup tables and command cheat sheets.

---

## How to Use This Learning Module

### For DocCon Implementation

**Principle 1: Nothing is Deleted**
- Implement append-only conduct logging (like ψ/memory/logs/)
- Archive old violations, don't erase them
- Timestamp everything

**Principle 2: Patterns Over Intentions**
- Track actual email review frequency (not stated goals)
- Monitor real commit quality (via git log)
- Measure loop compliance via evidence

**Principle 3: External Brain, Not Command**
- Mirror violations, suggest fixes
- Don't command oracles (that's BoB's role)
- Keep conduct review objective

### For Identity & Memory

**Resonance System** (WHO you are):
- Location: `ψ/memory/resonance/`
- Files: identity.md, personality.md, oracle.md, working-style.md
- Content: stable, infrequently updated, defines your nature

**Learnings System** (PATTERNS you found):
- Location: `ψ/memory/learnings/`
- Format: 3-tier system (Foundation, Architecture, Polish)
- Use: discovered patterns, anti-patterns, teaching moments

### For Workflows

**Core Cycle**: `ccc → nnn → gogogo → rrr`
- `ccc` (Create Context & Compact) - gather situation
- `nnn` (Next Task Planning) - plan work
- `gogogo` (Execute Plan) - do work
- `rrr` (Retrospective) - reflect & learn

**Search Commands**:
- `/context-finder` - find relevant history (Haiku does heavy lifting)
- `/trace [project]` - find lost projects (5 parallel agents)
- `/recap` - fresh start context

### For Multi-Agent Safety

**Critical Rule**: Never rewrite git history in multi-agent setup
- ❌ `git commit --amend` (breaks agent hash alignment)
- ❌ `git rebase -i` (orphans all synced agents)
- ✅ Always create NEW commits

**File Access**:
- Use `.tmp/` for temporary files (gitignored)
- Notify user before accessing files outside repo
- Never access `/tmp/` or `~/.cache/` without warning

---

## Key Takeaways for DocCon

### The "Oracle Keeps the Human Human" Principle

Nat's Oracle doesn't command—it mirrors. DocCon should do the same:
1. Observe conduct (collect data)
2. Mirror patterns back (show evidence)
3. Suggest improvements (external brain)
4. Respect agency (never command)

### The Frequency Reveals Priority Pattern

DocCon should track what oracles **actually do** (frequency) vs what they **say** (intention):
- Which loops miss most?
- Which violations repeat?
- What patterns emerge from behavior?

### The Append-Only Principle

Build conduct-reviews as immutable records:
- Never delete records
- Archive old violations with context
- Use timestamps as truth markers
- Enable reconstruction of any decision

---

## Cross-Reference

| DocCon Function | Soul Pattern | Reference |
|-----------------|--------------|-----------|
| Conduct Review | Oracle mirror | Principle 2 |
| Loop Monitoring | Pattern tracking | Frequency analysis |
| Violation logging | Append-only | Section: Nothing is Deleted |
| Communication | Transparency | Rule 6: Oracle never pretends |
| Delegation | Subagent pattern | Subagent Architecture |
| Context mgmt | ψ/ structure | Memory Structure section |

---

**Next Steps**:
1. Read **1253_CODE-SNIPPETS.md** for detailed patterns
2. Reference **1253_ARCHITECTURE.md** for system design
3. Use **1253_QUICK-REFERENCE.md** for day-to-day lookup
4. Implement principles progressively (don't force all at once)

---

*This learning module is part of DocCon-Oracle's foundation.*
*Date: 2026-03-18 | Analyst: Claude Code*
