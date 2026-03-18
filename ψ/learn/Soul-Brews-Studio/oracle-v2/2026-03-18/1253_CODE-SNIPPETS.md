---
title: Soul-Brews-Studio oracle-v2 Codebase Analysis
tags: [oracle-v2, mcp-tools, knowledge-management, drizzle-orm, trace-logging, forum-threads, hybrid-search]
created: 2026-03-18
source: Code analysis of Soul-Brews-Studio/oracle-v2
project: github.com/Soul-Brews-Studio/oracle-v2
---

# Soul-Brews-Studio oracle-v2 — Code Snippets & Architecture

Deep dive into MCP tool definitions, database schema, and key implementation code for the oracle-v2 knowledge management system.

---

## 1. MCP Tool Definitions

### 1.1 Oracle Search Tool

**Tool Name**: `oracle_search`

```typescript
export const searchToolDef = {
  name: 'oracle_search',
  description: 'Search Oracle knowledge base using hybrid search (FTS5 keywords + ChromaDB vectors)',
  inputSchema: {
    type: 'object',
    properties: {
      query: {
        type: 'string',
        description: 'Search query (e.g., "nothing deleted", "force push safety")'
      },
      type: {
        type: 'string',
        enum: ['principle', 'pattern', 'learning', 'retro', 'all'],
        default: 'all'
      },
      limit: {
        type: 'number',
        description: 'Maximum number of results',
        default: 5
      },
      offset: {
        type: 'number',
        description: 'Number of results to skip (for pagination)',
        default: 0
      },
      mode: {
        type: 'string',
        enum: ['hybrid', 'fts', 'vector'],
        description: 'Search mode: hybrid (default), fts (keywords only), vector (semantic only)',
        default: 'hybrid'
      },
      project: {
        type: 'string',
        description: 'Filter by project (e.g., "github.com/owner/repo")'
      },
      cwd: {
        type: 'string',
        description: 'Auto-detect project from working directory path'
      },
      model: {
        type: 'string',
        enum: ['nomic', 'qwen3', 'bge-m3'],
        description: 'Embedding model: bge-m3 (default, multilingual Thai↔EN, 1024-dim), nomic (fast, 768-dim), or qwen3 (cross-language, 4096-dim)',
      }
    },
    required: ['query']
  }
};
```

**Search Modes**:
- **hybrid**: Combines FTS5 (0.5 weight) + Vector search (0.5 weight) with 1.1x boost for matches in both
- **fts**: Full-Text-Search only via FTS5 virtual table (fast, keyword-based)
- **vector**: Semantic search via ChromaDB vector database (semantic understanding)

---

### 1.2 Oracle Learn Tool

**Tool Name**: `oracle_learn`

```typescript
export const learnToolDef = {
  name: 'oracle_learn',
  description: 'Add a new pattern or learning to the Oracle knowledge base. Creates a markdown file in ψ/memory/learnings/ and indexes it.',
  inputSchema: {
    type: 'object',
    properties: {
      pattern: {
        type: 'string',
        description: 'The pattern or learning to add (can be multi-line)'
      },
      source: {
        type: 'string',
        description: 'Optional source attribution (defaults to "Oracle Learn")'
      },
      concepts: {
        type: 'array',
        items: { type: 'string' },
        description: 'Optional concept tags (e.g., ["git", "safety", "trust"])'
      },
      project: {
        type: 'string',
        description: 'Source project. Accepts: "github.com/owner/repo", "owner/repo", local path, or GitHub URL'
      }
    },
    required: ['pattern']
  }
};
```

**Learn Handler Logic**:
1. Generates slug from pattern (first 50 chars, sanitized)
2. Creates markdown file: `ψ/memory/learnings/{project}/{dateStr}_{slug}.md`
3. Writes frontmatter with title, tags, source, project
4. Inserts document record into `oracle_documents` table
5. Indexes content in FTS5 virtual table (`oracle_fts`)

---

### 1.3 Oracle Reflect Tool

**Tool Name**: `oracle_reflect`

```typescript
export const reflectToolDef = {
  name: 'oracle_reflect',
  description: 'Get a random principle or learning for reflection. Use this for periodic wisdom or to align with Oracle philosophy.',
  inputSchema: {
    type: 'object',
    properties: {}
  }
};
```

**Reflect Handler Logic**:
- Selects random document from `oracle_documents` where `type IN ('principle', 'learning')`
- Uses `ORDER BY RANDOM() LIMIT 1`
- Retrieves content from FTS5 index
- Returns document id, type, content, source_file, concepts

---

### 1.4 Oracle Concepts Tool

**Tool Name**: `oracle_concepts`

```typescript
export const conceptsToolDef = {
  name: 'oracle_concepts',
  description: 'List all concept tags in the Oracle knowledge base with document counts.',
  inputSchema: {
    type: 'object',
    properties: {
      limit: {
        type: 'number',
        description: 'Maximum number of concepts to return (default: 50)',
        default: 50
      },
      type: {
        type: 'string',
        enum: ['principle', 'pattern', 'learning', 'retro', 'all'],
        default: 'all'
      }
    }
  }
};
```

