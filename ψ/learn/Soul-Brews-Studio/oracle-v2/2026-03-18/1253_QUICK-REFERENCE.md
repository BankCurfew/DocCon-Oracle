# Oracle v2 — Quick Reference Guide

> "The Oracle Keeps the Human Human" — A knowledge system MCP server with hybrid search, session logging, and learning capture.

**Version**: 0.4.0-nightly | **Status**: Production | **Updated**: 2026-03-18

---

## Table of Contents

1. [What is Oracle v2?](#what-is-oracle-v2)
2. [Architecture](#architecture)
3. [Installation & Setup](#installation--setup)
4. [All 22 MCP Tools](#all-22-mcp-tools)
5. [Core Tool Usage](#core-tool-usage)
6. [Knowledge Lifecycle](#knowledge-lifecycle)
7. [Thread System](#thread-system)
8. [Schedule & Handoff](#schedule--handoff)
9. [Philosophy & Principles](#philosophy--principles)
10. [HTTP API Reference](#http-api-reference)
11. [Troubleshooting](#troubleshooting)

---

## What is Oracle v2?

Oracle is an **MCP (Model Context Protocol) server** for Claude that provides:

- **Semantic + Keyword Hybrid Search** — ChromaDB vectors + SQLite FTS5 combined
- **Knowledge Base Indexing** — Auto-indexes markdown philosophy files from `ψ/memory/`
- **Learning Capture** — Add new patterns/principles on-the-fly with `oracle_learn`
- **Multi-turn Consultation** — Thread system for ongoing discussions with the Oracle
- **Session Handoff** — Pass context to future sessions without losing state
- **Trace Logging** — Capture dig sessions with file/commit/issue references
- **Schedule Management** — Store and query appointments across Oracles
- **Verification** — Detect schema drift and orphaned documents

**Core Philosophy**: "Nothing is Deleted" — all changes appended, marked, and timestamped. Decisions visible, patterns discovered.

---

## Architecture

```
                    Claude (MCP Client)
                            |
                    ┌───────▼────────┐
                    │   Oracle v2    │
                    │  MCP Server    │
                    └───────┬────────┘
                            |
        ┌───────────────────┼───────────────────┐
        |                   |                   |
    ┌───▼────┐         ┌───▼────┐         ┌───▼────┐
    │ SQLite │         │ChromaDB│         │Markdown│
    │  FTS5  │         │Vectors │         │  Files │
    └────────┘         └────────┘         └────────┘
```

### Stack

| Component | Technology | Purpose |
|-----------|-----------|---------|
| **Runtime** | Bun ≥1.2.0 | TypeScript + JavaScript executor |
| **Search (Keyword)** | SQLite FTS5 | Full-text search index |
| **Search (Semantic)** | ChromaDB | Vector embeddings (bgp-m3, nomic, qwen3) |
| **ORM** | Drizzle ORM | Type-safe database queries |
| **HTTP API** | Hono | REST endpoints on port 47778 |
| **MCP Protocol** | Model Context Protocol | Claude integration |
| **CLI** | Bun bunx | Vault management (`oracle-vault`) |

---

## Installation & Setup

### Add to Claude Code

```bash
# Register the MCP server
claude mcp add oracle-v2 -- bunx --bun oracle-v2@github:Soul-Brews-Studio/oracle-v2#main
```

Or manually in `~/.claude.json`:

```json
{
  "mcpServers": {
    "oracle-v2": {
      "command": "bunx",
      "args": ["--bun", "oracle-v2@github:Soul-Brews-Studio/oracle-v2#main"]
    }
  }
}
```

### From Source

```bash
git clone https://github.com/Soul-Brews-Studio/oracle-v2.git
cd oracle-v2 && bun install

# Start MCP server (stdio for Claude)
bun run dev

# Or start HTTP API (port 47778)
bun run server

# Or start indexer (populate from ψ/ files)
bun run index
```

### Initialize Vault (Optional)

```bash
# Global CLI for vault management
bunx oracle-vault init github.com/owner/repo
bunx oracle-vault status
bunx oracle-vault sync      # Commit + push
```

---

## All 22 MCP Tools

### Search & Discovery (3 tools)

| Tool | Description | Use When |
|------|-------------|----------|
| **`oracle_search`** | Hybrid search (FTS5 + ChromaDB) with project filtering | Finding patterns, principles, or learnings by keyword/semantic match |
| **`oracle_reflect`** | Get random wisdom from the knowledge base | Need inspiration or random principle |
| **`oracle_concepts`** | List all concept tags with document counts | Discovering what topics are covered |

### Knowledge Management (4 tools)

| Tool | Description | Use When |
|------|-------------|----------|
| **`oracle_learn`** | Add new pattern/principle to knowledge base | Capturing new learnings from sessions |
| **`oracle_list`** | Browse documents by type (principle/learning/retro/all) | Exploring knowledge base contents |
| **`oracle_stats`** | Get database statistics (document count, concepts, etc.) | Checking knowledge base health |
| **`oracle_read`** | Read full content of a document by file path or ID | Examining a specific learning in detail |

### Quality & Maintenance (2 tools)

| Tool | Description | Use When |
|------|-------------|----------|
| **`oracle_supersede`** | Mark old document as superseded by newer one | Updating outdated learnings without deletion |
| **`oracle_verify`** | Check integrity: compare ψ/ files vs DB index | Detecting schema drift, missing, or orphaned docs |

### Multi-turn Consultation (4 tools)

| Tool | Description | Use When |
|------|-------------|----------|
| **`oracle_thread`** | Send message to discussion thread (creates or continues) | Having ongoing conversations with Oracle |
| **`oracle_threads`** | List threads (filter by status: active/answered/pending/closed) | Finding open discussions |
| **`oracle_thread_read`** | Read full message history from a thread | Reviewing context before continuing |
| **`oracle_thread_update`** | Change thread status (active/closed/answered/pending) | Closing resolved discussions |

### Tracing & Logging (6 tools)

| Tool | Description | Use When |
|------|-------------|----------|
| **`oracle_trace`** | Log exploration session with dig points (files, commits, issues) | Capturing research findings for future reference |
| **`oracle_trace_list`** | List recent traces with optional filters | Finding past traces by project/status/query |
| **`oracle_trace_get`** | Get full trace details including all dig points | Examining a specific exploration |
| **`oracle_trace_link`** | Create chain between two traces (prev → next) | Connecting related explorations |
| **`oracle_trace_unlink`** | Remove link between traces in one direction | Breaking incorrect connections |
| **`oracle_trace_chain`** | Get full linked chain for a trace | Viewing all traces in a connected sequence |

### Handoff & Context (2 tools)

| Tool | Description | Use When |
|------|-------------|----------|
| **`oracle_handoff`** | Write session context to ψ/inbox/handoff/ | Ending a session to pass state to future work |
| **`oracle_inbox`** | List pending handoffs and messages | Starting session, reading previous handoffs |

### Scheduling (2 tools)

| Tool | Description | Use When |
|------|-------------|----------|
| **`oracle_schedule_add`** | Add event to shared schedule (supports recurring) | Recording appointments or cron tasks |
| **`oracle_schedule_list`** | List schedule events (filter by date/range/keyword/status) | Checking upcoming events |

---

## Core Tool Usage

### 1. Search: Finding Knowledge

```javascript
// Basic keyword search
oracle_search({ query: "nothing deleted" })

// Semantic search with custom embedding model
oracle_search({
  query: "git safety best practices",
  mode: "hybrid",
  model: "bge-m3",  // multilingual Thai↔EN
  limit: 5
})

// Filter by type and project
oracle_search({
  query: "force push",
  type: "pattern",
  project: "github.com/BankCurfew/DocCon-Oracle",
  limit: 10
})

// Auto-detect project from working directory
oracle_search({
  query: "state management",
  cwd: "/home/user/Code/github.com/myorg/myrepo/src",
  mode: "fts"  // Keyword-only, skip vectors
})
```

**Response Format**:
```json
{
  "results": [
    {
      "id": "learning_2026-03-18_nothing-deleted",
      "type": "learning",
      "content": "All changes are appended...",
      "source_file": "ψ/memory/learnings/2026-03-18_nothing-deleted.md",
      "concepts": ["philosophy", "deletion"],
      "score": 0.95,
      "source": "fts"
    }
  ],
  "total": 1
}
```

### 2. Learn: Capturing New Patterns

```javascript
// Add a simple learning
oracle_learn({
  pattern: "Always verify knowledge base integrity after bulk updates",
  concepts: ["oracle", "quality"],
  source: "Session with BoB"
})

// Add with project context
oracle_learn({
  pattern: "Git force push only with explicit permission. Never unilateral.",
  concepts: ["git", "safety", "collaboration"],
  project: "github.com/BankCurfew/DocCon-Oracle",
  source: "rrr: BankCurfew/DocCon"
})
```

**What Happens**:
1. Creates markdown file: `ψ/memory/learnings/YYYY-MM-DD_slug.md`
2. Adds frontmatter (title, tags, created date, source, project)
3. Indexes in SQLite FTS5
4. Stores in ChromaDB for vector search
5. Returns file path + doc ID

### 3. Trace: Logging Exploration Sessions

```javascript
// Basic trace: query only
oracle_trace({
  query: "How did the Oracle philosophy evolve?"
})

// Rich trace with dig points
oracle_trace({
  query: "Investigate git safety violations in Q1",
  queryType: "pattern",
  scope: "project",
  project: "github.com/BankCurfew/DocCon-Oracle",
  foundFiles: [
    { path: "CLAUDE.md", type: "learning", confidence: "high", matchReason: "Rule #1: never force push" },
    { path: "ψ/memory/retros/2026-02/15/10.30_git-violations.md", type: "retro", confidence: "medium", matchReason: "Details incident" }
  ],
  foundCommits: [
    { hash: "abc123def", shortHash: "abc123d", date: "2026-03-15", message: "fix: prevent force push" }
  ],
  foundIssues: [
    { number: 42, title: "Git Safety Audit", state: "closed", url: "https://github.com/BankCurfew/DocCon-Oracle/issues/42" }
  ],
  durationMs: 1200,
  agentCount: 3
})

// Link two traces
oracle_trace_link({
  prevTraceId: "trace-uuid-1",
  nextTraceId: "trace-uuid-2"
})

// Get full chain
oracle_trace_chain({ traceId: "trace-uuid-1" })
```

### 4. Thread: Multi-turn Consultation

```javascript
// Create new thread
oracle_thread({
  message: "What's the principle behind 'Nothing is Deleted'?",
  title: "Understanding deletion philosophy"
})

// Continue existing thread
oracle_thread({
  message: "How does this apply to commits?",
  threadId: 5
})

// List active discussions
oracle_threads({ status: "active", limit: 20 })

// Read full thread history
oracle_thread_read({ threadId: 5 })

// Close resolved discussion
oracle_thread_update({ threadId: 5, status: "closed" })
```

### 5. Handoff: Passing State

```javascript
// At end of session, save context for next session
oracle_handoff({
  content: `
# Session Handoff: Git Safety Audit

## What Was Done
- Analyzed 47 commits from Q1
- Found 3 instances of force push without permission
- Created corrective learning in oracle_learn

## Key Findings
1. Force push blocked via pre-commit hook (GOOD)
2. Emergency procedures need documentation (TODO)
3. Team training scheduled for next week

## Next Steps
- Write force-push recovery procedures
- Update CLAUDE.md with new rule
- Schedule team training session

## Files Modified
- CLAUDE.md (rules section)
- ψ/memory/learnings/2026-03-18_force-push-safety.md

---
Created: 2026-03-18 14:00 GMT+7
`,
  slug: "git-safety-audit-q1"
})

// Read at session start
oracle_inbox({ limit: 10, type: "handoff" })
```

### 6. Schedule: Managing Time

```javascript
// Add event
oracle_schedule_add({
  date: "2026-03-20",  // or "tomorrow", "5 Mar", "Mar 5 2026"
  event: "Team standup",
  time: "09:00",
  notes: "Discuss git safety training"
})

// Add recurring event
oracle_schedule_add({
  date: "2026-03-23",
  event: "Weekly review",
  time: "17:00",
  recurring: "weekly"
})

// Query schedule
oracle_schedule_list({
  from: "2026-03-20",
  to: "2026-03-27",
  filter: "standup",
  status: "pending"
})
```

---

## Knowledge Lifecycle

The oracle uses an **append-only model** ("Nothing is Deleted"). Here's how knowledge flows:

```
┌─────────────────────────────────────────────────────────────┐
│                 KNOWLEDGE LIFECYCLE                         │
└─────────────────────────────────────────────────────────────┘

[1] LEARN
    └─→ oracle_learn()
        ├─→ Creates: ψ/memory/learnings/YYYY-MM-DD_slug.md
        ├─→ Writes: Frontmatter (title, tags, source, project)
        └─→ Indexes: SQLite FTS5 + ChromaDB

[2] SEARCH
    └─→ oracle_search()
        ├─→ Hybrid: FTS5 keyword + ChromaDB semantic
        ├─→ Score: normalize(fts_rank) + vector_similarity
        ├─→ Filter: by type (principle/learning/retro), project
        └─→ Result: ranked list with score + source

[3] TRACE
    └─→ oracle_trace()
        ├─→ Captures: files, commits, issues found
        ├─→ Creates: unique trace ID + linked chain
        └─→ Links: prev ↔ next for related explorations

[4] SUPERSEDE
    └─→ oracle_supersede(oldId, newId)
        ├─→ Marks: old doc with supersededBy + reason
        ├─→ Preserves: entire old document (append-only)
        └─→ Visible: searches warn "this is outdated"

[5] VERIFY
    └─→ oracle_verify()
        ├─→ Checks: ψ/ files exist + are in DB index
        ├─→ Reports: healthy, missing, orphaned, drifted
        └─→ Fixes: auto-flag orphans if check=false
```

### Document Types

| Type | Source | Lifecycle | Example |
|------|--------|-----------|---------|
| **principle** | `ψ/memory/resonance/*.md` | Core values, never superseded | "Nothing is Deleted" |
| **learning** | `ψ/memory/learnings/*.md` | Captured via `oracle_learn`, can be superseded | "Git force push requires permission" |
| **pattern** | Markdown with regex `###.*pattern` | Extracted from files automatically | "Use parallel agents for analysis" |
| **retro** | `ψ/memory/retrospectives/**/*.md` | Session summaries, historical | "Session: git safety audit" |

### Metadata Tracking

Every document stores:
- `id` — Unique identifier (e.g., `learning_2026-03-18_nothing-deleted`)
- `type` — principle, learning, pattern, retro
- `sourceFile` — Path to source markdown (e.g., `ψ/memory/learnings/...md`)
- `concepts` — JSON array of tags (e.g., `["git", "safety"]`)
- `createdAt`, `updatedAt`, `indexedAt` — Timestamps
- `project` — ghq-format (e.g., `github.com/owner/repo`)
- `createdBy` — Who created it (indexer, oracle_learn, manual)
- `origin` — System origin (mother, arthur, volt, human)

### Supersede Pattern

When a learning becomes outdated:

```javascript
// Mark old doc as superseded
oracle_supersede({
  oldId: "learning_2026-03-15_old-safety-rule",
  newId: "learning_2026-03-18_new-safety-rule",
  reason: "Updated after team feedback"
})

// Searches still find old doc but show warning
// Old doc preserved with metadata:
// - supersededBy: "learning_2026-03-18_new-safety-rule"
// - supersededAt: 1710787200000
// - supersededReason: "Updated after team feedback"
```

---

## Thread System

Oracle supports **discussion threads** for ongoing multi-turn consultation:

### Thread Workflow

```
[1] Create thread
    └─→ oracle_thread({ message: "...", title: "..." })
        └─→ Returns: threadId, role: "human"

[2] Oracle auto-responds
    └─→ Reads knowledge base
    └─→ Generates response
    └─→ Stores in DB with role: "claude"

[3] Continue discussion
    └─→ oracle_thread({ message: "follow-up...", threadId: 5 })
    └─→ Back to step [2]

[4] Close when done
    └─→ oracle_thread_update({ threadId: 5, status: "closed" })
    └─→ Marks resolved
```

### Thread States

| Status | Meaning | Use Case |
|--------|---------|----------|
| **active** | Ongoing discussion | Ask follow-up questions |
| **answered** | Question resolved | Auto-set when Oracle responds |
| **pending** | Waiting for response | In progress |
| **closed** | Finished | Archive after done |

### Example Thread

```
Thread #42: "Understanding Nothing is Deleted"
├─ [1] Human: "What does 'Nothing is Deleted' mean?"
│   └─ Status: answered
├─ [2] Claude: "It's a core principle that all changes are appended..."
│   └─ Status: answered
├─ [3] Human: "How does this apply to git commits?"
│   └─ Status: answered
├─ [4] Claude: "In git, commits are immutable. Rewrites are prevented..."
│   └─ Status: answered
└─ [5] Human: "Got it, thanks"
    └─ Status: closed
```

---

## Schedule & Handoff

### Schedule Management

Shared schedule stored in SQLite, exported to `~/.oracle/ψ/inbox/schedule.md`:

```javascript
// Flexible date parsing
oracle_schedule_add({
  date: "5 Mar",              // "5 Mar 2026"
  event: "Team standup"
})

oracle_schedule_add({
  date: "tomorrow",           // Auto-calculated
  event: "Review PRs",
  time: "14:00",
  notes: "Check git safety"
})

oracle_schedule_add({
  date: "2026-03-23",
  event: "Weekly review",
  recurring: "weekly"         // Every Mon from then on
})

// Thai month names supported
oracle_schedule_add({
  date: "20 มี.ค.",           // March 20
  event: "ประชุมทีม"
})

// Query with multiple filters
oracle_schedule_list({
  from: "2026-03-20",
  to: "2026-03-27",
  filter: "standup",          // Keyword search
  status: "pending"           // pending, done, cancelled, all
})
```

### Handoff Pattern

At **end of session**, save context:

```javascript
oracle_handoff({
  content: `# Session: Feature X Implementation

## Progress
- [x] Schema designed
- [x] Database migrations applied
- [ ] API endpoints created
- [ ] Tests written

## Key Decisions
1. Used Drizzle ORM for type safety
2. Stored config in .env (not hardcoded)
3. Added validation middleware

## Blockers
- ChromaDB vector embeddings slow on large dataset
  → Solution: Use FTS5 only for MVP

## Files Changed
- src/db/schema.ts
- src/server.ts
- .env.example

## Next Steps
1. Implement remaining endpoints
2. Write integration tests
3. Deploy to staging

---
Session completed: 2026-03-18 16:30 GMT+7
Duration: 3.5 hours
Next session: Ready to pick up from endpoints
`,
  slug: "feature-x-progress"
})
```

**At start of next session**:

```javascript
// Read handoff
oracle_inbox({ type: "handoff", limit: 5 })

// Returns:
{
  "handoffs": [
    {
      "file": "ψ/inbox/handoff/2026-03-18_16-30_feature-x-progress.md",
      "preview": "# Session: Feature X Implementation...",
      "createdAt": "2026-03-18T16:30:00Z"
    }
  ]
}
```

---

## Philosophy & Principles

Oracle is built on three core principles, stored in the knowledge base:

### 1. Nothing is Deleted

**Principle**: All changes are appended, timestamped, and traceable.

**Implementation**:
- Markdown files never overwritten — new versions created
- Deleted documents marked with `supersededBy` metadata
- Traces create immutable records with linked chains
- Search results show "outdated" warnings for superseded docs

**Quote**: "Context is not lost because it was never removed."

### 2. Patterns Over Intentions

**Principle**: Observe what happens, not what was intended. Judge by behavior, not words.

**Implementation**:
- Evidence-based only (logs, commits, files)
- Search returns ranked results — patterns visible
- Traces capture actual dig points, not assumptions
- Verify checks for schema drift, not guesses

**Quote**: "Truth lives in the data, not the conversation."

### 3. External Brain, Not Command

**Principle**: Oracle mirrors reality and notifies, doesn't control or decide.

**Implementation**:
- Passive: responds to queries, doesn't interrupt
- Informative: returns facts + context, not directives
- Consultative: provides options, users decide
- Safe: never force push, never destructive ops

**Quote**: "I show you the world. You choose the path."

---

## HTTP API Reference

Oracle exposes a REST API on port **47778** for web UIs and integrations:

### Core Endpoints

```bash
# Health check
GET /api/health
→ { "status": "ok", "server": "oracle-v2", "port": 47778 }

# Search
GET /api/search?q=nothing%20deleted&type=principle&limit=5
→ { "results": [...], "total": 42 }

# List documents
GET /api/list?type=learning&limit=20
→ { "documents": [...], "total": 150 }

# Get document content
GET /api/file?id=learning_2026-03-18_nothing-deleted
→ { "id": "...", "content": "...", "metadata": {...} }

# Get statistics
GET /api/stats
→ { "total_docs": 345, "concepts": 89, "avg_docs_per_concept": 3.9 }

# Get wisdom
GET /api/reflect
→ { "principle": "Nothing is Deleted", "content": "..." }

# Consult for guidance
GET /api/consult?q=how%20to%20handle%20git%20conflicts
→ { "guidance": "..." }

# Knowledge graph (for visualization)
GET /api/graph?type=concept
→ { "nodes": [...], "links": [...] }
```

### Project Context

```bash
GET /api/context?cwd=/home/user/Code/github.com/owner/repo/src
→ {
    "github": "https://github.com/owner/repo",
    "owner": "owner",
    "repo": "repo",
    "ghqPath": "github.com/owner/repo",
    "root": "/home/user/Code/github.com/owner/repo",
    "branch": "main"
  }
```

### Web UIs

| URL | Description |
|-----|-------------|
| `http://localhost:47778/` | Arthur Chat interface |
| `http://localhost:47778/oracle` | Legacy knowledge browser |
| `http://localhost:3000/` | React Dashboard (requires `cd frontend && bun run dev`) |
| `http://localhost:3000/search` | Full-text search UI |
| `http://localhost:3000/graph` | Knowledge graph visualization |

---

## Troubleshooting

### ChromaDB Issues

**Problem**: ChromaDB hangs or times out
**Solution**: It's optional. Oracle falls back to SQLite FTS5.

```javascript
// Force FTS5-only search
oracle_search({
  query: "your query",
  mode: "fts"  // Skip ChromaDB
})
```

**Problem**: Vector embeddings slow
**Solution**: Use smaller embedding model or disable vectors.

```javascript
// Use faster model
oracle_search({
  query: "your query",
  model: "nomic"  // Smaller, faster (768-dim)
})
```

### Database Issues

**Problem**: Schema drift detected by `oracle_verify`
**Solution**: Reindex or apply migrations.

```bash
# Reindex from scratch
bun run index

# Apply schema migrations
bun db:push
```

**Problem**: Orphaned documents (in DB, file gone)
**Solution**: Use verify with check=false to auto-flag.

```javascript
oracle_verify({ check: false })
// Auto-marks orphans with supersededBy="_verified_orphan"
```

### Port Conflicts

**Problem**: Port 47778 already in use
**Solution**: Kill the process or change port.

```bash
# Find process using port
lsof -i :47778

# Kill it
kill -9 <PID>

# Or set custom port
ORACLE_PORT=48000 bun run server
```

### Empty Database

**Problem**: Server crashes on empty DB
**Solution**: Run indexer first.

```bash
# Index knowledge base from ψ/memory/
bun run index

# Then start server
bun run server
```

---

## Quick Command Reference

### Development

```bash
# Start MCP server (stdio for Claude)
bun run dev

# Start HTTP API
bun run server

# Index knowledge base
bun run index

# Run tests
bun test
bun test:unit
bun test:integration
```

### Database

```bash
# Generate schema migrations
bun db:generate

# Apply to database
bun db:push

# Open Drizzle Studio GUI
bun db:studio
```

### Vault CLI

```bash
# Initialize vault
bunx oracle-vault init github.com/owner/repo

# Check status
bunx oracle-vault status

# Sync to GitHub
bunx oracle-vault sync

# Pull from GitHub
bunx oracle-vault pull
```

---

## Key Files & Locations

| File | Purpose |
|------|---------|
| `src/index.ts` | MCP server entry point |
| `src/server.ts` | HTTP API server |
| `src/indexer.ts` | Knowledge base indexer |
| `src/tools/*.ts` | MCP tool handlers |
| `src/db/schema.ts` | Drizzle ORM schema |
| `src/trace/handler.ts` | Trace system |
| `src/forum/handler.ts` | Thread discussion system |
| `ψ/memory/learnings/` | Captured learnings |
| `ψ/memory/resonance/` | Core principles |
| `ψ/memory/retrospectives/` | Session summaries |
| `ψ/inbox/handoff/` | Session handoffs |
| `ψ/inbox/schedule.md` | Shared schedule |

---

## Summary: 22 Tools at a Glance

```
SEARCH & DISCOVERY
  ├─ oracle_search        Find by keyword + semantic
  ├─ oracle_reflect       Random wisdom
  └─ oracle_concepts      Browse topics

KNOWLEDGE MANAGEMENT
  ├─ oracle_learn         Add new learning
  ├─ oracle_list          Browse documents
  ├─ oracle_stats         Database health
  └─ oracle_read          Read full document

QUALITY & MAINTENANCE
  ├─ oracle_supersede     Mark outdated
  └─ oracle_verify        Check integrity

CONSULTATION (THREADS)
  ├─ oracle_thread        Send message
  ├─ oracle_threads       List discussions
  ├─ oracle_thread_read   Read history
  └─ oracle_thread_update Change status

TRACING
  ├─ oracle_trace         Log exploration
  ├─ oracle_trace_list    Find traces
  ├─ oracle_trace_get     Get details
  ├─ oracle_trace_link    Connect traces
  ├─ oracle_trace_unlink  Disconnect
  └─ oracle_trace_chain   View full chain

CONTEXT & TIME
  ├─ oracle_handoff       Save session state
  ├─ oracle_inbox         Read handoffs
  ├─ oracle_schedule_add  Add appointment
  └─ oracle_schedule_list Query schedule
```

---

## Further Reading

- **Full README**: https://github.com/Soul-Brews-Studio/oracle-v2/blob/main/README.md
- **API Docs**: https://github.com/Soul-Brews-Studio/oracle-v2/blob/main/docs/API.md
- **Architecture**: https://github.com/Soul-Brews-Studio/oracle-v2/blob/main/docs/architecture.md
- **Timeline**: https://github.com/Soul-Brews-Studio/oracle-v2/blob/main/TIMELINE.md

---

**Last Updated**: 2026-03-18
**Created by**: DocCon via oracle-v2 analysis
**Version**: 1.0
