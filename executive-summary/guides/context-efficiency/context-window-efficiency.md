# Context Window Efficiency - Executive Summary

## Critical Facts

**Context Limits:** Claude has a 200K token limit (150K words or 600 pages). This fills quickly with code, conversation history, and memory files.

**Token Costs:** Every token costs money, latency, quality, and capacity.

**Quality Degradation:** LLM performance declines with context length:
- 0-50K tokens: 98-99% recall
- 50-100K tokens: 95-98% recall
- 100-150K tokens: 90-95% recall
- 150-200K tokens: 85-90% recall

**Key Finding:** Shorter, focused context is better.

## Four Core Principles

1. **Relevance Principle:** Only include information directly relevant to the current task.

2. **Summarization Principle:** Summarize when possible, include full text only when necessary.

3. **Just-In-Time Principle:** Load information when needed, not preemptively.

4. **Forgetting Principle:** Deliberately discard irrelevant context. Start fresh conversations after 50+ exchanges.

## Critical Strategies

**File References:** Reference files and load snippets as needed instead of loading entire files.

**Strategic Chunking:** Break large tasks into smaller, context-efficient chunks.

**Diff-Based Updates:** Show only what changed, not entire files (90% token savings).

**Code Summarization:** Use comments/docstrings for background context (80-90% token savings).

**Conversation Pruning:** Reset with summaries after completing subtasks or use handoff documents between sessions.

**Schema Over Data:** Use TypeScript types instead of example data (95%+ token savings).

**Lazy Loading:** Let Claude request needed files rather than pre-loading everything.

## Typical Context Breakdown

- Conversation history: 30K-50K tokens
- Files in context: 20K-100K tokens
- Memory files: 2K-10K tokens
- System prompts: 5K-15K tokens
- Output buffer: 10K-30K tokens
- Working buffer: 20K+ tokens

**Total: 87K-225K tokens** - window fills quickly.

## Key Anti-Patterns

- Loading entire codebase preemptively
- Including irrelevant files
- Maintaining mega-conversations without pruning
- Using full data examples instead of schemas
- Keeping completed task context in memory