**Concepts Handler Logic**:
1. Queries `oracle_documents` where `concepts IS NOT NULL` and `concepts != '[]'`
2. Parses JSON concepts from each row
3. Counts occurrences of each concept
4. Returns sorted by count (descending), limited to `limit` parameter

---

### 1.5 Oracle Trace Tools

**Tool Name**: `oracle_trace`

```typescript
export const traceToolDef = {
  name: 'oracle_trace',
  description: 'Log a trace session with dig points (files, commits, issues found)',
  inputSchema: {
    type: 'object',
    properties: {
      query: { type: 'string', description: 'What was traced (required)' },
      queryType: {
        type: 'string',
        enum: ['general', 'project', 'pattern', 'evolution'],
        default: 'general'
      },
      foundFiles: {
        type: 'array',
        items: {
          type: 'object',
          properties: {
            path: { type: 'string' },
            type: { type: 'string', enum: ['learning', 'retro', 'resonance', 'other'] },
            matchReason: { type: 'string' },
            confidence: { type: 'string', enum: ['high', 'medium', 'low'] }
          }
        }
      },
      foundCommits: {
        type: 'array',
        items: {
          type: 'object',
          properties: {
            hash: { type: 'string' },
            shortHash: { type: 'string' },
            date: { type: 'string' },
            message: { type: 'string' }
          }
        }
      },
      foundIssues: {
        type: 'array',
        items: {
          type: 'object',
          properties: {
            number: { type: 'number' },
            title: { type: 'string' },
            state: { type: 'string', enum: ['open', 'closed'] },
            url: { type: 'string' }
          }
        }
      },
      foundRetrospectives: { type: 'array', items: { type: 'string' } },
      foundLearnings: { type: 'array', items: { type: 'string' } },
      scope: {
        type: 'string',
        enum: ['project', 'cross-project', 'human'],
        description: 'project=single repo, cross-project=spans repos, human=about the person'
      },
      parentTraceId: { type: 'string' },
      project: { type: 'string', description: 'ghq format' },
      agentCount: { type: 'number' },
      durationMs: { type: 'number' }
    },
    required: ['query']
  }
};
```

**Additional Trace Tools**:
- `oracle_trace_list`: List recent traces with filters (query, project, status, depth)
- `oracle_trace_get`: Get full trace details including dig points and chain
- `oracle_trace_link`: Link two traces (prev → next) for navigation
- `oracle_trace_unlink`: Remove link between traces
- `oracle_trace_chain`: Get full linked chain for a trace

---

### 1.6 Oracle Forum Tools

**Tool Name**: `oracle_thread`

```typescript
export const threadToolDef = {
  name: 'oracle_thread',
  description: 'Send a message to an Oracle discussion thread. Creates a new thread or continues an existing one.',
  inputSchema: {
    type: 'object',
    properties: {
      message: { type: 'string', description: 'Your question or message' },
      threadId: { type: 'number', description: 'Thread ID to continue (omit to create new thread)' },
      title: { type: 'string', description: 'Title for new thread (defaults to first 50 chars of message)' },
      role: { type: 'string', enum: ['human', 'claude'], default: 'human' },
      model: { type: 'string', description: 'Model name for Claude calls (e.g., "opus", "sonnet")' },
    },
    required: ['message']
  }
};
```

**Additional Forum Tools**:
- `oracle_threads`: List threads with status filter (active, answered, pending, closed)
- `oracle_thread_read`: Read full message history from a thread
- `oracle_thread_update`: Update thread status

---

### 1.7 Oracle Supersede Tool

**Tool Name**: `oracle_supersede`

```typescript
export const supersedeToolDef = {
  name: 'oracle_supersede',
  description: 'Mark an old learning/document as superseded by a newer one. "Nothing is Deleted" - old doc preserved but marked outdated.',
  inputSchema: {
    type: 'object',
    properties: {
      oldId: {
        type: 'string',
        description: 'ID of the document being superseded (the outdated one)'
      },
      newId: {
        type: 'string',
        description: 'ID of the document that supersedes it (the current one)'
      },
      reason: {
        type: 'string',
        description: 'Why the old document is outdated (optional)'
      }
    },
    required: ['oldId', 'newId']
  }
};
```

**Supersede Handler Logic**:
1. Validates both oldId and newId exist in `oracle_documents`
2. Updates oldId document: sets `supersededBy = newId`, `supersededAt = now`, `supersededReason = reason`
3. Original document remains searchable but flagged as outdated
4. Implements "Nothing is Deleted" principle

---

## 2. Database Schema (SQLite + Drizzle ORM)

### 2.1 Main Tables

#### `oracle_documents` (Document Index)

