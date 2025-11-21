# Context Window Efficiency (Continued)

## Part 2: Advanced Strategies and Tools

### 8. **Implement Context Windowing for Long Documents**

**When you must work with long documents, use windowing.**

**Pattern:**
```
Human: I need to refactor this 2000-line file

Claude: Let's approach this systematically:
1. First, show me lines 1-100 (imports and setup)
2. Then lines 100-300 (core logic area 1)
3. etc.

This way I can focus on each section without holding the entire file in context.
```

**Token savings: 90%+ during the analysis phase**

### 9. **Use Directory Structures as Context**

**Instead of showing file contents, show structure.**

❌ **Inefficient:**
```typescript
// Here's every file in the project
[50 files of code]
```

✅ **Efficient:**
```
src/
├── auth/           # Authentication & authorization
│   ├── authService.ts     # Main auth logic
│   ├── tokenManager.ts    # JWT handling
│   └── middleware.ts      # Auth middleware
├── users/          # User management
│   ├── userService.ts
│   └── userRepository.ts
└── payments/       # Payment processing
    ├── paymentService.ts
    └── stripeAdapter.ts
```

**Then:** "I need to modify payment processing" → Load only payments/ files

**Token savings: 95%+ for initial orientation**

### 10. **Implement Progressive Disclosure**

**Start high-level, drill down only where needed.**

**Example workflow:**
```
Stage 1: Architecture overview (500 tokens)
  ↓ Need more detail on auth
Stage 2: Auth module overview (800 tokens)
  ↓ Need to modify token validation
Stage 3: Token validation implementation (2K tokens)
  ↓ Make the change
Stage 4: Related test file (1.5K tokens)

Total: 4.8K tokens
vs loading everything upfront: 50K+ tokens
```

## ❌ DON'T: Context Waste Patterns

### 1. **Don't Dump Entire Codebases**

**Problem:** "Here's my entire project, help me understand it"

**Why it fails:**
- Claude can't effectively process 100K+ tokens at once
- Important details get lost in volume
- Quality of analysis degrades
- Most of that context is irrelevant to any single question

**Better:** "Here's my project structure [500 tokens]. I'm working on authentication. What auth-related files should I show you?"

### 2. **Don't Include Build Artifacts or Generated Code**

**Problem:** Including dist/, node_modules/, or generated files

**Why it fails:**
- Wastes massive token budget on irrelevant code
- Generated code rarely needs AI analysis
- Build artifacts change constantly
- Provides no useful information

**Examples to exclude:**
- `dist/`, `build/`, `.next/`, etc.
- `node_modules/`, `vendor/`
- Compiled binaries
- Generated TypeScript definitions from libraries
- Bundled/minified code
- Lock files (package-lock.json, yarn.lock)

**Exception:** Source maps or specific generated files you're debugging

### 3. **Don't Repeat Information Across Messages**

**Problem:** Re-stating the same context in every message

**Example of waste:**
```
Message 1: "I'm building a React app with TypeScript..."
Message 2: "In my React/TypeScript app, I need to..."
Message 3: "For this React/TS project, how do I..."
```

**Why it fails:**
- Context is already in conversation history
- Wastes tokens repeating known information
- Leaves less room for new information

**Better:** Say it once, then just proceed with assumptions established

### 4. **Don't Keep Dead Conversation Threads**

**Problem:** Never pruning completed discussion threads

**Pattern:**
```
Messages 1-20: Discussed and implemented Feature A
Messages 21-40: Discussed and implemented Feature B
Messages 41-50: Discussed and implemented Feature C
Message 51: "Now let's work on Feature D"
```

**Why it fails:**
- Feature A, B, C discussions are irrelevant to Feature D
- Consuming 30K+ tokens with dead context
- May cause confusion if patterns changed

**Better:** At message 51, start new conversation with summary:
```
"Previous session completed Features A, B, C. Key decisions:
- Using pattern X for state management
- Error handling via Y pattern
- Tests passing

Now working on Feature D."
```

### 5. **Don't Load All Dependencies Documentation**

**Problem:** Including docs for React, TypeScript, Express, etc.

**Why it fails:**
- Claude already knows these frameworks
- Massive token waste (framework docs are huge)
- Your token budget better spent on project-specific code
- Standard docs won't help Claude more than training data

