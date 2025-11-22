# Executive Summary: Context Window Management Approaches

## Seven Major Approaches

1. **Dev Docs Pattern** (5-star effectiveness): Three-file structure (plan.md, context.md, tasks.md) in version control
2. **CLAUDE.md/Static Files** (3-star): Project-level patterns and conventions
3. **Session Commands** (2-star): Built-in /compact, /clear, /resume
4. **MCP External Memory** (4-star): Automatic cross-tool persistence via MCP servers
5. **Vector DB/RAG** (4-star): Semantic search for massive codebases
6. **Sub-agent Architecture** (4-star): Specialized agents with isolated contexts
7. **Chunking/Summarization** (2-star): Breaking large contexts into chunks

## Rankings by Effectiveness

**#1 Dev Docs Pattern** - Best for complex multi-day/week features
- Lossless persistence across all context resets
- Git-trackable, human-readable, zero infrastructure
- Proven in 300k LOC production rewrite over 6 months
- Resume speed: 30 seconds to full context restoration

**#2 CLAUDE.md** - Best for project-level context
- Static guidelines for patterns, conventions, architecture
- Complements Dev Docs (not a replacement)
- Low maintenance, team-shareable

**#3 Session Commands** - Best for short-term cleanup
- Built-in, zero setup
- Lossy summarization, ephemeral (hours not weeks)
- Not suitable for complex projects requiring persistence

**#4 MCP External Memory** - Best for large teams
- Automatic cross-tool memory
- High setup complexity, potential cost
- Overkill for solo developers

**#5 Vector DB/RAG** - Best for code discovery
- Semantic search in 500k+ LOC codebases
- Extreme complexity, high cost
- Retrieves code but not task state

**#6 Sub-agent Architecture** - Best for workflow specialization
- Context isolation per specialized task
- High coordination overhead
- Complements rather than replaces Dev Docs

**#7 Chunking/Summarization** - Best for document processing
- Simple but lossy (10-20% loss per level)
- Poor choice for development continuity

## Critical Success Factors

1. **Update Frequency**: Update context after every major milestone, not just at session end. Developers updating every 30-60 minutes report 90%+ success vs. 50-60% for end-of-day updates.

2. **Explicit Decision Logging**: Document WHY, not just WHAT. Include reasoning and trade-offs.

3. **Lossless Handoffs**: Only Dev Docs and static files preserve information without loss. Summarization, sub-agents, and RAG are lossy.

4. **Session Progress Visibility**: Context must answer "where are we?" in 10 seconds. Place progress section at top.

## Evidence-Based Recommendation

**For 80% of developers: Use Dev Docs Pattern + CLAUDE.md + Session Commands**

Reasons:
- Zero infrastructure cost
- Lossless information preservation
- Works for solo developers and teams
- Simple to adopt

**Add MCP/RAG/Sub-agents only when:**
- Team size >10 developers
- Codebase >500k LOC
- Budget for infrastructure exists

## Common Anti-Patterns

1. **Using technology to avoid discipline**: Automatic systems cannot capture nuance of human decisions. Manual updates worth the 30 seconds.

2. **Over-relying on summarization**: Stacking summaries degrades fidelity exponentially.

3. **Not using any system**: Even minimal dev docs beats nothing.

4. **Premature infrastructure**: Dev docs solve 95% of cases without infrastructure. Start simple, add complexity only when proven needed.

## Quick Adoption Path

- Week 1: Create CLAUDE.md with project patterns
- Week 2: Add Dev Docs for one feature, measure resume speed
- Week 3: Refine based on what worked
- Week 4: Make it habit for all complex tasks