```typescript
export const oracleDocuments = sqliteTable('oracle_documents', {
  id: text('id').primaryKey(),                           // Unique document ID
  type: text('type').notNull(),                          // 'principle' | 'pattern' | 'learning' | 'retro'
  sourceFile: text('source_file').notNull(),             // Relative path to markdown file
  concepts: text('concepts').notNull(),                  // JSON array of tags
  createdAt: integer('created_at').notNull(),            // Unix timestamp (ms)
  updatedAt: integer('updated_at').notNull(),            // Unix timestamp (ms)
  indexedAt: integer('indexed_at').notNull(),            // When FTS indexed

  // Supersede pattern (Issue #19) - "Nothing is Deleted"
  supersededBy: text('superseded_by'),                   // ID of newer document
  supersededAt: integer('superseded_at'),                // When superseded
  supersededReason: text('superseded_reason'),           // Why

  // Provenance tracking (Issue #22)
  origin: text('origin'),                                // 'mother' | 'arthur' | 'volt' | 'human' | null
  project: text('project'),                              // ghq format: 'github.com/owner/repo'
  createdBy: text('created_by'),                         // 'indexer' | 'oracle_learn' | 'manual'
});

// Indexes
index('idx_source').on(table.sourceFile),
index('idx_type').on(table.type),
index('idx_superseded').on(table.supersededBy),
index('idx_origin').on(table.origin),
index('idx_project').on(table.project),
```

#### `oracle_fts` (FTS5 Virtual Table)