**Exception:** When using obscure libraries or very recent APIs not in training data, brief relevant snippets can help.

### 6. **Don't Mix Active and Archive Context**

**Problem:** Loading both current work and historical reference material

**Example:**
```
- Current feature branch code
- Old deprecated implementation (for reference)
- Historical decision docs
- Previous version of file
- Migration notes from 2 years ago
```

**Why it fails:**
- Claude may confuse old and new patterns
- Historical information rarely relevant to current work
- Archive material better summarized than included verbatim

**Better:** "We used to use Redux, now using Zustand" (summary) vs. including old Redux implementation files

## Measuring Context Efficiency

### Token Budget Tracking

**Manual estimation:**
```bash
# Rough token count (1 token ≈ 4 characters for English text)
wc -c file.ts | awk '{print $1/4 " tokens"}'

# For multiple files
find src/ -name "*.ts" -exec wc -c {} + | awk '{sum+=$1} END {print sum/4 " tokens"}'
```

**More accurate (using tokenizer tools):**
- Install: `npm install -g @anthropic-ai/tokenizer`
- Count: `tokenize-count file.ts`

**Set budget alerts:**
- **Green zone:** <50K tokens in active context
- **Yellow zone:** 50K-100K tokens (start optimizing)
- **Red zone:** >100K tokens (definitely optimize)

### Efficiency Metrics

**Context Efficiency Score:**
```
Efficiency = Useful Information Tokens / Total Context Tokens

Good: >0.7 (70% of context is directly relevant)
Acceptable: 0.4-0.7
Poor: <0.4 (too much noise)
```

**Example calculation:**
- Total context: 80K tokens
- Memory files: 5K tokens (relevant ✓)
- Current code being modified: 10K tokens (relevant ✓)
- Related files: 15K tokens (relevant ✓)
- Conversation history: 50K tokens
  - Relevant discussion: 20K tokens (relevant ✓)
  - Completed task discussion: 30K tokens (not relevant ✗)

**Useful tokens: 50K / Total: 80K = 0.625 efficiency**

**Action:** Prune the 30K tokens of completed task discussion → New efficiency: 50K/50K = 1.0

### Performance Indicators

**Signs of good context efficiency:**
- Claude finds relevant information quickly
- Responses are focused and accurate
- Minimal "I need to see..." requests
- Fast response times
- Consistent quality throughout conversation

**Signs of poor context efficiency:**
- Claude asks for information you already provided
- Responses become vaguer as conversation progresses
- Long response times
- Claude mixing up different parts of the codebase
- Quality degrades over time

## Context-Efficient Workflows

### Workflow 1: Feature Implementation

**Phase 1 - Planning (Minimal Context):**
```
Context load: CLAUDE.md + architecture overview + feature requirements
Tokens: ~3K

Task: Plan the implementation approach
```

**Phase 2 - Implementation (Focused Context):**
```
Context load: Relevant source files + tests + dependencies
Tokens: ~15K

Task: Implement feature incrementally
Strategy: One file/component at a time
```

**Phase 3 - Testing (Test Context):**
```
Context load: Implementation + test files + test utilities
Tokens: ~10K

Task: Write and verify tests
```

**Phase 4 - Review (Fresh Context):**
```
New conversation!
Context load: Summary of changes + final diff
Tokens: ~5K

Task: Review for issues and improvements
```

**Total tokens across phases: ~33K vs. loading everything at once: 100K+**

### Workflow 2: Bug Fixing

**Phase 1 - Reproduction (Minimal Context):**
```
Context: Bug report + high-level architecture
Tokens: ~2K

Task: Identify likely problem area
```

**Phase 2 - Investigation (Targeted Context):**
```
Context: Suspect files + recent changes + related tests
Tokens: ~8K

Task: Pinpoint the bug
```

**Phase 3 - Fix (Focused Context):**
```
Context: File with bug + tests + dependencies
Tokens: ~5K

Task: Implement and verify fix
```

**Total: ~15K tokens - highly efficient for bug fixes**

### Workflow 3: Refactoring

**Phase 1 - Analysis (Structural Context):**
```
Context: Directory structure + file summaries + architecture docs
Tokens: ~5K

Task: Plan refactoring strategy
```

