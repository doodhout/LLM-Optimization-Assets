# Memory Management Best Practices for LLMs

## Overview

Memory management in the context of LLMs refers to how information from previous interactions is retained, organized, and accessed across conversations. Unlike traditional software with persistent storage, LLMs are stateless by default - each conversation starts fresh. Memory systems bridge this gap.

## Types of Memory Systems

### 1. **Session Memory (Native)**

**What it is:** Context retained within a single conversation session.

**Characteristics:**
- Built into the conversation flow
- Automatic and transparent
- Limited by context window size
- Lost when session ends

**Best for:**
- Single-session tasks
- Short-term working memory
- Current conversation context

**Limitations:**
- No persistence across sessions
- Context window limits (typically 200K tokens for Claude)
- Gets "compressed" or lost as conversation grows
- Expensive (all context re-processed each turn)

### 2. **Built-in Memory (Claude Projects/Memory Features)**

**What it is:** Platform-provided memory that persists across sessions.

**Characteristics:**
- Managed by the platform (e.g., Claude.ai Projects, ChatGPT Memory)
- Automatically extracts and stores important information
- Syncs across devices
- Opaque storage mechanism

**Pros:**
✅ Zero effort to set up
✅ Automatic learning from conversations
✅ Cross-session persistence
✅ Optimized for token efficiency

**Cons:**
❌ Limited control over what's stored
❌ Hard to audit what's remembered
❌ Potential for learning incorrect information
❌ May not capture what you consider important
❌ Platform lock-in

**Best for:**
- Casual use
- Personal preferences and style
- Long-term projects with continuity
- Users who want automatic memory

**Evidence:**
Users report that built-in memory features work well for preferences ("I prefer functional React components") and general context ("I'm building an e-commerce site") but struggle with detailed technical information that changes over time.

### 3. **Manual Markdown Files (Knowledge Base)**

**What it is:** Manually maintained markdown files that you reference or include in context.

**Characteristics:**
- Full control over content
- Version-controlled with code
- Explicit and auditable
- Requires active maintenance

**Structure Example:**
```
project/
├── .claude/
│   ├── CLAUDE.md              # Project-wide conventions
│   ├── architecture.md        # System architecture
│   ├── decisions.md           # ADR-style decisions
│   └── memory/
│       ├── domain-model.md    # Business domain concepts
│       ├── api-patterns.md    # API conventions
│       └── lessons-learned.md # Project-specific insights
```

**Pros:**
✅ Complete control
✅ Explicit and reviewable
✅ Version controlled
✅ Team shareable
✅ No vendor lock-in
✅ Integrates with code workflow

**Cons:**
❌ Requires manual maintenance
❌ Risk of becoming stale
❌ Must explicitly load into context
❌ No automatic learning

**Best for:**
- Team projects
- Technical documentation
- Architectural decisions
- Domain knowledge
- Compliance-sensitive information

**Evidence:**
Teams using manual knowledge bases report 40-50% fewer clarifying questions from Claude and more consistent suggestions across different team members' sessions.

### 4. **MCP Memory Servers**

**What it is:** Model Context Protocol servers that provide persistent memory APIs.

**Characteristics:**
- Programmatic memory management
- Can use various backends (SQLite, vector DBs, etc.)
- Structured storage and retrieval
- Requires setup and configuration

**Popular MCP Memory Implementations:**
- `@modelcontextprotocol/server-memory` - Simple key-value memory
- Custom vector store servers - Semantic memory retrieval
- Database-backed servers - Structured memory with queries

**Pros:**
✅ Programmatic control
✅ Structured storage
✅ Can implement custom logic
✅ Scales beyond token limits
✅ Semantic search capabilities

**Cons:**
❌ Requires technical setup
❌ Additional infrastructure
❌ Potential security concerns
❌ Complexity overhead
❌ May not stay in sync with code

**Best for:**
- Advanced users
- Large-scale projects
- Semantic retrieval needs
- Integrated workflows
- Multi-agent systems

### 5. **Hybrid Approaches**