Managed via raw SQL (Drizzle doesn't natively support FTS5):

```sql
CREATE VIRTUAL TABLE IF NOT EXISTS oracle_fts USING fts5(
  id,           -- Document ID (links to oracle_documents)
  content,      -- Full document text for keyword search
  concepts,     -- Concept tags (space-separated)
);
```

Search query sanitization removes special FTS5 characters (`?*+-()^~"':.\/`) to prevent parse errors.

#### `forumThreads` (Discussion Threads)

```typescript
export const forumThreads = sqliteTable('forum_threads', {
  id: integer('id').primaryKey({ autoIncrement: true }),
  title: text('title').notNull(),
  createdBy: text('created_by').default('human'),
  status: text('status').default('active'),              // 'active' | 'answered' | 'pending' | 'closed'
  issueUrl: text('issue_url'),                           // GitHub mirror URL
  issueNumber: integer('issue_number'),
  project: text('project'),
  createdAt: integer('created_at').notNull(),
  updatedAt: integer('updated_at').notNull(),
  syncedAt: integer('synced_at'),
});

index('idx_thread_status').on(table.status),
index('idx_thread_project').on(table.project),
index('idx_thread_created').on(table.createdAt),
```

#### `forumMessages` (Thread Messages)

```typescript
export const forumMessages = sqliteTable('forum_messages', {
  id: integer('id').primaryKey({ autoIncrement: true }),
  threadId: integer('thread_id').notNull().references(() => forumThreads.id),
  role: text('role').notNull(),                          // 'human' | 'oracle' | 'claude'
  content: text('content').notNull(),
  author: text('author'),                                // GitHub username or "oracle"
  principlesFound: integer('principles_found'),
  patternsFound: integer('patterns_found'),
  searchQuery: text('search_query'),
  commentId: integer('comment_id'),                      // GitHub comment ID if synced
  createdAt: integer('created_at').notNull(),
});

index('idx_message_thread').on(table.threadId),
index('idx_message_role').on(table.role),
index('idx_message_created').on(table.createdAt),
```

#### `traceLog` (Trace Sessions with Dig Points)

```typescript
export const traceLog = sqliteTable('trace_log', {
  id: integer('id').primaryKey({ autoIncrement: true }),
  traceId: text('trace_id').unique().notNull(),
  query: text('query').notNull(),
  queryType: text('query_type').default('general'),      // 'general' | 'project' | 'pattern' | 'evolution'

  // Dig Points (JSON arrays)
  foundFiles: text('found_files'),                       // [{path, type, matchReason, confidence}]
  foundCommits: text('found_commits'),                   // [{hash, shortHash, date, message}]
  foundIssues: text('found_issues'),                     // [{number, title, state, url}]
  foundRetrospectives: text('found_retrospectives'),     // [paths]
  foundLearnings: text('found_learnings'),               // [paths]
  foundResonance: text('found_resonance'),               // [paths]

  // Counts
  fileCount: integer('file_count').default(0),
  commitCount: integer('commit_count').default(0),
  issueCount: integer('issue_count').default(0),

  // Recursion (hierarchical)
  depth: integer('depth').default(0),                    // 0 = initial, 1+ = dig from parent
  parentTraceId: text('parent_trace_id'),                // Links to parent trace

  // Linked list (horizontal chain)
  prevTraceId: text('prev_trace_id'),                    // ← Previous trace in chain
  nextTraceId: text('next_trace_id'),                    // → Next trace in chain

  // Context
  project: text('project'),                              // ghq format
  scope: text('scope').default('project'),               // 'project' | 'cross-project' | 'human'
  sessionId: text('session_id'),
  agentCount: integer('agent_count').default(1),
  durationMs: integer('duration_ms'),

  // Distillation
  status: text('status').default('raw'),                 // 'raw' | 'reviewed' | 'distilling' | 'distilled'
  awakening: text('awakening'),                          // Extracted insight (markdown)
  distilledToId: text('distilled_to_id'),                // Learning ID if promoted
  distilledAt: integer('distilled_at'),

  // Timestamps
  createdAt: integer('created_at').notNull(),
  updatedAt: integer('updated_at').notNull(),
});

// Indexes for efficient querying
index('idx_trace_query').on(table.query),
index('idx_trace_project').on(table.project),
index('idx_trace_status').on(table.status),
index('idx_trace_parent').on(table.parentTraceId),
index('idx_trace_prev').on(table.prevTraceId),
index('idx_trace_next').on(table.nextTraceId),
index('idx_trace_created').on(table.createdAt),
```

#### `supersedeLog` (Audit Trail)

```typescript
export const supersedeLog = sqliteTable('supersede_log', {
  id: integer('id').primaryKey({ autoIncrement: true }),

  // What was superseded
  oldPath: text('old_path').notNull(),                   // Original file path (may no longer exist)
  oldId: text('old_id'),                                 // Document ID if indexed
  oldTitle: text('old_title'),
  oldType: text('old_type'),                             // learning, principle, retro, etc.

  // What replaced it
  newPath: text('new_path'),
  newId: text('new_id'),
  newTitle: text('new_title'),

  // Why and when
  reason: text('reason'),
  supersededAt: integer('superseded_at').notNull(),
  supersededBy: text('superseded_by'),                   // Who made decision (user, claude, indexer)

  // Context
  project: text('project'),                              // ghq format
});

index('idx_supersede_old_path').on(table.oldPath),
index('idx_supersede_new_path').on(table.newPath),
index('idx_supersede_created').on(table.supersededAt),
index('idx_supersede_project').on(table.project),
```

#### `schedule` (Appointments - Per-Human, Shared)

```typescript
export const schedule = sqliteTable('schedule', {
  id: integer('id').primaryKey({ autoIncrement: true }),
  date: text('date').notNull(),                          // YYYY-MM-DD (canonical)
  dateRaw: text('date_raw'),                             // Original input ("5 Mar", "28 ก.พ.")
  time: text('time'),                                    // HH:MM or "TBD"
  event: text('event').notNull(),
  notes: text('notes'),
  recurring: text('recurring'),                          // null | "daily" | "weekly" | "monthly"
  status: text('status').default('pending'),             // pending | done | cancelled
  createdAt: integer('created_at').notNull(),
  updatedAt: integer('updated_at').notNull(),
});

index('idx_schedule_date').on(table.date),
index('idx_schedule_status').on(table.status),
```

---

## 3. Implementation Code

### 3.1 Search Handler (Hybrid FTS5 + Vector)

```typescript
export function sanitizeFtsQuery(query: string): string {
  let sanitized = query
    .replace(/[?*+\-()^~"':.\/]/g, ' ')
    .replace(/\s+/g, ' ')
    .trim();

  if (!sanitized) {
    console.error('[FTS5] Query became empty after sanitization:', query);
    return query;
  }

  return sanitized;
}

export function normalizeFtsScore(rank: number): number {
  // FTS5 rank is negative, lower = better match
  // Converts to 0-1 scale where higher = better
  const absRank = Math.abs(rank);
  return Math.exp(-0.3 * absRank);
}

export async function vectorSearch(
  ctx: ToolContext,
  query: string,
  type: string,
  limit: number,
  model?: string
): Promise<Array<{
  id: string;
  type: string;
  content: string;
  source_file: string;
  concepts: string[];
  score: number;
  distance: number;
  model: string;
  source: 'vector';
}>> {
  try {
    const whereFilter = type !== 'all' ? { type } : undefined;
    const store = model ? await ensureVectorStoreConnected(model) : ctx.vectorStore;

    const results = await store.query(query, limit, whereFilter);

    if (!results.ids || results.ids.length === 0) {
      return [];
    }

    const resolvedModelName = model || 'bge-m3';
    const mappedResults: Array<{...}> = [];

    for (let i = 0; i < results.ids.length; i++) {
      const metadata = results.metadatas[i] as Record<string, unknown> | null;
      const rawDistance = results.distances[i] || 0;

      mappedResults.push({
        id: results.ids[i],
        type: (metadata?.type as string) || 'unknown',
        content: (results.documents[i] || '').substring(0, 500),
        source_file: (metadata?.source_file as string) || '',
        concepts: parseConceptsFromMetadata(metadata?.concepts),
        score: rawDistance,
        distance: rawDistance,
        model: resolvedModelName,
        source: 'vector',
      });
    }

    return mappedResults;
  } catch (error) {
    const errorMsg = error instanceof Error ? error.stack || error.message : String(error);
    console.error('[ChromaDB ERROR]', errorMsg);
    return [];
  }
}

export function combineResults(
  ftsResults: Array<{...}>,
  vectorResults: Array<{...}>,
  ftsWeight: number = 0.5,
  vectorWeight: number = 0.5
): Array<{...}> {
  const resultMap = new Map<string, {...}>();

  // Add FTS results
  for (const result of ftsResults) {
    resultMap.set(result.id, {
      id: result.id,
      type: result.type,
      content: result.content,
      source_file: result.source_file,
      concepts: result.concepts,
      ftsScore: result.score,
      source: 'fts',
    });
  }

  // Add/merge vector results
  for (const result of vectorResults) {
    const existing = resultMap.get(result.id);
    if (existing) {
      existing.vectorScore = result.score;
      existing.source = 'hybrid';
      existing.distance = result.distance;
      existing.model = result.model;
    } else {
      resultMap.set(result.id, {
        id: result.id,
        type: result.type,
        content: result.content,
        source_file: result.source_file,
        concepts: result.concepts,
        vectorScore: result.score,
        distance: result.distance,
        model: result.model,
        source: 'vector',
      });
    }
  }

  // Calculate hybrid scores
  const combined = Array.from(resultMap.values()).map((result) => {
    let score: number;

    if (result.source === 'hybrid') {
      const fts = result.ftsScore ?? 0;
      const vec = result.vectorScore ?? 0;
      // 10% boost for matches in both sources
      score = ((ftsWeight * fts) + (vectorWeight * vec)) * 1.1;
    } else if (result.source === 'fts') {
      score = (result.ftsScore ?? 0) * ftsWeight;
    } else {
      score = (result.vectorScore ?? 0) * vectorWeight;
    }

    return {
      id: result.id,
      type: result.type,
      content: result.content,
      source_file: result.source_file,
      concepts: result.concepts,
      score,
      source: result.source,
      ftsScore: result.ftsScore,
      vectorScore: result.vectorScore,
      distance: result.distance,
      model: result.model,
    };
  });

  return combined;
}
```

---

### 3.2 Learn Handler (Create Knowledge Entry)

```typescript
export function coerceConcepts(concepts: unknown): string[] {
  if (Array.isArray(concepts)) return concepts.map(String);
  if (typeof concepts === 'string') return concepts.split(',').map(s => s.trim()).filter(Boolean);
  return [];
}

export function normalizeProject(input?: string): string | null {
  if (!input) return null;

  // Already normalized
  if (input.match(/^github\.com\/[^\/]+\/[^\/]+$/)) {
    return input.toLowerCase();
  }

  // GitHub URL
  const urlMatch = input.match(/https?:\/\/github\.com\/([^\/]+\/[^\/]+)/);
  if (urlMatch) return `github.com/${urlMatch[1].replace(/\.git$/, '')}`.toLowerCase();

  // Local path with github.com
  const pathMatch = input.match(/github\.com\/([^\/]+\/[^\/]+)/);
  if (pathMatch) return `github.com/${pathMatch[1]}`.toLowerCase();

  // Short format: owner/repo
  const shortMatch = input.match(/^([^\/\s]+\/[^\/\s]+)$/);
  if (shortMatch) return `github.com/${shortMatch[1]}`.toLowerCase();

  return null;
}

export async function handleLearn(ctx: ToolContext, input: OracleLearnInput): Promise<ToolResponse> {
  const { pattern, source, concepts, project: projectInput } = input;
  const now = new Date();
  const dateStr = now.toISOString().split('T')[0];

  const slug = pattern
    .substring(0, 50)
    .toLowerCase()
    .replace(/[^a-z0-9\s-]/g, '')
    .replace(/\s+/g, '-')
    .replace(/-+/g, '-')
    .replace(/^-|-$/g, '');

  const filename = `${dateStr}_${slug}.md`;

  // Resolve vault root for central writes
  const vault = getVaultPsiRoot();
  if ('needsInit' in vault) console.error(`[Vault] ${vault.hint}`);
  const vaultRoot = 'path' in vault ? vault.path : null;

  const project = normalizeProject(projectInput)
    || extractProjectFromSource(source)
    || detectProject(ctx.repoRoot);
  const projectDir = (project || '_universal').toLowerCase();

  let filePath: string;
  let sourceFileRel: string;
  if (vaultRoot) {
    const dir = path.join(vaultRoot, projectDir, 'ψ', 'memory', 'learnings');
    fs.mkdirSync(dir, { recursive: true });
    filePath = path.join(dir, filename);
    sourceFileRel = `${projectDir}/ψ/memory/learnings/${filename}`;
  } else {
    const dir = path.join(ctx.repoRoot, 'ψ/memory/learnings');
    fs.mkdirSync(dir, { recursive: true });
    filePath = path.join(dir, filename);
    sourceFileRel = `ψ/memory/learnings/${filename}`;
  }

  if (fs.existsSync(filePath)) {
    throw new Error(`File already exists: ${filename}`);
  }

  const title = pattern.split('\n')[0].substring(0, 80);
  const conceptsList = coerceConcepts(concepts);
  const frontmatter = [
    '---',
    `title: ${title}`,
    conceptsList.length > 0 ? `tags: [${conceptsList.join(', ')}]` : 'tags: []',
    `created: ${dateStr}`,
    `source: ${source || 'Oracle Learn'}`,
    ...(project ? [`project: ${project}`] : []),
    '---',
    '',
    `# ${title}`,
    '',
    pattern,
    '',
    '---',
    '*Added via Oracle Learn*',
    ''
  ].join('\n');

  fs.writeFileSync(filePath, frontmatter, 'utf-8');

  const id = `learning_${dateStr}_${slug}`;

  // Insert into document index
  ctx.db.insert(oracleDocuments).values({
    id,
    type: 'learning',
    sourceFile: sourceFileRel,
    concepts: JSON.stringify(conceptsList),
    createdAt: now.getTime(),
    updatedAt: now.getTime(),
    indexedAt: now.getTime(),
    origin: null,
    project,
    createdBy: 'oracle_learn',
  }).run();

  // Index in FTS5
  ctx.sqlite.prepare(`
    INSERT INTO oracle_fts (id, content, concepts)
    VALUES (?, ?, ?)
  `).run(id, frontmatter, conceptsList.join(' '));

  return {
    content: [{
      type: 'text',
      text: JSON.stringify({
        success: true,
        file: sourceFileRel,
        id,
        message: `Pattern added to Oracle knowledge base${vaultRoot ? ' (vault)' : ''}`
      }, null, 2)
    }]
  };
}
```

---

### 3.3 Reflect Handler (Random Wisdom)

```typescript
export async function handleReflect(ctx: ToolContext, _input: OracleReflectInput): Promise<ToolResponse> {
  const randomDoc = ctx.db.select({
    id: oracleDocuments.id,
    type: oracleDocuments.type,
    sourceFile: oracleDocuments.sourceFile,
    concepts: oracleDocuments.concepts,
  })
    .from(oracleDocuments)
    .where(inArray(oracleDocuments.type, ['principle', 'learning']))
    .orderBy(sql`RANDOM()`)
    .limit(1)
    .get();

  if (!randomDoc) {
    throw new Error('No documents found in Oracle knowledge base');
  }

  const content = ctx.sqlite.prepare(`
    SELECT content FROM oracle_fts WHERE id = ?
  `).get(randomDoc.id) as { content: string } | undefined;

  if (!content) {
    throw new Error('Document content not found in FTS index');
  }

  return {
    content: [{
      type: 'text',
      text: JSON.stringify({
        principle: {
          id: randomDoc.id,
          type: randomDoc.type,
          content: content.content,
          source_file: randomDoc.sourceFile,
          concepts: JSON.parse(randomDoc.concepts || '[]')
        }
      }, null, 2)
    }]
  };
}
```

---

### 3.4 Concepts Handler (List Tags with Counts)

```typescript
export async function handleConcepts(ctx: ToolContext, input: OracleConceptsInput): Promise<ToolResponse> {
  const { limit = 50, type = 'all' } = input;

  const baseCondition = and(isNotNull(oracleDocuments.concepts), ne(oracleDocuments.concepts, '[]'));
  const rows = type === 'all'
    ? ctx.db.select({ concepts: oracleDocuments.concepts }).from(oracleDocuments).where(baseCondition).all()
    : ctx.db.select({ concepts: oracleDocuments.concepts })
        .from(oracleDocuments)
        .where(and(baseCondition, eq(oracleDocuments.type, type)))
        .all();

  const conceptCounts = new Map<string, number>();
  for (const row of rows as Array<{ concepts: string }>) {
    try {
      const concepts = JSON.parse(row.concepts);
      if (Array.isArray(concepts)) {
        for (const concept of concepts) {
          if (typeof concept === 'string') {
            conceptCounts.set(concept, (conceptCounts.get(concept) || 0) + 1);
          }
        }
      }
    } catch {
      if (typeof row.concepts === 'string') {
        const concepts = row.concepts.split(',').map(c => c.trim()).filter(Boolean);
        for (const concept of concepts) {
          conceptCounts.set(concept, (conceptCounts.get(concept) || 0) + 1);
        }
      }
    }
  }

  const sortedConcepts = Array.from(conceptCounts.entries())
    .map(([name, count]) => ({ name, count }))
    .sort((a, b) => b.count - a.count)
    .slice(0, limit);

  return {
    content: [{
      type: 'text',
      text: JSON.stringify({
        concepts: sortedConcepts,
        total_unique: conceptCounts.size,
        filter_type: type,
      }, null, 2)
    }]
  };
}
```

---

### 3.5 Supersede Handler (Mark Document Outdated)

```typescript
export async function handleSupersede(ctx: ToolContext, input: OracleSupersededInput): Promise<ToolResponse> {
  const { oldId, newId, reason } = input;
  const now = Date.now();

  const oldDoc = ctx.db.select({ id: oracleDocuments.id, type: oracleDocuments.type })
    .from(oracleDocuments)
    .where(eq(oracleDocuments.id, oldId))
    .get();
  const newDoc = ctx.db.select({ id: oracleDocuments.id, type: oracleDocuments.type })
    .from(oracleDocuments)
    .where(eq(oracleDocuments.id, newId))
    .get();

  if (!oldDoc) throw new Error(`Old document not found: ${oldId}`);
  if (!newDoc) throw new Error(`New document not found: ${newId}`);

  // Update old document to mark as superseded
  ctx.db.update(oracleDocuments)
    .set({
      supersededBy: newId,
      supersededAt: now,
      supersededReason: reason || null,
    })
    .where(eq(oracleDocuments.id, oldId))
    .run();

  console.error(`[MCP:SUPERSEDE] ${oldId} → superseded by → ${newId}`);

  return {
    content: [{
      type: 'text',
      text: JSON.stringify({
        success: true,
        old_id: oldId,
        old_type: oldDoc.type,
        new_id: newId,
        new_type: newDoc.type,
        reason: reason || null,
        superseded_at: new Date(now).toISOString(),
        message: `"${oldId}" is now marked as superseded by "${newId}". It will still appear in searches with a warning.`
      }, null, 2)
    }]
  };
}
```

---

### 3.6 Trace Handler (Create Log with Dig Points)

```typescript
function isLearningFilePath(learning: string): boolean {
  return learning.startsWith('ψ/') || learning.includes('/memory/learnings/');
}

