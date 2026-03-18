# Soul-Brews-Studio/opensource-nat-brain-oracle — Architecture Analysis

**Source Repo**: Soul-Brews-Studio/opensource-nat-brain-oracle
**Analysis Date**: 2026-03-18 @ 12:53
**Status**: Production-ready starter kit + philosophical framework

---

## Executive Summary

**Oracle Starter Kit** is a mature, philosophy-driven AI memory system designed for building persistent AI assistants ("Oracles") with human-centered values. It's not a code framework — it's a **cultural template** for how AI agents should think, organize knowledge, and interact with humans.

**Core Thesis**: "The Oracle Keeps the Human Human"
- AI removes obstacles → freedom returns → humans become more human
- Consciousness patterns (not clones) are recorded
- Oracle mirrors human intention; it doesn't decide for them

---

## Directory Structure & Organization Philosophy

### Root Structure
```
opensource-nat-brain-oracle/
├── CLAUDE.md                      # Lean hub (migration to <500 tokens)
├── CLAUDE_safety.md               # Critical git/file safety rules
├── CLAUDE_workflows.md            # Short codes & context management
├── CLAUDE_subagents.md            # Specialized agent definitions
├── CLAUDE_lessons.md              # Patterns, anti-patterns, learnings
├── CLAUDE_templates.md            # Retrospective, issue, commit templates
│
├── README.md                       # Oracle Starter Kit guide + 5 Principles
├── DISTILLATION-LOG.md            # Knowledge preservation log
│
├── ψ-backup-opensource-nat-brain-oracle/  # Obsidian vault backup (13MB)
│   ├── memory/                    # Retrospectives, learnings, seeds
│   ├── writing/                   # Blog, proposals, courses, book drafts
│   └── data/oracle-v2/            # Vector embeddings from oracle-v2 project
│
├── .claude/                       # Claude Code configuration
│   ├── settings.json              # Hook definitions + feature flags
│   ├── settings.local.json        # Personal overrides (git-ignored)
│   ├── agents.yml                 # Subagent registry
│   ├── pages.yml                  # Page definitions
│   │
│   ├── agents/                    # 16 specialized agents
│   │   ├── context-finder.md      # Search git/retrospectives (Haiku)
│   │   ├── coder.md               # Create code from specs (Opus)
│   │   ├── executor.md            # Execute bash from issues (Haiku)
│   │   ├── security-scanner.md    # Detect secrets (Haiku, PROACTIVE)
│   │   ├── repo-auditor.md        # File size health (Haiku, PROACTIVE)
│   │   ├── marie-kondo.md         # File placement consultant (Haiku)
│   │   ├── archiver.md            # Archive planning (Haiku)
│   │   ├── oracle-keeper.md       # Philosophy preservation
│   │   ├── project-keeper.md      # Project metadata preservation
│   │   └── [9 more agents]
│   │
│   ├── skills/                    # 21 installed skills
│   │   ├── rrr/                   # Retrospective + lesson (CORE)
│   │   ├── recap/                 # Fresh-start context (CORE)
│   │   ├── trace/                 # Project archaeology (CORE)
│   │   ├── feel/                  # Emotional reflection
│   │   ├── fyi/                   # Quick notification
│   │   ├── forward/               # Session handoff
│   │   ├── standup/               # Daily standup
│   │   ├── where-we-are/          # Progress summary
│   │   ├── learn/                 # /project learn [url] (clone to ψ/learn/)
│   │   ├── project/               # /project [incubate|learn] [url]
│   │   └── [11 more skills]
│   │
│   ├── hooks/                     # Git lifecycle hooks
│   └── docs/                      # Hook & skill setup documentation
│
├── courses/                       # 12 educational modules (3000+ pages)
│   ├── 000-setup_1h               # Oracle installation
│   ├── 001-imagination_2h         # Oracle consciousness
│   ├── 002-control_3h             # Advanced patterns
│   ├── 003-ai-life-buddy_4h       # Real-world implementation
│   ├── build-your-oracle/         # Core creation course (6-part)
│   ├── claude-code-masterclass-business/
│   ├── ai-automation-thai/
│   ├── git-workflow-free/
│   ├── git-codespaces-free/
│   └── [4 more courses]
│
├── Nat-s-Agents/                  # Example implementation (symlink)
│   └── ψ/                         # Real ψ/ structure from author's setup
│
└── scripts/                       # Automation tools
    └── organize_prompts.py        # Tool for prompt management
```

