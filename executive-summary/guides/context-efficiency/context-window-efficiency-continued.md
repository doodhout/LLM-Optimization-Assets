# Executive Summary: Context Window Efficiency (Advanced)

## Core Strategies

**Context Windowing:** Analyze long documents in sections rather than loading entirely (90%+ token savings).

**Directory Structures:** Show file structure with comments instead of full file contents (95%+ token savings for orientation).

**Progressive Disclosure:** Start high-level, drill down only where needed. Example: architecture overview → module overview → specific implementation.

## Critical Don'ts

**Don't dump entire codebases:** Claude can't effectively process 100K+ tokens; important details get lost, quality degrades.

**Don't include build artifacts:** Exclude dist/, node_modules/, compiled binaries, minified code, lock files. These waste massive token budget on irrelevant content.

**Don't repeat information:** Context is already in conversation history; repeating wastes tokens.

**Don't keep dead conversation threads:** Start new conversation with summary when switching major tasks. Previous completed work consumes tokens without adding value.

**Don't load framework documentation:** Claude already knows common frameworks; token budget better spent on project-specific code.

**Don't mix active and archive context:** Historical implementations and migration notes cause confusion and waste tokens.

## Token Budget Management

**Budget zones:**
- Green: <50K tokens
- Yellow: 50K-100K tokens (optimize)
- Red: >100K tokens (must optimize)

**Efficiency score:** Useful tokens / Total tokens
- Good: >0.7
- Acceptable: 0.4-0.7
- Poor: <0.4

**Token estimation:** ~4 characters per token for English text.

## Context-Efficient Workflows

**Feature Implementation:** Planning (3K tokens) → Implementation (15K) → Testing (10K) → Review in fresh conversation (5K). Total: 33K vs 100K+ loading everything.

**Bug Fixing:** Reproduction (2K) → Investigation (8K) → Fix (5K). Total: 15K tokens.

**Refactoring:** Analysis (5K) → Incremental execution (8K per iteration, drop previous) → Integration (12K).

## Tool-Specific Optimizations

**Claude.ai:** Use Projects feature, start new chats for major tasks, selective "Add context", export important conversations.

**Claude Code CLI:** Configure .claudeignore, use focused prompts, leverage CLAUDE.md, start new sessions for new tasks.

**API Usage:** Implement context management, cache stable context, use prompt caching for large stable contexts, measure token usage, programmatic rolloff.

**IDEs:** Configure relevant file patterns only, use context-aware features (@files, @code), regularly clear irrelevant context.

## Advanced Techniques

**Context Partitioning:** Divide project into independent partitions (frontend, backend, database, infrastructure). Load only relevant partition (75% reduction).

**Context Inheritance:** Base layer (always loaded) + feature layers (loaded as needed).

**Context Compression:**
- Symbolic representation: Describe patterns instead of showing full code
- Pattern templates: Show one reference implementation, describe others relative to it
- Difference encoding: Describe variations from known patterns
- Token savings: 60-80%

**Dynamic Context Loading:** Priority queue approach - critical (current files) → high (dependencies) → medium (related) → skip low (reference) and archive (historical).

## Performance Indicators

**Good efficiency signs:** Claude finds info quickly, focused responses, minimal clarification requests, fast response times, consistent quality.

**Poor efficiency signs:** Asking for already-provided info, vague responses, long response times, mixing up codebase parts, degrading quality over time.

## Common Context Patterns

**Focused Session:** 10K-30K tokens for bug fixes, small features, isolated refactoring.

**Exploratory Session:** 5K growing to 40K for onboarding, architecture analysis.

**Implementation Session:** 30K-60K for feature development, multi-file changes.

**Review Session:** 10K-20K fresh start for code review, testing, validation.

**Deep Dive Session:** 60K-100K loaded in phases for architecture changes, complex debugging.

## Golden Rule

Every token in context must earn its place by being directly relevant to the current task. 20K tokens at 100% relevance outperforms 150K tokens at 30% relevance. Quality beats volume.

## Quick Wins

- Start with clear, focused goals
- Use file references instead of dumping codebases
- Summarize completed work rather than keeping full history
- Start fresh sessions for new major tasks
- Configure ignore patterns to exclude build artifacts, dependencies, generated files

## Key Principles

1. **Relevance:** Only load what matters for current task
2. **Summarization:** Compress when possible, expand when necessary
3. **Just-in-Time:** Load as needed, not preemptively
4. **Forgetting:** Deliberately prune completed context
5. **Measurement:** Track and optimize token usage