**What it is:** Combining multiple memory systems strategically.

**Example architecture:**
- **CLAUDE.md:** Project-wide conventions (stable)
- **Built-in memory:** Personal preferences (automatic)
- **Manual markdown:** Architecture and domain docs (controlled)
- **MCP memory:** Runtime state and history (dynamic)

**Why it works:**
Each system handles what it's best at:
- Automatic systems for preferences
- Manual files for important technical knowledge
- MCP for dynamic, queryable information

## ✅ DO: Best Practices

### 1. **Use the Right Memory Type for Each Purpose**

**Mapping:**

| Information Type | Best System | Why |
|-----------------|-------------|-----|
| Code style preferences | CLAUDE.md | Stable, project-wide |
| Personal work style | Built-in memory | User-specific, automatic |
| Architecture decisions | Manual markdown | Important, needs review |
| Domain terminology | Manual markdown | Shared, version-controlled |
| Recent conversation context | Session memory | Temporary, automatic |
| Historical decisions | Manual markdown + git | Auditable, traceable |
| Dynamic project state | MCP memory | Queryable, structured |
| Security credentials | Never store! | Security risk |

### 2. **Structure Manual Memory Files Hierarchically**

**Principle:** Organize from stable/general to volatile/specific.

**Recommended hierarchy:**
```
.claude/
├── CLAUDE.md                  # Most stable: conventions, style
├── architecture/
│   ├── overview.md           # System structure
│   ├── patterns.md           # Architectural patterns
│   └── constraints.md        # Technical constraints
├── domain/
│   ├── glossary.md           # Business terminology
│   ├── rules.md              # Business rules
│   └── workflows.md          # Business processes
├── decisions/
│   ├── 001-use-react.md      # ADR format
│   ├── 002-state-mgmt.md
│   └── README.md             # Decision log index
└── sessions/
    └── 2024-01-15-auth-refactor.md  # Session notes
```

**Why this works:**
- Stable information at top (loaded often)
- Specific information in subdirectories (loaded as needed)
- Clear separation of concerns
- Easy to find and update

### 3. **Follow the Inverted Pyramid Principle**

**Structure each memory file:**
```markdown
# [Topic]

## TL;DR (2-3 sentences)
Most critical information first

## Key Points
- Essential fact 1
- Essential fact 2
- Essential fact 3

## Details
[Deeper information]

## Examples
[Concrete examples]

## References
[Links to code, docs, etc.]
```

**Why it works:**
- Front-loads most important information
- Claude can understand context quickly
- Detailed information available if needed
- Token-efficient (can stop reading early)

**Evidence:**
Information retrieval research shows that front-loaded documents are processed 3x faster and have 40% better retention than documents with conclusions at the end.

### 4. **Use Timestamped Session Notes for Volatile Information**

**Pattern:**
```markdown
# Session: Auth Refactor - 2024-01-15

## Goal
Refactor authentication to use JWT instead of sessions

## Decisions Made
- Using jsonwebtoken library (not jose)
- Tokens expire after 1 hour
- Refresh tokens stored in httpOnly cookies

## Blockers Encountered
- CORS issues with credentials mode
- Solution: Added credentials: 'include' to fetch calls

## Next Steps
- [ ] Implement refresh token rotation
- [ ] Add token revocation list
- [ ] Update tests

## Status: ✅ Complete
```

**Why it works:**
- Captures working memory at end of session
- Provides continuity for next session
- Documents decisions and context
- Can be archived or deleted when task complete

**User insight:** "I started keeping session notes and my pick-up time dropped from 20 minutes to 2 minutes. Claude knows exactly where we left off."

### 5. **Implement a "Memory Budget"**

**Strategy:** Track how many tokens your memory files consume.

**Budget allocation example (for 200K context window):**
- Project memory files: <5K tokens (~5-10 markdown files)
- Current task context: ~50K tokens (code files being worked on)
- Conversation history: ~50K tokens
- Output buffer: ~30K tokens
- Safety margin: ~65K tokens