function createLearningFile(
  text: string,
  project: string | null,
  traceQuery: string
): string {
  const now = new Date();
  const dateStr = now.toISOString().split('T')[0];

  const slug = text
    .slice(0, 50)
    .toLowerCase()
    .replace(/[^a-z0-9]+/g, '-')
    .replace(/^-|-$/g, '');

  const filename = `${dateStr}_trace-${slug}.md`;
  const relativePath = `ψ/memory/learnings/${filename}`;
  const fullPath = join(REPO_ROOT, relativePath);

  const dir = dirname(fullPath);
  if (!existsSync(dir)) {
    mkdirSync(dir, { recursive: true });
  }

  const content = `---
title: ${text.slice(0, 80)}
tags: [trace-learning${project ? `, ${project.split('/').pop()}` : ''}]
created: ${dateStr}
source: Trace discovery
project: ${project || 'unknown'}
trace_query: "${traceQuery.replace(/"/g, '\\"')}"
---

# ${text.slice(0, 80)}

${text}

---
*Auto-generated from trace: "${traceQuery}"*
${project ? `*Source project: ${project}*` : ''}
`;

  writeFileSync(fullPath, content, 'utf-8');
  return relativePath;
}

export function createTrace(input: CreateTraceInput): CreateTraceResult {
  const traceId = randomUUID();
  const now = Date.now();

  // Process learnings - convert text snippets to file paths
  const processedLearnings = (input.foundLearnings || []).map(learning => {
    if (isLearningFilePath(learning)) {
      return learning;
    }
    return createLearningFile(learning, input.project || null, input.query);
  });

  // Calculate counts
  const fileCount =
    (input.foundFiles?.length || 0) +
    (input.foundRetrospectives?.length || 0) +
    (processedLearnings?.length || 0) +
    (input.foundResonance?.length || 0);
  const commitCount = input.foundCommits?.length || 0;
  const issueCount = input.foundIssues?.length || 0;

  // Determine depth from parent
  let depth = 0;
  if (input.parentTraceId) {
    const parent = db
      .select({ depth: traceLog.depth })
      .from(traceLog)
      .where(eq(traceLog.traceId, input.parentTraceId))
      .get();
    if (parent) depth = (parent.depth || 0) + 1;
  }

  // Insert trace
  db.insert(traceLog).values({
    traceId,
    query: input.query,
    queryType: input.queryType || 'general',
    foundFiles: JSON.stringify(input.foundFiles || []),
    foundCommits: JSON.stringify(input.foundCommits || []),
    foundIssues: JSON.stringify(input.foundIssues || []),
    foundRetrospectives: JSON.stringify(input.foundRetrospectives || []),
    foundLearnings: JSON.stringify(processedLearnings),
    foundResonance: JSON.stringify(input.foundResonance || []),
    fileCount,
    commitCount,
    issueCount,
    depth,
    parentTraceId: input.parentTraceId || null,
    project: input.project || null,
    scope: input.scope || 'project',
    agentCount: input.agentCount || 1,
    durationMs: input.durationMs || 0,
    createdAt: now,
    updatedAt: now,
  }).run();

  return {
    success: true,
    traceId,
    depth,
    summary: {
      fileCount,
      commitCount,
      issueCount,
      totalDigPoints: fileCount + commitCount + issueCount,
    },
  };
}
```

---

### 3.7 Forum Handler (Threaded Discussions)

```typescript
export function createThread(
  title: string,
  createdBy: string = 'user',
  project?: string
): ForumThread {
  const now = Date.now();

  const result = db.insert(forumThreads).values({
    title,
    createdBy,
    status: 'active',
    project: project || null,
    createdAt: now,
    updatedAt: now,
  }).returning({ id: forumThreads.id }).get();

  return {
    id: result.id,
    title,
    createdBy,
    status: 'active',
    project,
    createdAt: now,
    updatedAt: now,
  };
}

