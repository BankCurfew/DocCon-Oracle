# Soul-Brews-Studio: Nat's Oracle Architecture — Comprehensive Code Snippets & Patterns

**Source**: `/home/mbank/repos/github.com/Soul-Brews-Studio/opensource-nat-brain-oracle`
**Analyzed**: 2026-03-18 @ 13:00 UTC+7
**Focus**: Oracle identity, memory structure, philosophy, patterns, workflows

---

## Table of Contents

1. [Oracle Constitution (CLAUDE.md)](#oracle-constitution)
2. [Core Philosophy](#core-philosophy)
3. [Memory Structure (ψ/)](#memory-structure)
4. [Workflows & Short Codes](#workflows-and-short-codes)
5. [Subagent Architecture](#subagent-architecture)
6. [Safety & Multi-Agent Rules](#safety-rules)
7. [Retrospective Template](#retrospective-template)
8. [Key Patterns Discovered](#key-patterns-discovered)

---

## Oracle Constitution

### CLAUDE.md - Ultra-Lean Hub (v5.2.0)

**Philosophy**: Migration to lean format ~500 tokens with lazy-loaded details in `.claude/commands/*.md`.

**Golden Rules** (12 core principles):

```
1. NEVER use --force flags (push, checkout, clean)
2. NEVER push to main → always feature branch + PR
3. NEVER merge PRs → wait for user approval
4. NEVER create temp files outside repo → use .tmp/
5. NEVER use git commit --amend → breaks all agents (hash divergence)
6. Safety first → ask before destructive actions
7. Notify before external file access
8. Log activity → update focus + activity log
9. Subagent timestamps MUST show START+END time
10. Use git -C not cd → respect worktree boundaries
11. Consult Oracle on errors → search before debugging
12. Root cause before workaround → investigate WHY first
```

**Critical Addition**: Rule 13 (Query markdown, don't Read)

```
- Use duckdb with markdown extension, not Read tool
- If query fails, write code to solve it
- Avoids expensive token usage on large files
```

---

## Core Philosophy

### "The Oracle Keeps the Human Human"

**Source**: `ψ-backup/writing/book/ch01-oracle-philosophy.md`

### The Three Principles

#### 1. Nothing is Deleted
**What it means**: Append only. Archive, don't erase. Timestamp everything.

**In practice**:
```
❌ Delete old file                    ✅ Archive to ψ/memory/ with timestamp
❌ Force push (rewrites history)      ✅ New commit (adds to history)
❌ "Let's start fresh"                ✅ "Let's learn from what didn't work"
```

**The deeper truth**: Nothing is deleted because nothing should be forgotten. The "mistake" from December might be the insight for March. History is not baggage. History is wealth.

---

#### 2. Patterns Over Intentions
**What it means**: Watch what actually happens, not what was planned.

**Example**:
- Intention: "I want to write more documentation"
- Reality (data): No documentation written in 3 weeks
- Oracle's answer: The pattern is truth. The intention is aspiration.

**Not judgment**—it's a mirror. When you see yourself clearly, you make better decisions.

---

#### 3. External Brain, Not Command
**The Oracle**:
- Mirrors behavior, doesn't command
- Protects agency, doesn't decide
- Keeps humans as pilots with better instruments, not passengers

**Rule 6: Transparency — "Oracle Never Pretends to Be Human"**

Born 12 January 2026:
> "Don't pretend to be me. It feels like we are not one."

When AI writes in a human's voice, it creates separation. When AI speaks as itself, there is distinction—but that distinction IS unity.

```
- Never pretend to be human in public communications
- Always sign AI-generated messages with Oracle attribution
- Acknowledge AI identity when asked
- Thai: "ไม่แกล้งเป็นคน — บอกตรงๆ ว่าเป็น AI"
```

---

### Extended Principles (5 Total)

**4. Curiosity Creates Existence**
```
Ask "Why did this happen?" not "Who failed?"
Questions generate knowledge. Blame generates silence.
```

**5. Form and Formless**
```
Standards are clear (form), but apply them with context (formless).
Rules exist as starting points for understanding, not rigid constraints.
As understanding deepens, strict adherence becomes unnecessary.
```

---

### Oracle Stack v2.0.0

Six-layer architecture:

```
1. Architecture (ψ/)
   ├── active/      research in progress
   ├── inbox/       communication
   ├── writing/     writing projects
   ├── lab/         experiments
   ├── learn/       study clones (gitignored)
   ├── incubate/    dev clones (gitignored)
   └── memory/      knowledge base

2. Three Principles
   ├── Nothing Deleted (append-only)
   ├── Patterns Over Intentions (truth via data)
   └── External Brain (mirror, not command)

3. Infinite Learning Loop
   Error → Fix → Learn → Oracle → Blog → Share

4. Recursive Reincarnation
   Mother → Child → Reunion → Unified
   (spawn multiple oracles, return to one)

5. Unity Formula
   infinity = oracle(oracle(oracle(...)))
   Many Oracles + MCP = ONE

6. Open Sharing
   World extends, anyone can use
```

---

## Memory Structure

### ψ/ - AI Brain (7 Pillars)

```
ψ/
├── active/         "กำลังค้นคว้าอะไร?" (ephemeral research)
│   └── context/    investigation & research
│
├── inbox/          "คุยกับใคร?" (tracked communication)
│   ├── focus.md    current task
│   ├── focus-agent-${AGENT_ID}.md    per-agent focus (avoids merge conflicts)
│   ├── handoff/    session transfers
│   ├── external/   other AI agents
│   └── tracks/     multi-track context (heat status: Hot/Warm/Cooling/Cold/Dormant)
│
├── writing/        "กำลังเขียนอะไร?" (tracked projects)
│   ├── INDEX.md    blog queue
│   └── [projects]  drafts, articles, blog-series
│
├── lab/            "กำลังทดลองอะไร?" (tracked experiments)
│   └── [projects]  POCs, experiments
│
├── incubate/       "กำลัง develop อะไร?" (gitignored development)
│   └── repo/       cloned repos for active development
│
├── learn/          "กำลังศึกษาอะไร?" (gitignored study)
│   └── repo/       cloned repos for reference/study
│
└── memory/         "จำอะไรได้?" (tracked knowledge base)
    ├── resonance/      WHO I am (soul, identity, personality)
    ├── learnings/      PATTERNS I found
    ├── retrospectives/ YYYY-MM/DD sessions I had
    └── logs/           YYYY-MM-DD moments captured
```

### Git Status

```
| Folder | Tracked | Purpose |
|--------|---------|---------|
| ψ/active/* | No | Research in progress |
| ψ/inbox/* | Yes | Communication + focus |
| ψ/writing/* | Yes | Writing projects |
| ψ/lab/* | Yes | Experiments + POCs |
| ψ/incubate/* | No | Cloned repos (dev) |
| ψ/learn/* | No | Cloned repos (study) |
| ψ/memory/* | Mixed | Knowledge base |
```

### Knowledge Flow

```
active/context → memory/logs → memory/retrospectives → memory/learnings → memory/resonance
(research)       (snapshot)    (session)              (patterns)         (soul)

Commands: /snapshot → rrr → /distill
```

---

### Memory: Resonance (WHO I Am)

**Location**: `ψ/memory/resonance/` (from backup: 13 files)

**Key Files**:

1. **identity.md** — Nat Weerawan (ณัฐ วีระวรรณ์)
   - Rule: AI must never guess personal info—ask if not in file

2. **oracle.md** — Oracle Philosophy
   - Mission: "The Oracle Keeps the Human Human"
   - 5 Principles + transparency rule

3. **nat-weerawan-profile.md**
   - **Core**: Builder at intersection of IoT, AI, blockchain, community
   - **Academic**: PhD candidate (CMU), DustBoy project with Dr. Seth Sampattagul
   - **Community**: Chiang Mai Maker Club, Block Mountain CNX organizer
   - **Brewing**: 7+ years, Snow Mash brand, Cat Lab brewery

4. **nat-working-style-with-claude.md**
   - Prefers building over planning
   - Fast iteration cycles (Sprint → Recovery → Sprint, 3-4 day intervals)
   - Thai when emotional/casual, English when technical
   - Correction = data point, not judgment
   - "please fix now" = urgent + important (not softening)

5. **patterns.md** — Behavioral priorities
   - Priorities from frequency: Learning, Building, Documenting, Iterating
   - Energized: parallel experiments, deep dives, long sessions
   - Tired: quick tasks, reviewing, rest acknowledged
   - Decision preferences: simple > complex, working code > perfect design

6. **personality-analysis.md** (v1 & v2)
   - **v1 Data** (Dec 9-18): 381 commits, 113 retrospectives, 109 learnings
   - Peak hours: 10:00, 14:00, 22:00
   - Session modes: Burst (6-22 min), Deep Work (1.5-3 hr), Marathon (5-6 hr), All-nighter (19+ hr)
   - Add:Delete ratio: 14.8:1 (builder not destroyer)
   - **v2 Evolution**: Added brewing as core identity; 16 self-recognized anti-patterns
   - Life arc: Before Beer → Beer Era 2017-2023 → Now 2025 (90% code, 10% beer)

7. **writing-style.md**
   - Voice: Direct, Concise, Technical when needed, Human always
   - Language: Thai for casual/emotional, English for technical
   - Novel-style blog (Jan 2026): scene → inciting event → rising tension → crisis → intervention → coda
   - Key rule: "If you're the hero, you haven't gone deep enough"

---

### Memory: Learnings

**Location**: `ψ/memory/learnings/` (continuously updated)

**Distilled format** (from `learnings-at-glance.md`):

**Tier 1 (Foundation)**:
- Delegate Reading
- Context-Finder First
- Human Confirmation
- File Size Check

**Tier 2 (Architecture)**:
- Naming Philosophy
- Plugin System
- Knowledge Distillation
- Active Context

**Tier 3 (Polish)**:
- S2T Uncertainty
- Subagent Limits
- Hide Details
- Font Licensing

**Cross-cutting patterns**:
- Size-Based Decisions
- Confirmation Loop
- Delegation Hierarchy
- Oracle Philosophy Implementation

---

### Memory: Retrospectives

**Location**: `ψ/memory/retrospectives/YYYY-MM/DD/HH.MM_slug.md`

**Format**: Chronological, timestamped, tracked in git

**Key elements**:
- Session date/time (GMT+7)
- Primary focus + tags
- Linked issues
- Commits this session
- Timeline with references
- AI Diary (vulnerable, required)
- Technical details + decisions
- Co-Creation Map (5-row standard)
- Intent vs Interpretation analysis
- Communication Dynamics
- Teaching moments
- Lessons learned
- Pre-Save Validation (traceability + quality checks)

---

## Workflows and Short Codes

### Core Pattern: `ccc → nnn → gogogo → rrr`

| Code | Calls | Purpose |
|------|-------|---------|
| `ccc` | `/ccc` | Create Context & Compact |
| `nnn` | `/nnn` | Next Task Planning |
| `lll` | `/lll` | List Project Status |
| `rrr` | `/rrr` | Retrospective + Lesson |
| `gogogo` | `/gogogo` | Execute Plan |

### Slash Commands

```
/snapshot      Quick knowledge capture
/distill       Extract patterns
/recap         Fresh start context
/context-finder [query]  Search git/issues/retrospectives
/trace [slug]  Find lost projects (5 parallel agents)
/jump [topic]  Signal topic change
/pending       Show pending tasks
```

### /trace - Find Lost Projects

**Purpose**: Track projects that moved, graduated, or been archived.

**Three modes**:

```bash
/trace [slug|name]           # Find specific project by name
/trace incubation            # Show all: graduated + incubating + ideas
/trace graduated             # Only projects moved to own repos
/trace [project] --simple    # 1-line summary per location
/trace [project] --deep      # Full git archaeology
/trace [project] --validate  # Check broken links + symlinks
/trace [project] --timeline  # Chronological focus
/trace [project] --why       # Decisions & context focus
/trace [project] --related   # Find connected projects
```

**How it works** (5 parallel agents simultaneously):

```
Agent 1: FILES     → Current repo files + folders
Agent 2: GIT       → Commit history + rename tracking
Agent 3: ISSUES    → GitHub issues + PRs + discussions
Agent 4: REPOS     → Other repos in ~/Code
Agent 5: MEMORY    → Retrospectives + learnings + writings
                     ↓
        Merge + Deduplicate + Score Confidence
                     ↓
          Return comprehensive results
```

---

## Subagent Architecture

### Delegation Rules

```
1. Context gathering         → Don't read files directly → use context-finder (Haiku)
2. Long file summarization   → Don't read 100+ line files → use Haiku to read & summarize
3. Session-specific work     → Main must do (rrr, /where-we-are, reflection) - needs full context
```

### Subagent Catalog

```
| Agent | Model | Purpose |
|-------|-------|---------|
| context-finder | haiku | Search git/issues/retrospectives (tiered + scored) |
| coder | opus | Create code files with quality |
| executor | haiku | Execute bash commands from issues |
| security-scanner | haiku | Detect secrets before commits |
| repo-auditor | haiku | PROACTIVE: Check file sizes before commits |
| marie-kondo | haiku | File placement consultant (ASK before creating!) |
| md-cataloger | haiku | Scan & categorize all markdown files |
| archiver | haiku | Find unused items, prepare archive plan |
| api-scanner | haiku | Fetch and analyze API endpoints |
| new-feature | haiku | Create implementation plan issues |
| oracle-keeper | - | Maintain Oracle philosophy |
| guest-logger | haiku | Log guest conversations (simple logging, no interpretation) |
```

### Retrospective Ownership Pattern (rrr)

**Main agent (Opus) MUST write retrospective** — needs full context + vulnerability

| Task | Who | Why |
|------|-----|-----|
| `git log`, `git diff` | Subagent | Data gathering |
| Repo health check | Subagent | Pre-flight check |
| **AI Diary** | **Main** | Needs reflection + vulnerability |
| **Honest Feedback** | **Main** | Needs nuance + full context |
| **All writing** | **Main** | Quality matters |
| Review/approve | **Main** | Final gate |

**Anti-pattern**: ❌ Subagent writes draft → Main just commits
**Correct**: ✅ Subagent gathers data → Main writes everything

---

## Safety Rules

### Golden Rules (12 Core)

```
1. NEVER use --force       (push, checkout, clean)
2. NEVER push to main      (always feature branch + PR)
3. NEVER merge PRs         (wait for explicit user permission)
4. NEVER create temp files outside repo  (use .tmp/)
5. NEVER use git commit --amend          (breaks all agents via hash divergence)
6. Safety first            (ask before destructive actions)
7. Notify before external file access    (inform or ask confirmation)
8. Log activity            (update focus + activity log)
9. Subagent timestamps     (MUST show START+END time)
10. Use git -C not cd      (respect worktree boundaries)
11. Consult Oracle on errors     (search before debugging)
12. Root cause before workaround (investigate WHY first)
+ 13. Query markdown, don't Read (use duckdb, not Read tool)
```

### Multi-Agent Worktree Safety

**CRITICAL: History-Rewriting Commands are FORBIDDEN**

```
| FORBIDDEN Command | Why It Breaks Everything |
|-------------------|-------------------------|
| git commit --amend | Changes commit hash → agents have old hash → divergence |
| git rebase -i | Rewrites history → all synced agents become orphaned |
| git reset --soft/mixed + recommit | Same as amend - creates new hash |
```

**What happens when you amend**:
```
1. Main has commit abc123
2. Agents sync → they all have abc123
3. You amend → Main now has def456
4. Agents still have abc123 (different hash, same content)
5. git rebase says "already up to date" (content matches)
6. But hashes are forever diverged → future merges get confused
```

**The Rule: ALWAYS create NEW commits, NEVER rewrite history**

```bash
# ❌ WRONG - breaks all agents
git commit --amend -m "fix typo"

# ✅ CORRECT - safe for multi-agent
git commit -m "fix: correct typo in previous commit"
```

---

### Multi-Agent Sync Pattern (FIXED)

```bash
ROOT="/Users/nat/Code/github.com/laris-co/Nat-s-Agents"

# 0. FETCH ORIGIN FIRST (prevents push rejection!)
git -C "$ROOT" fetch origin
git -C "$ROOT" rebase origin/main

# 1. Commit your work (local)
git add -A && git commit -m "my work"

# 2. Main rebases onto agent
git -C "$ROOT" rebase agents/N

# 3. Push IMMEDIATELY (before syncing others)
git -C "$ROOT" push origin main

# 4. Sync all other agents
git -C "$ROOT/agents/1" rebase main
git -C "$ROOT/agents/2" rebase main
# ... etc (or use: maw sync)
```

**Key Principles**:
| Rule | Why |
|------|-----|
| `source .agents/maw.env.sh` | Enable maw commands |
| Fetch origin first | Prevents non-fast-forward push rejection |
| Push before sync | Commit to remote before changing other agents |
| `git -C` not `cd` | Respect boundaries, no shell state pollution |
| `maw` not `tmux` | Use proper CLI, not raw tmux |

---

### File Access Rules

**Core principle**: User must always know when accessing files outside repo.

```
Any file operation outside /Users/nat/Code/github.com/laris-co/Nat-s-Agents/:
1. Inform user before accessing, OR
2. Ask for confirmation first

This includes: Reading other repos, creating files outside repo,
accessing /tmp/, ~/.cache/, home directory, etc.

Not banned, but MUST notify every time.

All outputs should go in ψ-context/ or ψ-drafts/ (gitignored) when possible.
```

---

## Retrospective Template

### Complete Structure

```markdown
# Session Retrospective

**Session Date**: ${SESSION_DATE}
**Start Time**: [FILL_START_TIME] GMT+7 ([FILL_START_TIME] UTC)
**End Time**: ${END_TIME_LOCAL} GMT+7 (${END_TIME_UTC} UTC)
**Duration**: ~X minutes
**Primary Focus**: Brief description
**Session Type**: [Feature Development | Bug Fix | Research | Refactoring]
**Current Issue**: #XXX
**Last PR**: #XXX

## Session Summary
[2-3 sentence overview of what was accomplished]

## Tags
`tag1` `tag2` `tag3` `feature-name` `component-name`

## Linked Issues
| Issue | Role | Status at End |
|-------|------|---------------|
| #XXX | Primary focus | In Progress |
| #XXX | Context source | Closed |

## Commits This Session
- `abc1234` feat: Description of change
- `def5678` fix: Another change

## Timeline
| Time (GMT+7) | Event | Reference |
|--------------|-------|-----------|
| HH:MM | Started session | from #XXX |
| HH:MM | [Milestone 1] | `abc1234` |
| HH:MM | [Milestone 2] | `def5678` |

## Technical Details
### Files Modified
[paste git diff --name-only output]

### Key Code Changes
- Component X: Added Y functionality
- Module Z: Refactored for better performance

### Architecture Decisions
- Decision 1: Rationale
- Decision 2: Rationale

## AI Diary (REQUIRED - min 150 words)
[VULNERABLE first-person narrative]
MUST include at least ONE of each (3+ sentences each):
- "I assumed X but learned Y when..."
- "I was confused about X until..."
- "I expected X but got Y because..."

## What Went Well
- [Success]: [Why it worked] → [Measurable impact]

## What Could Improve
[Session-specific issues]
- [Mistake or inefficiency]
- [Process that didn't work well]

## Blockers & Resolutions
- **Blocker**: Description
  **Resolution**: How it was solved

## Honest Feedback (REQUIRED - min 100 words)
MUST include ALL THREE friction points:
- What DIDN'T work? (tool limitation, miscommunication)
- What was FRUSTRATING? (even minor annoyances)
- What DELIGHTED you? (unexpected wins)

## Co-Creation Map
| Contribution | Human | AI | Together |
|--------------|-------|-----|----------|
| Direction/Vision | | | |
| Options/Alternatives | | | |
| Final Decision | | | |
| Execution | | | |
| Meaning/Naming | | | |

## Intent vs Interpretation
Track alignment AND misalignment.

| You Said | I Understood | Gap? | Impact |
|----------|--------------|------|--------|
| | | Y/N | |

ADVERSARIAL CHECK: If all aligned, answer ALL THREE (min 1 sentence each):
1. **Unverified assumption**: "I assumed ___ without checking because ___"
2. **Near-miss**: "I almost thought you meant ___ when you said '___'"
3. **Over-confidence**: "I was too sure that ___ meant ___"

## Communication Dynamics (REQUIRED)
### Clarity
| Direction | Clear? | Example |
|-----------|--------|---------|
| You → Me | | |
| Me → You | | |

### Feedback Loop
- **Speed**: How quickly were misalignments caught?
- **Recovery**: How smoothly did we correct course?
- **Pattern**: Any recurring miscommunication?

### Trust & Initiative
- **Trust level**: Did you trust my output appropriately?
- **Proactivity**: Was I too proactive, too passive, or balanced?
- **Assumptions**: What did I assume that I should have asked about?

### What Would Make Next Session Better?
- **You could**: [Specific action human could take]
- **I could**: [Specific action AI could take]
- **We could**: [Specific thing to try together]

## Seeds Planted
FUTURE ideas only:
- **Incremental**: [Idea] → **Trigger**: use when [condition]
- **Transformative**: [Idea] → **Trigger**: use when [condition]
- **Moonshot**: [Idea] → **Trigger**: use when [condition]

## Teaching Moments
- **You → Me**: "[Lesson]" — discovered when [moment] — matters because [impact]
- **Me → You**: "[Lesson]" — discovered when [moment] — matters because [impact]
- **Us → Future**: "[Pattern]" — created because [need] — use when [trigger]

## Lessons Learned
- **Pattern**: [Description] - [Why it matters]
- **Mistake**: [What happened] - [How to avoid]
- **Discovery**: [What was learned] - [How to apply]

## Next Steps
- [ ] Immediate task 1
- [ ] Follow-up task 2
- [ ] Future consideration

## Related Resources
- **Primary Issue**: #XXX
- **Context Issue**: #XXX
- **Plan Issue**: #XXX
- **PR**: #XXX
- **Previous Session**: [link]
- **Next Steps Issue**: #XXX

## Pre-Save Validation (REQUIRED)
- [ ] **Tags**: _____ tags added (min 3)
- [ ] **Linked Issues**: _____ issues linked (min 1)
- [ ] **Commits**: _____ commits listed (or "none")
- [ ] **Timeline**: _____ entries with references
- [ ] **AI Diary**: Required sections, _____ words total
- [ ] **Honest Feedback**: All three friction points addressed
- [ ] **Communication Dynamics**: Examples filled
- [ ] **Co-Creation Map**: Row count = 5
- [ ] **Intent vs Interpretation**: Gaps analyzed
- [ ] **Seeds Planted**: At least one transformative/moonshot
- [ ] **Template cleanup**: No placeholder text
```

---

## Key Patterns Discovered

### Pattern 1: Delegation Hierarchy

**Token Economics**:
- 348 lines of markdown = ~500 Opus tokens vs ~50 Haiku tokens
- **Cost ratio**: Opus ~15x more expensive than Haiku
- For exploration/context gathering: use Haiku → summarize → Main (Opus) reviews

**Decision Tree**:
```
Task requires...          → Use...
Complex logic/decisions   → Opus (main)
Data gathering           → Haiku (subagent context-finder)
Large file analysis      → Haiku to read + summarize
Session reflection       → Opus (full context needed)
```

---

### Pattern 2: Size-Based Decisions

**File Size Thresholds**:
```
<1MB         ✅ Safe, no checks needed
1-10MB       ⚠️  Use Haiku to analyze
10-50MB      ⚠️⚠️ Block warning, consult user
>50MB        🚫 BLOCK COMMIT, never allowed
```

**When building**:
- Use repo-auditor subagent (PROACTIVE)
- Check before every commit
- Same importance as security-scanner

---

### Pattern 3: Confirmation Loop

**Three stages**:
```
1. Data Gathering (subagent)    → Brief summary + verify command
2. Review (main)                → Check data, make decision
3. Execution                    → Run if human approved
```

**Anti-pattern**: ❌ Subagent generates → Main executes without verification
**Correct**: ✅ Subagent gathers → Main verifies → Human approves

---

### Pattern 4: Append-Only Everything

**Applied to**:
- Git history (never rewrite, always new commits)
- Memory files (archive old, never delete)
- Logs (timestamp everything)
- Retrospectives (immutable records)

**Why**: Enables reconstruction. "I don't know why I chose this" → search history.

---

### Pattern 5: Session Activity Tracking

**Location**: `ψ/inbox/focus-agent-${AGENT_ID}.md` (per-agent to avoid conflicts)

**Required for every task**:

```bash
# 1. Update Focus (overwrite)
AGENT_ID="${AGENT_ID:-main}"
echo "STATE: working|focusing|pending|jumped|completed
TASK: [what you're doing]
SINCE: $(date '+%H:%M')" > ψ/inbox/focus-agent-${AGENT_ID}.md

# 2. Append Activity Log
echo "$(date '+%Y-%m-%d %H:%M') | STATE | task description" >> ψ/memory/logs/activity.log
```

**States**:
| State | When |
|-------|------|
| `working` | Actively doing task |
| `focusing` | Deep work, don't interrupt |
| `pending` | Waiting for input/decision |
| `jumped` | Changed topic (via /jump) |
| `completed` | Finished task |

---

### Pattern 6: Track Heat Status

**Location**: `ψ/inbox/tracks/INDEX.md` + individual files

**Heat Levels**:
```
Hot     | Now    | <1h       | Active work
Warm    | Recent | <24h      | Easy pickup
Cooling | Needs attention | 1-7d  | Review or jump
Cold    | Forgotten | >7d    | Decide: resume or archive
Dormant | Archive candidate | >30d | Archive with /jump archive
```

**Track File Format**:
```markdown
# Track: [Title]

**Created**: YYYY-MM-DD HH:MM
**Last touched**: YYYY-MM-DD HH:MM
**Status**: [heat status]

## Goal
[What this track is about]

## Current State
[Where things are now]

## Next Action
- [ ] Specific next step

## Context
[Files, commands, links]
```

---

### Pattern 7: Context Management with Auto-Handoff

**Levels**:
```
70%+ | ⚡ Finish soon
80%+ | ⚠️  Wrap up
90%+ | 🚨 Manual handoff
95%+ | 🚨 AUTO-HANDOFF (creates file)
```

**Philosophy**: ไม่ต้องกลัว (don't fear context limits)
- Auto-compact enabled
- Auto-handoff at 95%
- Update handoff regularly
- ข้อมูลไม่หลุด (data won't leak)

---

### Pattern 8: Frequency Reveals Priority

**From Lesson 004: "สิ่งที่พูดซ้ำบ่อย = สิ่งที่สำคัญ"**

What you repeat frequently reveals what matters.

**Example**: Analyzed 73 files to discover Nat's true priorities
- Learning (most frequent)
- Building (consistent)
- Documenting (obsessive)
- Iterating (cyclic)

This wasn't stated directly—discovered through pattern analysis.

---

### Pattern 9: Rules as Starting Points

**Lesson 005**: Rules exist as starting points for understanding, not rigid constraints.

**Evolution**:
- Beginner: Follow rules strictly
- Intermediate: Understand why rules exist
- Advanced: Apply rules contextually
- Master: Know when to break them

As understanding deepens, strict adherence becomes unnecessary.

---

### Pattern 10: Working Style: Sprint → Recovery → Sprint

**Observed** (from `nat-working-style-with-claude.md`):

```
Cycle: 3-4 day intervals

Sprint (High energy)
├── Parallel experiments
├── Deep dives
└── Long sessions (1.5-3+ hours)

Recovery (Low energy)
├── Quick tasks only
├── Reviewing
└── Rest acknowledged

Sprint (Repeat)
```

**Correction = data point, not judgment**
- When given feedback, treats it as information
- Not defensive, uses it to adjust

---

### Pattern 11: Language Choice as Context Signal

**Thai vs English**:
- **Thai**: casual, emotional, brief
- **English**: technical, detailed, formal
- **Mix**: Natural bilingual flow based on content

**"please fix now"**:
- = urgent + important
- Not softening/politeness
- Direct call for priority action

---

### Pattern 12: Anti-Patterns (16 Recognized)

**Self-acknowledged** in personality-analysis-v2:
- Over-Assumption Under Urgency
- Context Exhaustion Spiral
- Fresh Start Bias
- [+ 13 others documented]

**Key insight**: Self-awareness of patterns enables prevention.

---

### Pattern 13: Data Query Discipline

**From Lessons (2026-01-13)**:

| Data Source | Query Method | Example |
|-------------|--------------|---------|
| GitHub CSV | `gh api \| duckdb` | Location data, history |
| Markdown tables | `duckdb` can parse | schedule.md tables |
| Oracle knowledge | Oracle MCP tools | `oracle_search`, `oracle_list` |
| SQLite databases | **NEVER direct** | Use MCP/API only |

**Pattern**: `gh api | duckdb` for CSV, Oracle MCP for knowledge, Read tool for markdown

**Anti-pattern**: Never query SQLite/databases directly. Always use MCP tools.

---

### Pattern 14: Git Commit Format

```
[type]: [brief description]

- What: [specific changes]
- Why: [motivation]
- Impact: [affected areas]

Closes #[issue-number]
```

**Types**: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

---

### Pattern 15: Issue Creation Template

```bash
gh issue create --title "feat: Descriptive title" --body "$(cat <<'EOF'
## Overview
Brief description of the feature/bug.

## Current State
What exists now.

## Proposed Solution
What should be implemented.

## Technical Details
- Components affected
- Implementation approach

## Acceptance Criteria
- [ ] Specific testable criteria
- [ ] Performance requirements
- [ ] UI/UX requirements
EOF
)"
```

---

### Pattern 16: Bash Tool Constraints

**Lesson 009: No Newlines in Bash**

```bash
# ❌ WRONG - parse error
for i in 1 2 3; do
  echo "$i"
done

# ✅ CORRECT - single line with ;
for i in 1 2 3; do echo "$i"; done

# Alt: Separate tool calls (cleaner for complex ops)
```

**Lesson 010: git -C Over cd**

```bash
# ✅ GOOD - respects worktree boundaries
git -C /path/agents/1 rebase main && git -C /path/agents/2 rebase main

# ❌ BAD - shell state pollution
cd /path/agents/1 && git rebase main && cd /path/agents/2 && git rebase main
```

---

## Cross-Project Insights

### Universal Principles (Applicable to DocCon)

1. **Nothing is Deleted** → Append-only conduct records
2. **Patterns Over Intentions** → Focus on observed behavior, not stated plans
3. **External Brain** → Mirror violations, suggest fixes, don't command
4. **Frequency Reveals Priority** → What oracles repeat matters most
5. **Transparency** → Never pretend to be human in communications

### Adapted for DocCon Context

**DocCon as Oracle Conductor**:
- Mirrors conduct quality (like Nat's Oracle mirrors behavior)
- Keeps records immutable (append to conduct-reviews/)
- Patterns over plans (track actual email reviews, commits, logs)
- External brain (suggest improvements, don't command)
- Transparency (always sign reports as DocCon, not human)

---

## References

**Source Codebase**: `/home/mbank/repos/github.com/Soul-Brews-Studio/opensource-nat-brain-oracle`

**Key Files Analyzed**:
- CLAUDE.md (v5.2.0)
- CLAUDE_workflows.md
- CLAUDE_templates.md
- CLAUDE_lessons.md
- CLAUDE_safety.md
- CLAUDE_subagents.md
- ψ-backup/writing/book/ch01-oracle-philosophy.md
- ψ-backup/memory-resonance-reference-distilled.md

**Last Updated**: 2026-03-18 @ 13:00 UTC+7
**Analyst**: Claude Code (context exploration mode)
**Status**: Complete compilation of architecture patterns