**Phase 2 - Execution (Incremental Context):**
```
For each file/module:
  Context: Current file + its tests + immediate dependencies
  Tokens: ~8K per iteration

  Task: Refactor and verify

  Then move to next file (previous file's context can be dropped)
```

**Phase 3 - Integration (Integration Context):**
```
Context: Changed files + integration tests
Tokens: ~12K

Task: Ensure everything works together
```

**Total: Much lower than loading entire refactoring scope upfront**

## Tool-Specific Optimizations

### Claude.ai Web Interface

**Best practices:**
- Use Projects feature to maintain persistent context
- Start new chats for new major tasks
- Use "Add context" feature selectively, not dumping entire folders
- Leverage built-in memory for preferences
- Export important conversations for future reference

**Context budget: ~200K tokens**
- Reserve 50K for conversation
- Reserve 50K for output
- Leaves 100K for code and docs
- Be selective!

### Claude Code (CLI)

**Best practices:**
- Configure `.claudeignore` to exclude unnecessary files
- Use focused prompts that request specific files
- Leverage file tree exploration rather than loading everything
- Use CLAUDE.md effectively (automatically loaded)
- Start new sessions for new tasks

**Example `.claudeignore`:**
```
node_modules/
dist/
build/
.next/
coverage/
*.log
*.lock
.git/
.env
```

### API Usage (Direct Anthropic API)

**Best practices:**
- Implement your own context management
- Cache stable context (system prompts, CLAUDE.md)
- Use prompt caching for large, stable contexts
- Measure and log token usage
- Implement conversation summarization
- Roll off old messages programmatically

**Prompt caching example:**
```python
# Cache expensive, stable context
response = anthropic.messages.create(
    model="claude-sonnet-4-5-20250929",
    system=[
        {
            "type": "text",
            "text": project_context,  # Large, stable context
            "cache_control": {"type": "ephemeral"}
        }
    ],
    messages=conversation_history
)
```

**Benefit:** Cached content is reused across requests, saving costs and latency

### IDEs with LLM Integration (Cursor, etc.)