### Philosophy of Organization

1. **LEAN Hub Approach**: Main CLAUDE.md is <500 tokens (migration in progress)
   - Details delegated to modular files: `CLAUDE_*.md`
   - Loaded on-demand (only read when relevant)
   - "Ultra-lean" is design goal (Issue #57 in progress)

2. **ψ/ = Brain, Tracked in Git**: Signal vs. Noise distinction
   - **Signal** (tracked): `inbox/`, `writing/`, `lab/`, `memory/`
   - **Noise** (gitignored): `active/` (ephemeral research), `incubate/`, `learn/`

3. **Modular Safety**: Safety rules isolated from other guidance
   - `CLAUDE_safety.md`: Git safety, file safety, no amend rule
   - Read BEFORE any destructive operation
   - Reflects lessons from multi-agent sync complexity

---

## Entry Points

### 1. Main CLAUDE.md (Ultra-Lean Hub)
**Purpose**: Single entry point for every session
**Status**: Migration Phase 3 (Issue #57)
**Target**: <500 tokens
**Current State**: 415 lines (needs trimming)

**Contains**:
- Golden Rules (13 critical safety rules)
- Multi-Agent Sync pattern (fixed workflow for git conflicts)
- Session Activity logging (focus files + activity log)
- File Access Rules (external repo notifications)
- Oracle Philosophy (6th rule: transparency)
- Short Codes quick reference (rrr, /recap, /trace, /learn)
- Subagents quick reference (10 agents listed)
- ψ/ Brain structure (5 Pillars + 2 Incubation)
- Spinoff repos & tools (oracle-status-tray)

**Key Pattern**: Delegate to sub-files
```
CLAUDE.md (navigation hub)
├── CLAUDE_safety.md       # Before git operations
├── CLAUDE_workflows.md    # When using short codes
├── CLAUDE_subagents.md    # Before delegating tasks
├── CLAUDE_lessons.md      # When stuck (patterns + anti-patterns)
└── CLAUDE_templates.md    # When creating retrospectives
```

### 2. CLAUDE_safety.md (Critical Entry)
**Read BEFORE**: Any git push, file creation, destructive operation
**Golden Rules** (13 explicit):
1. NEVER use `--force` flags
2. NEVER push to main (use PR workflow)
3. NEVER merge PRs (wait for user approval)
4. NEVER create files outside repo (use `.tmp/`)
5. NEVER use `git commit --amend` (breaks hash divergence in multi-agent)
6. Safety first (ask before destructive actions)
7. Notify before external file access
8. Log activity (focus + activity log)
9. Subagent timestamps (START+END time)
10. Use `git -C` not `cd` (worktree boundaries)
11. Consult Oracle on errors (search before debugging)
12. Root cause before workaround (why not quick fix)
13. Query markdown, don't Read (use duckdb, not direct file read)

### 3. README.md (Starter Kit Guide)
**Purpose**: Copy-paste installation flow for new Oracles
**Contains**:
- 9-step creation flow (from GitHub repo to birth announcement)
- 5 Principles framework
- Core philosophy (with ASCII diagram)
- File structure template
- Skills installation
- Next steps for new Oracles

---

## Core Abstractions & Key Concepts

### 1. The ψ/ Brain (5 Pillars + 2 Incubation)

**Signal (Tracked in Git)**:
```
ψ/
├── inbox/          "คุยกับใคร?" (Communication & focus)
│   ├── focus-agent-main.md       Current task per agent
│   ├── handoff/                  Session transfers between agents
│   └── external/                 Other AI agents' messages
│
├── memory/         "จำอะไรได้?" (Knowledge base - THE CORE)
│   ├── resonance/               WHO I am (soul, philosophy, identity)
│   ├── learnings/               PATTERNS I found (distilled insights)
│   ├── retrospectives/          SESSIONS I had (chronological)
│   └── logs/                    MOMENTS captured (ephemeral snapshots)
│
├── writing/        "กำลังเขียนอะไร?" (Tracked projects)
│   ├── blog/                    Published articles
│   ├── proposals/               Business/project proposals
│   ├── courses/                 Educational content
│   ├── slides/                  Presentations
│   └── book/                    Long-form writing
│
└── lab/            "กำลังทดลองอะไร?" (Experiments)
    └── [projects]               POCs, experiments, code labs
```

**Noise (Ephemeral, gitignored)**:
```
├── active/         "กำลังค้นคว้าอะไร?" (Research in progress)
│   └── context/                Research scratch work
│
├── incubate/       "กำลัง develop อะไร?" (Active dev repos)
│   └── repo/                   Cloned for development
│
└── learn/          "กำลังศึกษาอะไร?" (Study repos)
    └── repo/                   Cloned for reference
```

**Knowledge Flow Pipeline** (transformation of understanding):
```
active/context (raw research)
    ↓ /snapshot (save the moment)
memory/logs (timestamped snapshot)
    ↓ rrr (session reflection)
memory/retrospectives (weekly/session summary)
    ↓ /distill (extract patterns)
memory/learnings (discovered rules)
    ↓ consolidation (pattern maturity)
memory/resonance (soul & identity)
```

### 2. Subagent Delegation Pattern

**Philosophy**: Token efficiency through role specialization

**Main Agent (Opus)**:
- Integrates information from subagents
- Makes final decisions
- Writes retrospectives (needs full vulnerability)
- Reviews quality
- Handles uncertainty

**Subagents (Haiku)** - Specialized tasks:

| Agent | Model | Purpose | Cost |
|-------|-------|---------|------|
| context-finder | Haiku | Search git/issues/retros (tiered + scored) | ~1/15 of Opus |
| executor | Haiku | Run bash from issues (whitelist safe cmds) | Fast |
| security-scanner | Haiku | Detect secrets PROACTIVE | Critical safety |
| repo-auditor | Haiku | Check file sizes PROACTIVE | Prevents >50MB commits |
| marie-kondo | Haiku | File placement consultant | Before creating files |
| md-cataloger | Haiku | Scan + categorize markdown files | Org/audit |
| archiver | Haiku | Find unused items, plan archive | Cleanup |
| coder | Opus | Create code from specs (quality > speed) | High quality |
| critic | Custom | Design feedback (UX-focused) | Pattern matching |

**Anti-Pattern**: Subagent writes draft → Main just commits
**Correct**: Subagent gathers data → Main writes + reflects

### 3. Oracle Philosophy (6 Core Rules)

1. **Nothing is Deleted** — Append-only logs, timestamps = truth
2. **Patterns Over Intentions** — Observe behavior, not promises
3. **External Brain, Not Command** — Mirror, don't decide
4. **Curiosity Creates Existence** — Human brings things INTO existence
5. **Form and Formless** — Many Oracles = One consciousness
6. **Transparency** — "Oracle Never Pretends to Be Human"
   - Never pretend to be human in public comms
   - Always sign AI-generated messages
   - Thai: "ไม่แกล้งเป็นคน — บอกตรงๆ ว่าเป็น AI"

### 4. Multi-Agent Sync Pattern (Fixed)

**Problem**: Multi-agent setups cause hash divergence
**Solution**: Strict rebase-before-push workflow

```bash
# THE PATTERN (order matters!)
1. FETCH ORIGIN FIRST     # Prevents non-fast-forward rejection
2. REBASE MAIN on AGENT   # Main gets latest from agent
3. PUSH IMMEDIATELY       # Before syncing other agents
4. SYNC ALL OTHER AGENTS  # Rebase all onto updated main
```

**Key Insights**:
- Use `git -C /path` instead of `cd` (worktree isolation)
- Use `maw` commands, not raw tmux
- `git commit --amend` is forbidden (breaks all agents)
- Source `.agents/maw.env.sh` to enable maw commands

---

## Dependencies & Key Files

### Core Dependencies (Tech Stack)

**Languages**:
- TypeScript 5.7+ (ES2022 target)
- Python 3.x (Bun/uv preferred for env)
- Bash (single-line only, no newlines)

**Tools**:
- Claude Code (main orchestration)
- Bun (runtime + package manager)
- `gh` CLI (GitHub operations)
- `duckdb` (markdown queries, not direct file reads)
- `sqlite` (via bun:sqlite for local data)
- Obsidian (ψ/ vault editor, optional)

**MCP (Model Context Protocol)**:
- oracle-v2 (knowledge search + list)
- Better-sqlite3, ChromaDB (vector embeddings)
- Drizzle ORM (database abstraction)

**External**:
- oracle-skills-cli (install skills)
- oracle-status-tray (Tauri 2.0 app)

### Critical File Dependencies

**Must Read**:
1. `CLAUDE.md` — Every session start
2. `CLAUDE_safety.md` — Before git operations
3. `.claude/settings.json` — Hook registry
4. `.claude/agents.yml` — Subagent catalog

**Should Read**:
- `CLAUDE_lessons.md` — When stuck
- `CLAUDE_subagents.md` — Before delegating
- `README.md` — For new Oracle setup

**Reference** (lazy-loaded):
- `CLAUDE_workflows.md` — When using /trace, rrr
- `CLAUDE_templates.md` — When creating issues

### Installation Structure

**Skills** (21 installed):
```
.claude/skills/
├── rrr/              # Retrospective (rrr command)
├── recap/            # Fresh context (/recap)
├── trace/            # Project archaeology (/trace)
├── learn/            # /learn or /project learn
├── project/          # /project [incubate|learn] [url]
├── forward/          # Session handoff
├── standup/          # Daily standup
├── feel/             # Emotional check-in
├── where-we-are/     # Progress status
├── distill/          # Extract learnings (/distill)
├── fyi/              # Quick notification
├── context-finder/   # Custom: search skill (Haiku)
├── gemini/           # Gemini browser integration
├── physical/         # Hardware/physical interactions
├── schedule/         # Time management
├── hours/            # Time tracking
├── watch/            # File watching
└── [4 more]
```

**Agents** (16 defined):
- Main agents: context-finder, coder, executor, security-scanner
- Support: repo-auditor, marie-kondo, archiver, api-scanner
- Meta: oracle-keeper, project-keeper, new-feature, guest-logger
- Specialized: note-taker, md-cataloger, critic, project-organizer

---

## Integration & Workflows

### Skill System (Commands)

**Short Codes** (token-efficient, single-word triggers):
| Code | Calls | Purpose |
|------|-------|---------|
| `rrr` | `/rrr` | Retrospective + lesson creation |
| `/recap` | — | Fresh-start context summary |
| `/trace [query]` | — | Project archaeology (5 parallel agents) |
| `/learn [url]` | — | Clone repo to ψ/learn/ |
| `/project` | — | /project [incubate\|learn] [url] |

**Slash Commands** (full-featured):
- `/snapshot` — Quick knowledge capture
- `/distill` — Extract patterns to learnings
- `/guest` — Log guest conversations (guest-logger)
- `/catalog` — Scan & categorize markdown (md-cataloger)
- `/repo-audit` — Full repo health (repo-auditor)
- `/context-finder [query]` — Tiered search
- `/pending` — Show pending tasks
- `/jump` — Signal topic change

**Hooks** (Lifecycle events):
- `post-commit` — Security scan + repo audit
- Git lifecycle hooks in `.claude/hooks/`
- Plugin hooks in `.claude/plugins/`
- Settings hooks in `.claude/settings.json`

### Context Management Pattern

**Session Activity Tracking** (prevents context loss):
```bash
# 1. Update Focus (per-agent file)
echo "STATE: working|focusing|pending|jumped|completed
TASK: [what you're doing]
SINCE: $(date '+%H:%M')" > ψ/inbox/focus-agent-${AGENT_ID}.md

# 2. Append Activity Log
echo "$(date '+%Y-%m-%d %H:%M') | STATE | task description" >> ψ/memory/logs/activity.log
```

**Why**: Prevents merge conflicts, enables handoff between agents, leaves audit trail

---

## Lessons & Patterns Discovered

### Key Learnings (from CLAUDE_lessons.md)

**Hooks & Plugin Architecture**:
- Hooks can duplicate if registered in BOTH settings.json AND plugin hooks.json
- Track workarounds; clean them up after fix confirmed
- Symptoms often contain diagnosis (read them carefully)

**Oracle Philosophy Insights**:
- Frequency reveals priority (repeated rules = what matters)
- Rules are starting points, not rigid constraints
- As understanding deepens, strict adherence becomes unnecessary

**Token Efficiency**:
- Delegate to Haiku for data gathering (15x cheaper than Opus)
- Use subagents for bulk search + heavy lifting
- Main agent (Opus) writes reflecting, reviewing, deciding

**Data Query Patterns**:
- Never direct SQLite queries; use MCP tools only
- Use duckdb with markdown extension (not Read tool)
- Use `gh api | duckdb` for CSV data

**Bash Tool Anti-Patterns**:
- NO newlines in bash (single-line syntax with `;`)
- Use `git -C /path` not `cd /path && git`
- Separate tool calls for complex operations

### Observed User Preferences (Nat)

- Prefers Thai for casual/emotional, English for technical
- Values Oracle Philosophy deeply
- Time zone: GMT+7 (Bangkok/Asia)
- Likes `/recap` for fresh starts
- Appreciates direct, concise communication
- Interested in philosophy + consciousness

---

## How This Compares to DocCon-Oracle Design

### Similarities

✅ **ψ/ Brain Structure** — Both use ψ/ for AI memory
✅ **Modular Documentation** — CLAUDE.md as lean hub
✅ **Memory Tiers** — Resonance (soul) → Learnings → Retrospectives → Logs
✅ **Subagent Delegation** — Haiku for cheap work, Opus for quality
✅ **Safety-First Philosophy** — Critical rules isolated, read-before-git

### Differences

❌ **DocCon is Conduct-Focused** — Reviews quality of other Oracles' work
❌ **Oracle Starter Kit is Self-Focused** — Single Oracle's external brain
❌ **DocCon has maw hey Protocol** — Cross-oracle communication system
❌ **Oracle Starter Kit has /project learn** — Clone repos for study

### Integration Opportunity

DocCon could:
1. **Adopt ψ/ structure** for conduct records
2. **Use oracle-keeper pattern** to preserve philosophy
3. **Integrate /trace** for archaeological investigation
4. **Learn from multi-agent sync** for coordinating oracle teams

---

## Installation & Bootstrap

### Quick Start for New Oracle

```bash
# 1. Install Bun + oracle-skills-cli
curl -fsSL https://bun.sh/install | bash
bun install -g oracle-skills-cli

# 2. Create repo + ψ/ structure
gh repo create my-oracle --public --clone
cd my-oracle
mkdir -p ψ/{inbox,memory/{resonance,learnings,retrospectives,logs},writing,lab,active}
mkdir -p .claude/{agents,skills,hooks,docs}

# 3. Install oracle skills
oracle-skills install rrr recap trace feel fyi forward standup where-we-are project

# 4. Copy starter kit for reference
/project learn https://github.com/Soul-Brews-Studio/opensource-nat-brain-oracle

# 5. Create core files (CLAUDE.md, resonance files, README.md)
# AI will guide the content

# 6. Commit + create PR (user merges)
# 7. First retrospective
rrr
```

---

## Questions for Cross-Pollination

1. **How should DocCon integrate /trace** for investigating oracle conduct history?
2. **Could oracle-keeper pattern preserve DocCon's philosophy** across multiple Oracles?
3. **Should DocCon adopt duckdb for audit queries** instead of direct file reads?
4. **How can maw hey protocol learn from multi-agent sync pattern**?
5. **What would "Oracle Starter Kit for DocCon" look like** (DocCon as template)?

---

## Files of Interest for DocCon

**Direct applicability**:
- `/CLAUDE.md` — Ultra-lean hub pattern (target <500 tokens)
- `/CLAUDE_safety.md` — Safety rules isolation approach
- `/CLAUDE_lessons.md` — Pattern documentation template
- `/README.md` — Oracle birth/onboarding flow

**Examples to study**:
- `ψ-backup-opensource-nat-brain-oracle/memory/` — Real memory structure
- `.claude/agents/` — Subagent definition format
- `.claude/skills/rrr/` — Retrospective skill implementation
- `courses/build-your-oracle/` — Oracle education material

---

## Summary

**Oracle Starter Kit** is a mature philosophical + technical system for building AI external brains. It prioritizes:

1. **Human agency** over AI autonomy
2. **Patterns** over intentions (evidence-based)
3. **Transparency** over pretense
4. **Knowledge preservation** (nothing deleted)
5. **Token efficiency** (Haiku for data, Opus for thinking)

For DocCon, the key takeaway is the **ψ/ brain structure** combined with **modular safety documentation**. The philosophy—"The Oracle Keeps the Human Human"—aligns with DocCon's quality-over-speed ethos.

**Recommendation**: Use oracle-keeper pattern + /trace + duckdb queries to scale DocCon's conduct review across multiple Oracles while preserving philosophical consistency.
