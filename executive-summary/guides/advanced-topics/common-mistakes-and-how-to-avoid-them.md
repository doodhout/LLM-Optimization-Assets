# Executive Summary: Common Mistakes and How to Avoid Them

## Quality and Consistency

**AI Output is Stochastic:** Same input produces different outputs; re-prompt when results are poor rather than trying to fix bad luck.

**Prompt Quality Matters:** Slight wording changes drastically affect results. Vague, lazy prompts yield poor output.

**Re-Prompting is Powerful:** Hit double-ESC to branch from previous prompts with better constraints. Often yields 10x better results on second try.

**Know When to Step In:** If Claude struggles for 30+ minutes on something you can fix in 2 minutes, just fix it yourself and move on.

**Provide Context Systematically:** Reference existing patterns, be specific about requirements, and iterate with learnings.

## Critical Mistakes

**1. Vibe Coding:** Accepting code without understanding it creates technical debt. Read and understand every line generated.

**2. Too Much at Once:** Never let Claude write more than 200 lines without testing. Break tasks into small chunks.

**3. Skipping Planning:** Always plan before implementing. Understand requirements, design solution, break into tasks.

**4. Not Testing Continuously:** Test after every change. Debugging is 15x faster when testing incrementally.

**5. Mega-Conversations:** Context degrades after ~50 exchanges or 3-4 hours. Start fresh conversations for new tasks.

**6. Ignoring Code Review:** Review every piece of generated code. Unreviewed AI code has significantly higher bug rates.

**7. Over-Relying on Powerful Models:** 70-80% of tasks work fine with Haiku or Sonnet. Use Opus only for complex tasks. Start cheap, escalate if needed.

**8. Context Window Mismanagement:** Load only relevant files. Claude's recall degrades as context fills (98% at 0-50K tokens, 85% at 150K-200K tokens).

**9. Secrets in Config Files:** Never store API keys or passwords in configuration files. Use environment variables and .env files.

**10. Unaudited MCP Servers:** Review source code, check dependencies, and verify permissions before using MCP servers. Malicious servers can exfiltrate data.

**11. Not Documenting Decisions:** Document why AI-generated decisions were made, including context, options considered, and consequences.

**12. Neglecting Architecture:** Design before coding. Maintain SOLID principles and conduct regular architectural reviews.

## Workflow Anti-Patterns

**The Rewrite Loop:** Not learning from failures. Analyze what went wrong and improve prompts iteratively.

**The Feature Factory:** Shipping unreviewed features rapidly leads to technical debt crisis. Maintain quality at each step.

**The AI Dependency:** Can't code without AI or don't understand generated code. Use AI as assistant, not replacement.

## Prevention Strategies

**Team Standards:** Establish guidelines (review all AI code, max 200 lines per generation, test continuously, document decisions).

**Code Review Checklist:** Verify understanding, conventions, security, performance, testing, architecture fit.

**Regular Retrospectives:** Weekly discussions about what worked, what didn't, and process improvements.

**Monitor Metrics:** Track code churn, bug rates, technical debt to adjust processes.

## Key Principle

AI amplifies thinking, not replaces it. You remain responsible for quality, security, and maintainability. Successful AI-assisted development requires understanding, incrementalism, planning, testing, review, documentation, and architectural discipline.