**Best practices:**
- Configure relevant file patterns, not entire workspace
- Use context-aware features (Cursor's @files, @code, etc.)
- Regularly clear irrelevant context
- Use focused prompts referencing specific files
- Leverage IDE's code intelligence to reduce context needs

## Advanced Context Techniques

### 1. Context Partitioning

**Divide project into independent context partitions.**

**Example:**
```
Partition 1: Frontend
- React components
- Styles
- Frontend state management
- Relevant: When working on UI

Partition 2: Backend API
- Express routes
- Controllers
- Services
- Relevant: When working on API

Partition 3: Database
- Schema
- Migrations
- Repositories
- Relevant: When working on data layer

Partition 4: Infrastructure
- Docker config
- CI/CD
- Deployment scripts
- Relevant: When working on DevOps
```

**Strategy:** Load only relevant partition(s) for each task

**Benefit:** 75% context reduction by loading 1 partition vs. all 4

### 2. Context Inheritance

**Build context hierarchically.**

**Base layer (Always loaded):**
- Project overview
- Tech stack
- Core conventions

**Feature layers (Loaded as needed):**
- Auth layer: Base + auth-specific context
- Payments layer: Base + payments-specific context
- Admin layer: Base + admin-specific context

**Benefit:** Shared base context + specialized context only when needed

### 3. Context Compression

**Techniques to compress information:**

**Technique 1 - Symbolic Representation:**
Instead of:
```typescript
export function getUserById(id: string): Promise<User> {
  return db.users.findUnique({ where: { id } })
}
// ... 20 more similar functions
```

Use:
```typescript
// UserService exports:
// - CRUD operations: get, create, update, delete
// - Search: findByEmail, findByRole
// - Bulk operations: bulkCreate, bulkUpdate
// All return Promise<User> or Promise<User[]>
// All use Prisma for DB access
```

**Technique 2 - Pattern Templates:**
Instead of showing 10 similar components, show pattern:
```typescript
// Pattern: All form components follow this structure
// 1. useForm() hook for form state
// 2. zod schema for validation
// 3. onSubmit handler with error handling
// 4. Loading/error UI states
// See: LoginForm.tsx as reference implementation
```

**Technique 3 - Difference Encoding:**
```typescript
// PaymentService is similar to UserService but:
// - Uses Stripe API instead of direct DB
// - Includes webhook handling
// - Has retry logic for failed payments
// - Requires idempotency keys
```

**Token savings: 60-80% vs. full code**

### 4. Dynamic Context Loading

**Implement a context manager.**

**Conceptual pattern:**
```
ContextManager:
  - Maintains priority queue of context
  - Loads high-priority first
  - Drops low-priority when budget tight
  - Tracks what's in context
  - Suggests what to load next

Priority tiers:
  1. Critical: Current task files
  2. High: Direct dependencies
  3. Medium: Related files
  4. Low: Background/reference
  5. Archive: Historical
```

**Example decision tree:**
```
Task: Fix bug in authentication

Load Critical:
  ✓ auth.ts (buggy file)
  ✓ auth.test.ts (failing test)

Load High:
  ✓ types/auth.ts (types used by auth.ts)
  ✓ middleware/auth.ts (calls auth.ts)

Load Medium (if budget allows):
  ? config/auth.ts (configuration)
  ? utils/jwt.ts (utility)

Skip Low:
  ✗ docs/auth.md (reference)
  ✗ old-auth.ts (deprecated)

Skip Archive:
  ✗ migration-notes.md (historical)
```

## Context Efficiency Checklist

Before starting a conversation, ask:

### Planning Phase
- [ ] What is the specific task?
- [ ] What files are directly relevant?
- [ ] What background context is truly needed?
- [ ] Can I summarize instead of including full text?
- [ ] Is there outdated context I can remove?

### During Conversation
- [ ] Am I including unnecessary files?
- [ ] Is conversation history growing too large?
- [ ] Should I summarize and start fresh?
- [ ] Am I re-stating known context?
- [ ] Is Claude showing signs of context overload?

### After Conversation
- [ ] What decisions should go into memory files?
- [ ] Should I create a handoff document?
- [ ] What context can be discarded?
- [ ] Did I learn any efficiency lessons?

## Common Context Patterns

### Pattern: The Focused Session

**When:** Working on a specific, well-defined task
**Context load:** Minimal, task-specific only
**Typical tokens:** 10K-30K
**Best for:** Bug fixes, small features, isolated refactoring

### Pattern: The Exploratory Session

**When:** Learning about unfamiliar codebase
**Context load:** Progressive, loading as you discover
**Typical tokens:** Start at 5K, grow to 40K as you explore
**Best for:** Onboarding, architecture analysis, planning

### Pattern: The Implementation Session

**When:** Building a new feature
**Context load:** Medium, feature-scoped
**Typical tokens:** 30K-60K (architecture + relevant code)
**Best for:** Feature development, multi-file changes

### Pattern: The Review Session

**When:** Reviewing code or testing
**Context load:** Fresh start with changes only
**Typical tokens:** 10K-20K
**Best for:** Code review, testing, final validation

### Pattern: The Deep Dive Session

**When:** Complex problem-solving or large refactoring
**Context load:** Heavy but partitioned
**Typical tokens:** 60K-100K, but loaded in phases
**Best for:** Architecture changes, complex debugging, large refactors

## Conclusion

Context window efficiency is about **quality over quantity**:

### Key Principles
1. ✅ **Relevance:** Only load what matters for the current task
2. ✅ **Summarization:** Compress when possible, expand when necessary
3. ✅ **Just-in-Time:** Load as needed, not preemptively
4. ✅ **Forgetting:** Deliberately prune completed context
5. ✅ **Measurement:** Track and optimize token usage

### Quick Wins
- Start conversations with clear, focused goals
- Use file references instead of dumping entire codebases
- Summarize completed work rather than keeping full history
- Start fresh sessions for new major tasks
- Configure ignore patterns to exclude irrelevant files

### Advanced Strategies
- Implement context partitioning for large projects
- Use progressive disclosure patterns
- Create handoff documents for session transitions
- Measure and optimize your context efficiency
- Develop project-specific context loading strategies

**The Golden Rule:** If Claude doesn't need it to complete the current task, don't include it. Every token in context should earn its place by being directly relevant and useful.

**Remember:** A 20K token context that's 100% relevant will outperform a 150K token context that's 30% relevant. Focus beats volume every time.
