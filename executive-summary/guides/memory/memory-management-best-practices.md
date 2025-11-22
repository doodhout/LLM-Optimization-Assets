# Memory Management Best Practices - Executive Summary

## Memory System Types

**Session Memory**: Context within single conversation. Limited by context window, lost when session ends.

**Built-in Memory**: Platform-managed (Claude Projects). Automatic but limited control, may learn incorrect information.

**Manual Markdown Files**: Version-controlled knowledge base. Full control, requires maintenance.

**MCP Memory Servers**: Programmatic memory with various backends. Scalable but requires technical setup.

**Hybrid**: Combine systems strategically based on information type.

## Critical Best Practices

**Use Right Memory Type**:
- Stable project conventions: CLAUDE.md
- Personal preferences: Built-in memory
- Architecture/domain knowledge: Manual markdown
- Dynamic queryable data: MCP memory
- Secrets: Never store

**Memory Budget**: Keep project memory files under 5K tokens total. Above 10K causes performance degradation.

**File Size**: Optimal range 150-300 tokens per file. Minimum 50 tokens to be useful, maximum 500 tokens.

**Structure Files**: Use inverted pyramid - critical information first, details later.

**Hierarchical Organization**: Stable/general at top level, volatile/specific in subdirectories.

**Version Control**: Store memory files in git, review changes like code.

**Reference, Don't Duplicate**: Link to authoritative sources rather than copying.

**Regular Maintenance**:
- Weekly: Review/archive session notes
- Monthly: Audit for stale information
- Quarterly: Major cleanup
- Per milestone: Update architecture docs

## Critical Anti-Patterns

**Don't Store Everything**: Be selective. Only information that's future-useful and non-derivable from code.

**Don't Store Secrets**: Never include API keys, passwords, credentials, or PII.

**Don't Allow Contradictions**: Single source of truth per topic. Update all references when decisions change.

**Don't Mix Abstraction Levels**: Keep strategic and tactical information separate.

**Don't Let Memory Go Stale**: Memory contradicting code causes errors and wasted time.

**Don't Ignore Session Memory**: Start new conversations for new tasks. Sessions over 50 exchanges show degraded recall.

## Recommendations by Project Size

**Small Personal (<10K LOC)**: Built-in memory + CLAUDE.md (200-400 tokens)

**Medium Team (10K-100K LOC)**: Manual markdown files (1500-2500 tokens total) + built-in memory for preferences

**Large Enterprise (>100K LOC)**: Hierarchical markdown + MCP memory + formal governance (5K-10K tokens, loaded selectively)

**Research Projects**: Session notes + running knowledge base of discoveries

## Security

**Data Residency**: Session and built-in memory may be retained by providers. Manual files you control.

**MCP Risks**: Code execution, data exfiltration, filesystem access. Audit servers, use official implementations, run locally.

## Measuring Effectiveness

**Token Efficiency**: (Questions Avoided + Errors Prevented) / Tokens Used

**Good Signs**: Claude remembers decisions without prompting, suggestions align with patterns, fewer corrections needed.

**Bad Signs**: Constantly correcting basic facts, suggestions conflict with conventions, always providing same context.

## Golden Rules

**DO**:
- Store what's unique to your project
- Keep organized and current
- Version control memory
- Measure effectiveness
- Prune regularly

**DON'T**:
- Store everything
- Let it go stale
- Store secrets
- Ignore token costs
- Set and forget

## Key Insight

Start with just CLAUDE.md. Add memory files only when repeating same context across sessions. Best memory system reduces friction without creating maintenance burden. If maintenance exceeds time saved, simplify.