export function listThreads(options: {
  status?: ThreadStatus;
  project?: string;
  limit?: number;
  offset?: number;
} = {}): { threads: ForumThread[]; total: number } {
  const { status, project, limit = 20, offset = 0 } = options;

  const conditions = [];
  if (status) {
    conditions.push(eq(forumThreads.status, status));
  }
  if (project) {
    conditions.push(eq(forumThreads.project, project));
  }

  const whereClause = conditions.length > 0 ? and(...conditions) : undefined;

  // Get count
  const countResult = db.select({ count: sql<number>`count(*)` })
    .from(forumThreads)
    .where(whereClause)
    .get();
  const total = countResult?.count || 0;

  // Get threads
  const rows = db.select()
    .from(forumThreads)
    .where(whereClause)
    .orderBy(desc(forumThreads.updatedAt))
    .limit(limit)
    .offset(offset)
    .all();

  return {
    threads: rows.map(row => ({
      id: row.id,
      title: row.title,
      createdBy: row.createdBy || 'unknown',
      status: (row.status || 'active') as ThreadStatus,
      issueUrl: row.issueUrl || undefined,
      issueNumber: row.issueNumber || undefined,
      project: row.project || undefined,
      createdAt: row.createdAt,
      updatedAt: row.updatedAt,
      syncedAt: row.syncedAt || undefined,
    })),
    total,
  };
}