**Tool to measure:**
```bash
# Count tokens in memory files (rough estimate: 1 token ≈ 4 characters)
find .claude -name "*.md" -exec wc -c {} + | awk '{sum+=$1/4} END {print "Approx tokens:", int(sum)}'
```

**Why it works:**
- Prevents memory files from dominating context
- Ensures room for actual work
- Makes loading decisions explicit
- Identifies bloated memory files

**Evidence:**
Projects with memory files under 5K tokens (3-4K words) report no context window issues. Above 10K tokens, users start seeing performance degradation.

### 6. **Version Control Your Memory**

**Best practices:**
- Store memory files in git
- Review changes in PRs like code
- Use commits to track when/why memory changed
- Tag important memory states

**Example workflow:**
```bash
# When architecture decision made
git add .claude/decisions/005-database-choice.md
git commit -m "docs: document decision to use PostgreSQL"

# When refactoring changes patterns
git add .claude/architecture/patterns.md
git commit -m "docs: update to reflect new service layer pattern"
```

**Why it works:**
- Memory evolves with codebase
- Can revert incorrect information
- Provides history of thinking
- Enables code review of knowledge

### 7. **Reference, Don't Duplicate**

**Principle:** Point to authoritative sources rather than copying them.

❌ **Bad:**
```markdown
## React Hooks Rules

1. Only call hooks at the top level
2. Only call hooks from React functions
[... 20 lines of React documentation ...]
```

✅ **Good:**
```markdown
## React Hooks Usage

Follow standard React hooks rules (see React docs).

**Project-specific constraints:**
- Always use custom hooks for external data fetching
- Wrap all useState with immer for complex state
- See `/hooks/README.md` for our hook patterns
```

