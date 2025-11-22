# Executive Summary

## Purpose
Evidence-based guide to optimizing LLM-assisted development with Claude. All recommendations backed by research, community insights, and real-world usage.

## Core Guides

**CLAUDE.md Best Practices:** Keep project-specific (300-500 tokens), use as navigation hub, not framework documentation copy.

**Agent Definition:** Single-responsibility agents with clear roles, automate skill activation with hooks.

**Memory Management:** Selective, not comprehensive. Store only what's unique to your project.

**Context Window Efficiency:** 20K tokens of relevant content beats 150K of mixed content.

**Tools and MCP:** Audit before use, keep data local when possible, build productivity system around Claude Code.

**Model Selection:** Match capability to complexity—most tasks don't need the most powerful model.

**Effective Workflows:** Small iterative changes with verification. Use dev docs for complex tasks to prevent tracking loss.

**Spec-Driven Development:** Replace ad-hoc prompting with clear specifications that generate predictable, testable code.

**Common Mistakes:** Never let Claude write >200 lines without testing. Re-prompt when output is suboptimal.

## Key Principles

1. **Understand, Don't Just Use:** Read and understand every line of AI-generated code.
2. **Small and Iterative:** 15-45 minute cycles, test after every change.
3. **Plan Before Implementing:** 10-15 minutes planning saves hours of rework.
4. **Context is Precious:** Only load what's relevant for current task.
5. **Security First:** Audit MCP servers, never commit secrets, least privilege.
6. **Match Tool to Task:** Haiku for simple, Sonnet for most, Opus for complex.
7. **Review Everything:** AI makes mistakes—verify all generated code.
8. **Document Decisions:** Capture why decisions were made.

## Golden Rules

**200-Line Rule:** Never let Claude write more than 200 lines without running and testing.

**Context Budget Rule:** If Claude doesn't need it for the current task, don't include it.

**Security Rule:** Audit before trusting. Least privilege always.

## Quick Reference

### DO:
- Read and understand all generated code
- Test after every change
- Break tasks into 15-45 minute cycles
- Plan before implementing
- Start fresh conversations for new tasks
- Keep CLAUDE.md under 500 tokens
- Audit MCP servers before using

### DON'T:
- Let AI write >200 lines without testing
- Skip the planning phase
- Accept code without understanding it
- Work in one mega-conversation for days
- Load entire codebases into context
- Store secrets in config files
- Use MCP servers without auditing
- Treat AI as a replacement for thinking

## Quick Start Paths

**Beginners:** CLAUDE.md Best Practices → Effective Workflows → Model Selection → Common Mistakes

**Intermediate:** Context Window Efficiency → Memory Management → Agent Definition

**Advanced:** Tools and MCP → Custom workflows → Team collaboration patterns

---

**Bottom Line:** AI-assisted development requires discipline. Success requires understanding generated code, working in small verified iterations, planning before implementing, and maintaining quality standards.