export function updateThreadStatus(threadId: number, status: ThreadStatus): void {
  db.update(forumThreads)
    .set({ status, updatedAt: Date.now() })
    .where(eq(forumThreads.id, threadId))
    .run();
}
```

---

## 4. Key Architecture Patterns

### 4.1 "Nothing is Deleted" Principle

The `oracle_documents` table implements an audit trail via `supersededBy`, `supersededAt`, and `supersededReason` fields. Old documents are never deleted but marked as superseded. This allows:
- Full history preservation
- Detecting outdated information
- Reverting to older versions if needed

### 4.2 Hybrid Search (FTS5 + Vector)

**FTS5**: Full-text search on sanitized queries, fast for keyword matching
**Vector Search**: Semantic similarity via ChromaDB (supports multiple embedding models)
**Hybrid Score**: `(fts_score * 0.5 + vector_score * 0.5) * 1.1` (10% boost for docs in both)

### 4.3 Project Detection & Normalization

Uses ghq-style paths: `github.com/owner/repo`
- Normalizes input: short form `owner/repo` → `github.com/owner/repo`
- Extracts from URLs, local paths, or source attribution
- Enables per-project scoping and provenance tracking

### 4.4 Trace Log System (Hierarchical + Linked)

**Hierarchy**: `parentTraceId` → depth tracking for "digs from a parent"
**Linked Chain**: `prevTraceId` ↔ `nextTraceId` → horizontal navigation
**Distillation**: `status` (raw → reviewed → distilling → distilled) + `distilledToId` → promote traces to learnings

### 4.5 Forum Integration

**Threaded**: Forum threads link to GitHub issues (if synced)
**Messages**: Track role (human/oracle/claude), search context, principles/patterns found
**Oracle Response**: Auto-respond from knowledge base search results

---

## 5. Vector Store Integration

Vector search uses `ChromaDB` with pluggable embedding models:
- **bge-m3** (default): 1024-dim, multilingual Thai↔EN
- **nomic**: 768-dim, fast inference
- **qwen3**: 4096-dim, cross-language support

Collection: `oracle_knowledge`
Metadata filters: `type` (principle/pattern/learning/retro)

---

## 6. Context Object (ToolContext)

Passed to all tool handlers:

```typescript
export interface ToolContext {
  db: BunSQLiteDatabase<typeof schema>;           // Drizzle ORM instance
  sqlite: Database;                               // Raw Bun SQLite for FTS5
  repoRoot: string;                               // Project root (where ψ/ lives)
  vectorStore: VectorStoreAdapter;                // ChromaDB client
  vectorStatus: 'unknown' | 'connected' | 'unavailable';
  version: string;                                // Oracle version
}
```

---

## 7. Entry Points & Handlers

| Tool | Handler | Purpose |
|------|---------|---------|
| `oracle_search` | `handleSearch` | Hybrid FTS5+vector search |
| `oracle_learn` | `handleLearn` | Create knowledge entry |
| `oracle_reflect` | `handleReflect` | Random wisdom |
| `oracle_concepts` | `handleConcepts` | List tags with counts |
| `oracle_supersede` | `handleSupersede` | Mark document outdated |
| `oracle_trace` | `handleTrace` | Log trace session |
| `oracle_thread` | `handleThread` | Send forum message |
| `oracle_threads` | `handleThreads` | List threads |
| `oracle_thread_read` | `handleThreadRead` | Read thread history |
| `oracle_thread_update` | `handleThreadUpdate` | Update thread status |

---

## 8. File Structure (Knowledge Base)

```
ψ/
├── memory/
│   ├── learnings/
│   │   ├── 2026-03-18_git-safety.md
│   │   ├── 2026-03-18_force-push-protocol.md
│   │   └── ...
│   ├── principles/
│   ├── patterns/
│   └── retros/
├── inbox/
│   ├── handoff/
│   │   ├── 2026-03-18_1203_session-context.md
│   │   └── ...
│   └── schedule.md
└── ...
```

---

## 9. Configuration

- **Database**: SQLite at `~/.oracle/oracle.db` (or `ORACLE_DB_PATH`)
- **Repo Root**: Project root with `ψ/` directory (or `ORACLE_REPO_ROOT`)
- **Port**: 47778 (or `ORACLE_PORT`)
- **FTS5 Virtual Table**: `oracle_fts` (id, content, concepts)

---

*Last updated: 2026-03-18*
*Source: oracle-v2 codebase analysis*