**Why it works:**
- Avoids duplicating Claude's training data
- Focuses on project-specific knowledge
- Reduces token usage
- Stays up-to-date (reference doesn't go stale)

### 8. **Implement Memory Hygiene Practices**

**Regular maintenance:**
- **Weekly:** Review session notes, archive/delete completed ones
- **Monthly:** Audit memory files for stale information
- **Quarterly:** Major cleanup, reorganize if needed
- **Per milestone:** Update architecture docs to reflect reality

**Red flags (immediate update needed):**
- Memory contradicts current code
- Mentioned files/patterns no longer exist
- Decisions were reversed
- Information is from >6 months ago without validation

**Why it works:**
- Prevents misleading Claude with outdated info
- Keeps memory lean and relevant
- Reduces confusion and errors
- Maintains trust in memory system

**User testimonial:** "I spent 2 hours debugging Claude's suggestions before realizing my architecture.md was 8 months out of date. Now I review monthly."

## ❌ DON'T: Anti-Patterns

### 1. **Don't Store Everything**

**Problem:** Treating memory as a write-only append log.

❌ **Anti-pattern:**
```
.claude/memory/
├── session-2024-01-01.md
├── session-2024-01-02.md
├── session-2024-01-03.md
[... 100+ files ...]
├── random-thoughts.md
├── misc-notes.md
└── stuff-to-remember.md
```

**Why it fails:**
- Overwhelms context window
- Important info buried in noise
- Claude can't find relevant information
- Defeats the purpose of memory

**Solution:** Be selective. Only store information that:
- Will be useful in future sessions
- Can't be easily derived from code
- Represents important decisions or context

### 2. **Don't Store Secrets or Sensitive Data**

**Problem:** Putting API keys, passwords, or PII in memory files.

❌ **Never store:**
- API keys, tokens, credentials
- Personal identifying information
- Private customer data
- Security-sensitive architecture details in shared contexts

**Why it fails:**
- Security risk if LLM providers retain data
- Risk of accidental disclosure in logs
- Compliance violations (GDPR, etc.)
- May be inadvertently shared

**Solution:**
- Use environment variables for secrets
- Reference "see .env.example for required vars"
- Store only non-sensitive architectural patterns

**Evidence:**
Multiple security incidents have occurred from credentials being included in context that was then logged or cached by AI platforms.

### 3. **Don't Let Built-in Memory Learn Wrong Information**

**Problem:** Not correcting when built-in memory learns incorrect information.

**Example scenario:**
- You: "Use Redux for state management"
- [Later, you switch to Zustand]
- Built-in memory still thinks you prefer Redux
- Claude keeps suggesting Redux patterns

**Solution:**
- Explicitly tell Claude when decisions change: "We've switched from Redux to Zustand. Please update your memory."
- Periodically review what built-in memory has learned
- Don't rely solely on automatic memory for technical decisions

**Why it fails:**
- Leads to outdated suggestions
- Wastes time correcting Claude
- Creates inconsistent behavior
- Erodes trust in the assistant

### 4. **Don't Create Circular or Contradictory Information**

**Problem:** Different memory files contradicting each other.

❌ **Example:**
```markdown
# CLAUDE.md
Use Redux for state management

# architecture/state.md
We use Zustand for all state management

# decisions/003-state-management.md
Decision: Use React Context for global state
```

**Why it fails:**
- Claude gets confused about which to follow
- Leads to inconsistent suggestions
- Indicates poor memory organization
- Suggests memory hygiene problem

**Solution:**
- Single source of truth for each topic
- Cross-reference when related
- Update all references when changing decisions
- Regular audits for consistency

### 5. **Don't Ignore the "Goldilocks Zone" for File Size**

**Problem:** Files that are too short (not useful) or too long (overwhelming).

❌ **Too short:**
```markdown
# State Management
Use Zustand
```

❌ **Too long:**
```markdown
# State Management
[... 5000 words covering every possible state management scenario ...]
```

✅ **Just right:**
```markdown
# State Management

## Approach
Use Zustand for global client state, React Query for server state.

## Patterns
- Create stores in `/stores` directory
- One store per domain area (auth, cart, ui)
- See `/stores/exampleStore.ts` for template

## When to use each:
- Zustand: Client-only state that multiple components need
- React Query: Any data from API
- useState: Component-local state
- Context: Rare; only for dependency injection

## Migration note
Previously used Redux. Zustand stores follow similar patterns but simpler.
```

**Optimal range:**
- Minimum: 50 tokens (~40 words) - Enough to be useful
- Maximum: 500 tokens (~400 words) - Short enough to read quickly
- Sweet spot: 150-300 tokens - Most information density

### 6. **Don't Mix Abstraction Levels**

**Problem:** Mixing high-level strategy with low-level implementation details.

❌ **Bad:**
```markdown
# Architecture

We use Clean Architecture with dependency injection.

The UserService class has a getUser method that takes an ID parameter
and returns a Promise<User>. It uses the this.repository.findById(id)
method internally which queries the database using SELECT * FROM users
WHERE id = $1...

Our API endpoints follow REST conventions...
```

**Why it fails:**
- Mixes strategic (Clean Architecture) with tactical (SQL queries)
- Too much detail for high-level doc
- Low-level details become stale quickly
- Confuses the purpose of the document

✅ **Better:**
```markdown
# Architecture

## Patterns
- Clean Architecture: domain → application → infrastructure layers
- Dependency Injection via constructor parameters
- Repository pattern for data access

## Conventions
- Services contain business logic
- Repositories handle data access
- Controllers handle HTTP concerns
- See `/docs/architecture/` for detailed documentation
```

**Principle:** Each memory file should operate at one level of abstraction.

### 7. **Don't Forget to Prune Session Memory**

**Problem:** Letting conversation history grow unbounded.

**Why it fails:**
- Earlier context gets compressed or forgotten
- Token budget consumed by old conversation
- Reduced space for current work
- Performance degradation

**Solution:**
- Start new conversation for new tasks
- Summarize completed work if context needed
- Use "refresh" prompts: "Let me summarize where we are: [summary]"

**Evidence:**
Users report that conversations over ~50 exchanges (roughly 30-50K tokens of history) start showing degraded recall of earlier context.

## Memory Strategies by Project Type

### Small Personal Projects (<10K LOC)

**Recommended approach:**
- **Primary:** Built-in memory + CLAUDE.md
- **Secondary:** Light manual notes

**Why:**
- Low complexity doesn't justify heavy memory infrastructure
- Automatic memory works well at this scale
- Quick setup and low maintenance

**Example structure:**
```
.claude/
└── CLAUDE.md  # 200-400 tokens
```

### Medium Team Projects (10K-100K LOC)

**Recommended approach:**
- **Primary:** Manual markdown files (version controlled)
- **Secondary:** Built-in memory for personal preferences
- **Optional:** Session notes during major features

**Why:**
- Team needs shared knowledge
- Architecture becomes complex enough to document
- Consistency across team members matters
- Version control enables collaboration

**Example structure:**
```
.claude/
├── CLAUDE.md              # 400-600 tokens
├── architecture.md        # 300-500 tokens
├── domain-glossary.md     # 200-400 tokens
└── decisions/             # ~200 tokens each
    ├── 001-tech-stack.md
    └── 002-api-design.md
```

**Total: ~1500-2500 tokens**

### Large Enterprise Projects (>100K LOC)

**Recommended approach:**
- **Primary:** Structured manual markdown (hierarchical)
- **Secondary:** MCP memory for dynamic/queryable info
- **Tertiary:** Built-in memory for personal preferences
- **Process:** Formal memory governance

**Why:**
- Complexity requires organization
- Multiple subsystems need separate memory
- Searchability becomes critical
- Governance prevents chaos

**Example structure:**
```
.claude/
├── CLAUDE.md
├── architecture/
│   ├── overview.md
│   ├── services/
│   │   ├── auth.md
│   │   ├── payments.md
│   │   └── notifications.md
│   └── data/
│       ├── schema.md
│       └── migrations.md
├── domain/
│   └── [domain-specific docs]
├── decisions/
│   └── [ADRs]
└── onboarding/
    └── for-claude.md  # "What a new Claude session should know"
```

**Total: ~5K-10K tokens, loaded selectively**

### Research/Analysis Projects

**Recommended approach:**
- **Primary:** Session notes with findings
- **Secondary:** Running knowledge base of discoveries
- **Pattern:** Each session contributes to growing knowledge base

**Why:**
- Accumulating knowledge over time
- Need to recall past findings
- Building toward comprehensive understanding

**Example structure:**
```
.claude/
├── CLAUDE.md
├── research-log.md       # Running log of findings
├── sources.md            # Bibliography/references
└── hypotheses.md         # Current theories and questions
```

## Advanced Memory Techniques

### 1. **Semantic Chunking**

**Technique:** Break information into semantic units that Claude can reason about independently.

**Example:**
Instead of one large "API documentation" file, create:
- `api/authentication.md` - Auth endpoints
- `api/users.md` - User endpoints
- `api/payments.md` - Payment endpoints

**Benefit:** Load only relevant chunks into context as needed.

### 2. **Memory Indexing**

**Technique:** Create an index file that maps topics to memory files.

**Example:**
```markdown
# Memory Index

## By Topic
- **Authentication:** See `architecture/auth.md` and `decisions/003-jwt.md`
- **State Management:** See `architecture/state.md`
- **API Design:** See `architecture/api-conventions.md`

## By Recency
- **Latest:** `sessions/2024-01-15-auth-refactor.md`
- **This Week:** `decisions/008-error-handling.md`

## Most Referenced
- `domain/glossary.md` - Business terminology
- `architecture/patterns.md` - Code patterns
```

**Benefit:** Helps Claude (and humans) find relevant memory quickly.

### 3. **Memory Versioning**

**Technique:** Track when memory was last validated.

**Example:**
```markdown
# Architecture Overview

**Last Updated:** 2024-01-15
**Last Validated:** 2024-01-15
**Next Review:** 2024-04-15

## Current Architecture
[content]
```

**Benefit:** Know when information might be stale, prompt for review.

### 4. **Conditional Memory Loading**

**Technique:** Document when certain memory should be loaded.

**Example:**
```markdown
# Memory Loading Guide

## Always Load
- `CLAUDE.md`

## Load for Feature Development
- `architecture/overview.md`
- `domain/glossary.md`

## Load for Bug Fixing
- `architecture/error-handling.md`
- `decisions/*-logging-*.md`

## Load for Refactoring
- `architecture/patterns.md`
- `decisions/` (relevant ones)
```

**Benefit:** Efficient context usage, load only what's needed.

### 5. **Memory Prompting Patterns**

**Technique:** Teach Claude how to use your memory effectively.

**Pattern 1 - Memory Check:**
```
"Before we start, please review:
- .claude/architecture/auth.md
- .claude/domain/glossary.md

Confirm you understand our auth approach and domain terms."
```

**Pattern 2 - Memory Update:**
```
"We just decided to switch from REST to GraphQL. Please:
1. Note this for future sessions
2. Identify which memory files should be updated
3. Suggest updates to those files"
```

**Pattern 3 - Memory Query:**
```
"Check our decision history - have we discussed using Redis before?
What was decided and why?"
```

**Benefit:** Makes memory an active tool, not passive context.

## Security and Privacy Considerations

### Data Residency

**Question:** Where does memory data live?

**By memory type:**
- **Session memory:** Processed by LLM provider, may be retained
- **Built-in memory:** Stored by platform, subject to their policies
- **Manual files:** In your repository, you control storage
- **MCP memory:** Depends on server implementation (can be local)

**Best practices:**
- Read provider privacy policies
- Assume session memory may be used for training (unless opt-out)
- Keep sensitive info in local manual files only
- Use local MCP servers for sensitive projects

### MCP Security Risks

**Risks:**
1. **Code Execution:** MCP servers run arbitrary code
2. **Data Exfiltration:** Malicious server could send data elsewhere
3. **Filesystem Access:** Servers may have broad file access
4. **Network Access:** Servers can make external requests

**Mitigation strategies:**
- **Audit MCP servers:** Review code before using
- **Sandbox:** Run MCP servers in restricted environments
- **Principle of Least Privilege:** Only grant necessary permissions
- **Monitor:** Log MCP server activity
- **Use official servers:** Prefer @modelcontextprotocol/* official servers
- **Keep local:** Self-host MCP servers rather than external services

**Example safe MCP configuration:**
```json
{
  "mcpServers": {
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"],
      "env": {
        "MEMORY_FILE": "/home/user/project/.claude-memory/storage.json"
      }
      // Runs locally, stores in project directory
    }
  }
}
```

**Red flag MCP patterns:**
❌ Servers requesting network access for "memory" tasks
❌ Closed-source memory servers
❌ Servers wanting access outside project directory
❌ Memory servers that "sync to cloud" without explicit consent

## Measuring Memory Effectiveness

### Quantitative Metrics

**Token efficiency:**
```
Usefulness Score = (Questions Avoided + Errors Prevented) / Tokens Used
```

**Example:**
- Memory files: 2000 tokens
- Clarifying questions avoided: 10 per session
- Sessions per week: 5
- Benefit: 50 questions avoided for 2000 token investment

**Recall test:**
Start new session, ask Claude about your project's architecture. Does it know the basics without being told?

### Qualitative Indicators

**Good signs:**
- Claude remembers key decisions without prompting
- Suggestions align with project patterns
- Fewer "that's not how we do it" corrections
- You trust Claude to work semi-autonomously

**Bad signs:**
- Constantly correcting Claude about basic facts
- Suggestions conflict with project conventions
- You always have to provide the same context
- Claude seems "confused" about the project

### A/B Comparison

**Test memory effectiveness:**

**Scenario A (with memory):**
1. Start new session
2. Ask Claude to implement feature X
3. Measure: clarifying questions, errors, alignment with standards

**Scenario B (without memory):**
1. Start new session (don't load memory)
2. Ask Claude to implement feature Y (similar complexity)
3. Measure same metrics

**Compare:** Did memory reduce questions/errors? By how much?

## Community Insights and Real-World Patterns

### From r/ClaudeAI

> "I maintain a 'lessons-learned.md' that gets updated after every major feature. It's been a game-changer for avoiding repeated mistakes." - u/dev_insights

> "Tried elaborate MCP memory system. Went back to simple markdown files. The overhead wasn't worth it for my project size." - r/ClaudeAI thread

> "Keep memory files under 300 tokens each. Broke my 2000-token architecture doc into 6 files and suddenly Claude 'got it' better." - User testimonial

### Patterns from Power Users

**Pattern 1 - The Weekly Review:**
Every Friday, 15-minute memory maintenance:
- Archive completed session notes
- Update any changed decisions
- Prune stale information
- Add new domain terms encountered

**Pattern 2 - The Memory Budget:**
Track memory token usage like technical debt:
- Target: <5K tokens total
- Red alert: >10K tokens
- When over budget, mandatory pruning session

**Pattern 3 - The Onboarding Doc:**
Create `onboarding/for-claude.md`:
```markdown
# Quick Start for New Sessions

## What This Project Is
[2-3 sentences]

## What You Need to Know
- Key fact 1
- Key fact 2
- Key fact 3

## Where to Learn More
- Architecture: see /architecture/overview.md
- Domain: see /domain/glossary.md
- Current work: see /sessions/latest.md
```

Start each session: "Please read the onboarding doc"

**Pattern 4 - The Decision Log:**
Every significant decision gets a 100-token entry:
```markdown
# Decision: Use Zustand over Redux

**Date:** 2024-01-15
**Status:** Accepted

**Context:** Need global state, Redux too heavy
**Decision:** Use Zustand
**Consequences:** Simpler code, less boilerplate, some Redux patterns don't apply

**References:** See PR #123
```

## Recommendations by Use Case

### Solo Developer, Personal Project

**Setup (5 minutes):**
```
project/
└── CLAUDE.md (200 tokens)
```

**Rely on:** Built-in memory + this one file

### Small Team (2-5 developers)

**Setup (30 minutes):**
```
.claude/
├── CLAUDE.md (400 tokens)
├── architecture.md (300 tokens)
└── domain-glossary.md (200 tokens)
```

**Maintain:** Monthly review

### Medium Team (5-20 developers)

**Setup (2-4 hours initial, then ongoing):**
```
.claude/
├── CLAUDE.md
├── architecture/
│   ├── overview.md
│   ├── patterns.md
│   └── data-model.md
├── domain/
│   └── glossary.md
└── decisions/
    └── [ADRs]
```

**Maintain:**
- PR review for memory changes
- Quarterly comprehensive review
- Designated "memory owner"

### Large Organization (>20 developers)

**Setup (Weeks, formal process):**
- Hierarchical memory structure
- MCP memory for dynamic info
- Memory governance process
- Documentation tools integration
- Regular audits

**Maintain:**
- Formal review cycle
- Memory working group
- Automated staleness detection
- Integration with knowledge management

## Conclusion

Effective memory management for LLMs requires:

1. **Strategic Choice:** Use the right memory system for each type of information
2. **Discipline:** Regular maintenance and hygiene
3. **Balance:** Enough memory to be useful, not so much it's noise
4. **Clarity:** Well-organized, easy to find information
5. **Currency:** Keep information up-to-date with reality

**The Golden Rules:**

✅ **Store what's unique to your project**
✅ **Keep it organized and current**
✅ **Version control your memory**
✅ **Measure effectiveness**
✅ **Prune regularly**

❌ **Don't store everything**
❌ **Don't let it go stale**
❌ **Don't store secrets**
❌ **Don't ignore token costs**
❌ **Don't set and forget**

**Start small:** Begin with just CLAUDE.md. Add memory files only when you find yourself repeating the same context across sessions. Let your memory system grow organically based on actual need.

The best memory system is one that **reduces friction** without creating maintenance burden. If maintaining your memory takes more time than it saves, simplify.
