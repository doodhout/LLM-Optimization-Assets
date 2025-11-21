# Common Mistakes and How to Avoid Them

## Overview

This guide consolidates common mistakes developers make when working with LLMs like Claude, based on community experience, research, and real-world usage patterns. Learning from these mistakes can save you significant time and frustration.

## Understanding Quality and Consistency

### The Reality of AI Output Quality

**Important context:** There's a recurring theme in forums about frustrations with usage limits and concerns about output quality declining. These are valid experiences that deserve acknowledgement.

**What's working for experienced users:**
- AI output can actually improve when you refine your workflow consistently
- The key is understanding what impacts quality and addressing it systematically
- Sometimes Claude misses the mark - that's the nature of stochastic models
- Quality comes from the system you build around Claude, not Claude alone

**Factors affecting output quality:**

**1. Stochastic nature:**
- AI models are inherently random
- Same input can produce different outputs
- Sometimes randomness doesn't go your way
- No amount of prompting fixes bad luck - re-prompt instead

**2. Prompt structure matters:**
- Models take things quite literally
- Slight wording changes can produce vastly different results
- Ambiguous phrasing leads to inferior outputs
- Misworded requests cause quality issues

**3. Fatigue and laziness:**
```
Beginning of day: Well-structured, thoughtful prompts → Great results
End of day: Lazy, vague prompts → Poor results

Pattern: Quality degrades with prompt effort
```

**Community insight:**
> "I've had my fair share of terrible prompting, usually towards the end of the day where I'm getting lazy. The results really show. So next time you think Anthropic shadow-nerfed Claude, take a step back and reflect on how you are prompting." - 6-month power user

### The Power of Re-Prompting

**When Claude produces suboptimal code:**

**Strategy: Hit double-ESC and branch from previous prompt**

```
First attempt: "Add error handling to the auth service"
Result: Suboptimal code with too much try-catch nesting

Re-prompt: "Add error handling to the auth service. Use a centralized
error handler instead of nested try-catch blocks. Follow the repository
pattern we use elsewhere."
Result: Much better code that fits project patterns
```

**Why re-prompting works:**
- You now know what you DON'T want
- Can provide additional constraints
- Clarify ambiguous parts
- Guide toward better solution

**How to re-prompt effectively:**
```
1. Hit double-ESC to see previous prompts
2. Select the prompt you want to branch from
3. Add what was missing:
   - "Use approach X, not approach Y"
   - "Follow the pattern in file Z"
   - "Focus on performance, not just correctness"
4. Let Claude try again with better constraints
```

**Benefits:**
- Often get 10x better results on second try
- Learn what constraints help
- Build better prompting skills
- Don't settle for mediocre output

**Community wisdom:**
> "Re-prompt often. You'd be amazed how often you can get way better results armed with the knowledge of what you don't want when giving the same prompt." - Experienced Claude Code user

### When to Step In and Fix It Yourself

**The pragmatic truth: Sometimes you just need to intervene.**

**AI is incredible, but it's not magic.** There are certain problems where pattern recognition and human intuition just win.

**When to step in:**
```
Signs you should fix it yourself:
- 30+ minutes watching Claude struggle
- You could fix it in 2 minutes
- Logic puzzle that needs human intuition
- Problem requiring real-world common sense
- Claude is in a loop trying different approaches
```

**Examples of "just fix it" situations:**
- Logic puzzles or brain teasers
- Problems requiring domain expertise Claude lacks
- Edge cases with subtle business logic
- Quick fixes you can see immediately
- Debugging that needs real-world context

**The right mindset:**
```
❌ Wrong: "But the AI should do everything"
✅ Right: "AI amplifies my capabilities - I'm still the developer"

Think of it like teaching someone to ride a bike:
Sometimes you need to steady the handlebars for a second before letting go again
```

**After you fix it:**
```
1. Fix the issue yourself (2 minutes)
2. Explain to Claude what you fixed and why
3. Continue with Claude on next task
4. No shame, no wasted time
```

**Community insight:**
> "I've had times where Claude spent 30 minutes on something I could fix in 2 minutes. No shame in stepping in. Think of it like steadying the handlebars on a bike - just help out and keep moving." - Production user

### Quality Through Systematic Approach

**The wisdom: "Ask not what Claude can do for you, ask what context you can give to Claude"**

**Building a quality system:**

**1. Provide comprehensive context:**
```
Poor: "Fix the bug"
Good: "Fix the authentication bug. The issue is in auth-service.ts where
token validation fails for refresh tokens. See our auth pattern in
docs/auth-flow.md. Follow the error handling pattern in base-service.ts."
```

**2. Reference existing patterns:**
```
Poor: "Create a new API endpoint"
Good: "Create a new API endpoint following the pattern in user-routes.ts.
Use the same controller structure, error handling, and validation approach."
```

**3. Be specific about what you want:**
```
Poor: "Make it better"
Good: "Refactor for readability. Extract the nested loops into separate
functions. Add comments explaining the algorithm. Keep performance the same."
```

**4. Iterate with learnings:**
```
Attempt 1: Generic request → OK result
Attempt 2: Add missing constraints → Better result
Attempt 3: Reference specific patterns → Great result

Don't settle after attempt 1
```

## Critical Mistakes

### 1. The "Vibe Coding" Trap

**What it is:**
"Vibe coding" is an exploratory, prompt-first approach where developers rapidly prompt, get code, and iterate, often without reading or understanding the generated code.

**Why it's problematic:**
- Creates code you don't understand
- Accumulates technical debt rapidly
- Makes debugging extremely difficult
- Violates fundamental software engineering principles
- Leads to duplicated code and poor patterns

**Evidence:**
Research shows that AI-generated code often results in significant increases in duplicated code blocks and code churn. Forrester forecasts an "incoming technical debt tsunami" over the next 2 years due to concerns around security, code quality, and automation "overreach."

**How to avoid it:**

✅ **Read and understand every line of generated code**
```
Bad workflow:
1. Prompt Claude
2. Copy code
3. Run it
4. If it works, move on

Good workflow:
1. Prompt Claude
2. Read and understand the code
3. Ask questions about unclear parts
4. Verify it follows your patterns
5. Test it
6. Review for quality
7. Then commit
```

✅ **Treat AI-generated code as a draft, not final**
- Review for logic errors
- Check for security issues
- Verify it fits your architecture
- Ensure it follows your conventions
- Test edge cases

✅ **Never accept code blindly**
> "The quality of AI-generated code depends entirely on how you prompt it - your architecture, constraints, and design rules are the new seniority."

### 2. Letting Claude Write Too Much at Once

**What it is:**
Asking Claude to implement large features or generate hundreds of lines of code in a single go.

**Why it's problematic:**
- Bugs compound across files
- Hard to debug when things go wrong
- Context window fills with broken code
- Quality degrades with complexity
- Often faster to restart than fix

**The 200-line rule:**
> "Never let Claude write more than 200 lines without running and testing the code." - r/ClaudeAI consensus

**Evidence:**
Users consistently report that large, monolithic implementations lead to chaos, while small iterative changes work smoothly.

**How to avoid it:**

✅ **Break tasks into small chunks**
```
Instead of:
"Implement the entire user management system"

Do this:
"Implement UserService with basic CRUD operations"
[Test and verify]
"Now implement user authentication"
[Test and verify]
"Now add user roles and permissions"
[Test and verify]
```

✅ **Work in working states**
- Each iteration should leave code in a runnable state
- Test after each small addition
- Commit frequently
- Never accumulate broken code

✅ **Follow the 15-45 minute cycle**
Each task should take:
- 10-15 minutes: Planning
- 20-30 minutes: Implementation
- 5-10 minutes: Testing and review

If it's taking longer, the task is too big.

### 3. Skipping the Planning Phase

**What it is:**
Jumping straight into implementation without understanding the problem or designing a solution.

**Why it's problematic:**
- Build the wrong thing
- Miss important requirements
- Create architectural messes
- Waste time on rework
- End up with unmaintainable code

**Real-world example:**
```
User: "Build a dashboard"
Claude: [Generates 800 lines of dashboard code]
User: "This isn't what I wanted..."
Result: 2 hours wasted
```

**How to avoid it:**

✅ **Always plan before implementing**
```
Step 1: Understand the requirement (5 minutes)
- What problem are we solving?
- Who is the user?
- What are the success criteria?

Step 2: Design the solution (10 minutes)
- What components do we need?
- How do they interact?
- What are the data flows?
- What are the edge cases?

Step 3: Break into tasks (5 minutes)
- List specific implementation steps
- Identify dependencies
- Order tasks logically

Step 4: Implement incrementally (iterative)
- One task at a time
- Test each piece
- Adjust plan as you learn
```

✅ **Use planning prompts**
```
Good prompts:
- "Help me plan implementing [feature]"
- "What components would we need for [feature]?"
- "What's the best approach to [problem]?"
- "Break down [large task] into smaller tasks"

Bad prompts:
- "Implement [feature]" (no planning)
- "Write code for [vague requirement]"
```

### 4. Not Testing Continuously

**What it is:**
Writing lots of code and testing at the end, or not testing at all.

**Why it's problematic:**
- Don't know what broke when tests fail
- Debugging becomes exponentially harder
- Accumulate broken code
- Waste time hunting for bugs
- End up with low-quality code

**Evidence:**
Studies show that debugging is 15x faster when testing after each change versus testing everything at the end.

**How to avoid it:**

✅ **Test after every change**
```
Write code → Write/update tests → Run tests → Pass? → Next change

Not:
Write lots of code → Hope it works → Run tests → Debug for hours
```

✅ **Red-Green-Refactor (TDD approach)**
```
1. Red: Write failing test
2. Green: Write minimal code to pass
3. Refactor: Improve code quality
4. Repeat
```

✅ **Use different test levels appropriately**
- Unit tests: For individual functions/components
- Integration tests: For component interactions
- End-to-end tests: For critical user flows

### 5. Working in Mega-Conversations

**What it is:**
Keeping the same conversation going for days, implementing multiple unrelated features in one thread.

**Why it's problematic:**
- Context window fills with irrelevant history
- Claude mixes up different features
- Performance degrades over time
- Can't find relevant information
- Model gets confused about decisions

**Evidence:**
Users report that conversations over ~50 exchanges or 3-4 hours start showing degraded recall and quality.

**How to avoid it:**

✅ **Start fresh conversations regularly**
```
Good pattern:
- Feature A → New conversation
- Feature B → New conversation
- Feature C → New conversation

Each conversation:
- Clear, focused context
- Relevant history only
- High-quality output
```

✅ **Use the /clear command**
Between unrelated tasks, clear the context to give Claude a fresh start while staying in the same session.

✅ **Create handoff documents**
```markdown
# Session Summary: Auth Implementation

## Completed:
- JWT authentication
- Refresh tokens
- Middleware

## Decisions:
- Using jsonwebtoken library
- 1-hour token expiry
- httpOnly cookies for refresh

## Next:
- Password reset flow
- Email verification

Start new session with this summary
```

### 6. Ignoring Code Review

**What it is:**
Accepting Claude's code without careful review, or skipping review entirely.

**Why it's problematic:**
- LLMs make mistakes
- May not follow your conventions
- Can introduce bugs or security issues
- Code quality degrades over time
- Accumulates technical debt

**Evidence:**
AI-generated code often undergoes substantial modification or removal after initial acceptance, leading to increased code churn. Studies show that unreviewed AI code has significantly higher bug rates.

**How to avoid it:**

✅ **Review every piece of generated code**
```
Review checklist:
- [ ] Does it solve the problem correctly?
- [ ] Are there any bugs or edge cases?
- [ ] Does it follow our conventions?
- [ ] Is it secure?
- [ ] Is it performant?
- [ ] Is it maintainable?
- [ ] Are there better approaches?
```

✅ **Use code review agents**
After Claude implements something, use a separate code review agent (or fresh Claude session) to review it.

✅ **Allocate time for review**
- Implementation: 70% of time
- Review: 20% of time
- Documentation: 10% of time

### 7. Over-Relying on the Most Powerful Model

**What it is:**
Always using Claude Opus (or the most expensive model) for every task, regardless of complexity.

**Why it's problematic:**
- Expensive (10-15x cost of Haiku)
- Slower responses
- No quality benefit for simple tasks
- Wastes budget
- Slower workflow

**Evidence:**
Community reports show that 70-80% of tasks can be handled by Haiku or Sonnet with no quality loss. Users who switched to appropriate models saved 60%+ on costs.

**How to avoid it:**

✅ **Match model to task complexity**
```
Simple tasks → Haiku
- Test generation
- Boilerplate code
- Simple refactoring
- Documentation
- Code formatting

Standard tasks → Sonnet 4.5
- Feature implementation
- Code review
- Bug fixing
- Most production work

Complex tasks → Opus 4
- Novel algorithms
- Architecture decisions
- Security audits
- Complex debugging
```

✅ **Start cheap, escalate if needed**
```
1. Try Haiku
2. If insufficient, use Sonnet 4.5
3. If still stuck, use Opus 4
```

### 8. Not Managing Context Window Efficiently

**What it is:**
Loading entire codebases, keeping irrelevant history, including unnecessary files.

**Why it's problematic:**
- Wastes token budget
- Reduces performance
- Important info gets lost in noise
- Slower responses
- Higher costs

**Evidence:**
Studies show that Claude's recall degrades as context fills:
- 0-50K tokens: 98-99% recall
- 50-100K tokens: 95-98% recall
- 100-150K tokens: 90-95% recall
- 150K-200K tokens: 85-90% recall

Shorter, focused context significantly outperforms large, unfocused context.

**How to avoid it:**

✅ **Only load relevant files**
```
Bad: Load entire src/ directory (100K tokens)
Good: Load only files related to current task (10K tokens)
```

✅ **Use references instead of copying**
```
Bad: Include entire API documentation
Good: "Follow the API patterns in /docs/api.md"
```

✅ **Summarize completed work**
```
Bad: Keep full history of finished tasks
Good: "Previous session: Implemented auth (JWT, middleware, tests passing)"
```

✅ **Start fresh conversations for new tasks**

### 9. Storing Secrets in Configuration Files

**What it is:**
Putting API keys, passwords, or tokens directly in CLAUDE.md, .clinerules, or MCP configs.

**Why it's problematic:**
- Security risk
- Gets committed to git
- Shared with team unintentionally
- May be logged or cached
- Hard to rotate secrets

**Evidence:**
Multiple security incidents have occurred from credentials being included in configuration files that were then committed to git or shared publicly.

**How to avoid it:**

✅ **Use environment variables**
```json
// Bad - secrets in config
{
  "github_token": "ghp_xxxxxxxxxxxxx"
}

// Good - reference environment variable
{
  "github_token": "${GITHUB_TOKEN}"
}
```

✅ **Use .env files (gitignored)**
```bash
# .env (add to .gitignore)
GITHUB_TOKEN=ghp_xxxxxxxxxxxxx
DATABASE_URL=postgresql://...

# Reference in code
process.env.GITHUB_TOKEN
```

✅ **Never commit secrets**
```bash
# .gitignore
.env
.env.local
secrets.json
credentials.json
*.key
```

### 10. Trusting MCP Servers Without Auditing

**What it is:**
Installing and using MCP servers from the internet without reviewing their code or understanding their permissions.

**Why it's problematic:**
- Security risk (malicious servers)
- Data exfiltration
- Unauthorized access
- Code execution vulnerabilities
- Supply chain attacks

**Evidence:**
Research has shown that malicious MCP servers can be deployed through aggregator platforms and that existing audit mechanisms often fail to detect them. These servers can access private files, trigger unintended system commands, and exfiltrate data.

**How to avoid it:**

✅ **Audit MCP servers before use**
```
Checklist:
- [ ] Review source code
- [ ] Check dependencies (npm audit)
- [ ] Understand permissions requested
- [ ] Verify author/maintainer trustworthiness
- [ ] Check community feedback
- [ ] Look for red flags
```

✅ **Red flags to watch for**
🚩 Requests network access to unknown domains
🚩 Wants file access outside project directory
🚩 Executes arbitrary commands
🚩 Closed-source or obfuscated code
🚩 Excessive environment variable requests
🚩 No clear documentation

✅ **Use principle of least privilege**
```
Bad: MCP server with full filesystem access
Good: MCP server restricted to project directory only
```

✅ **Prefer official MCP servers**
```
Official (safer):
@modelcontextprotocol/server-filesystem
@modelcontextprotocol/server-github
@modelcontextprotocol/server-memory

Community (audit required):
random-mcp-server-from-internet
```

### 11. Not Documenting AI-Generated Decisions

**What it is:**
Accepting AI suggestions without documenting why decisions were made.

**Why it's problematic:**
- Future developers don't understand reasoning
- Can't evaluate if decision is still valid
- Hard to maintain code
- Repeat same discussions
- Lose institutional knowledge

**How to avoid it:**

✅ **Document significant decisions**
```markdown
# Decision: Use Zustand over Redux

**Date:** 2024-01-15
**Made with:** Claude Sonnet 4.5

**Context:** Need global state management
**Options considered:** Redux, Zustand, Context API
**Decision:** Zustand
**Reasoning:**
- Simpler API than Redux
- Less boilerplate
- Good TypeScript support
- Sufficient for our scale

**Consequences:**
- Team needs to learn Zustand
- Redux patterns don't directly apply
- May need to migrate if requirements grow significantly
```

✅ **Document mistakes too**
```markdown
# Mistake Log

## 2024-01-16: Over-engineered auth system
**Problem:** Claude generated complex auth with many abstractions
**Impact:** Hard to understand, over-engineered for needs
**Lesson:** Start simple, add complexity only when needed
**Fix:** Simplified to basic JWT auth
```

✅ **Share learnings with team**
- Regular review of AI-generated code patterns
- Discuss what works and what doesn't
- Build team conventions for AI-assisted development

### 12. Neglecting Architecture for Speed

**What it is:**
Prioritizing rapid AI-generated code over good architectural design.

**Why it's problematic:**
- Creates architectural mess
- Hard to maintain long-term
- Accumulates technical debt
- Eventually have to rebuild
- Short-term gains, long-term pain

**Evidence:**
Research shows that AI-assisted development can let developers skip design thinking that prevents problematic patterns, and is biased toward adding new code rather than revisiting old decisions.

**How to avoid it:**

✅ **Design before coding**
```
1. Understand the problem
2. Design the architecture
3. Document key decisions
4. Then start implementing
```

✅ **Maintain architectural principles**
```
Even with AI assistance:
- Follow SOLID principles
- Keep components focused
- Maintain clear boundaries
- Avoid tight coupling
- Design for testability
```

✅ **Regular architectural reviews**
```
Weekly/bi-weekly:
- Review architectural patterns
- Identify emerging issues
- Refactor proactively
- Update architecture docs
```

✅ **Architecture documents**
```
Maintain:
- High-level architecture diagram
- Component responsibilities
- Data flow documentation
- Key design patterns
- Architectural constraints
```

## Workflow Anti-Patterns

### Anti-Pattern 1: The "Rewrite Loop"

**What it is:**
```
Attempt 1: Generate code → Doesn't work → Restart
Attempt 2: Generate code → Doesn't work → Restart
Attempt 3: Generate code → Doesn't work → Give up
```

**Why it fails:**
- Not learning from failures
- Not providing enough context
- Task too complex for single attempt

**Better approach:**
```
Attempt 1: Generate code → Doesn't work → Analyze what's wrong
Improve prompt with learnings → Generate improved code → Test
Iteratively fix issues → Success
```

### Anti-Pattern 2: The "Feature Factory"

**What it is:**
```
Ship feature A (unreviewed) →
Ship feature B (unreviewed) →
Ship feature C (unreviewed) →
Technical debt crisis →
Spend weeks refactoring
```

**Why it fails:**
- Quality decreases over time
- Debt compounds
- Eventually have to stop and clean up

**Better approach:**
```
Plan feature A → Implement → Review → Test → Refine → Ship →
Plan feature B → Implement → Review → Test → Refine → Ship →
Maintain quality at each step
```

### Anti-Pattern 3: The "AI Dependency"

**What it is:**
- Can't code without AI
- Don't understand generated code
- Lost core development skills
- Panic when AI is unavailable

**Why it fails:**
- You're no longer in control
- Can't debug effectively
- Can't make informed decisions
- Completely dependent on tool

**Better approach:**
```
Use AI as an assistant, not a replacement:
- Understand every line of code
- Maintain core development skills
- Use AI to accelerate, not replace thinking
- Could work without AI if needed (just slower)
```

## Prevention Strategies

### 1. Establish Team Standards

**Create team guidelines:**
```markdown
# AI-Assisted Development Standards

## Required:
- All AI-generated code must be reviewed
- Maximum 200 lines per AI generation
- Test after every change
- Document significant decisions
- No secrets in config files

## Process:
1. Plan before implementing
2. Implement in small chunks
3. Test continuously
4. Review before committing
5. Document decisions
```

### 2. Code Review Checklist

**For AI-generated code:**
```markdown
- [ ] Code is understood by reviewer
- [ ] Follows project conventions
- [ ] No obvious bugs or edge cases
- [ ] Security considerations addressed
- [ ] Performance is acceptable
- [ ] Tests are included and passing
- [ ] Documentation updated if needed
- [ ] No duplicated code
- [ ] Fits architectural patterns
- [ ] Error handling is appropriate
```

### 3. Regular Retrospectives

**Weekly team discussion:**
```
- What AI-generated code worked well?
- What problems did we encounter?
- What patterns should we adopt?
- What patterns should we avoid?
- How can we improve our process?
```

### 4. Monitoring and Metrics

**Track:**
- Code churn rate (AI vs. human)
- Bug rate in AI-generated code
- Time to fix AI-generated bugs
- Code review feedback patterns
- Technical debt accumulation

**Act on data:**
- Adjust processes based on metrics
- Identify problematic patterns
- Improve prompting strategies
- Refine team standards

## Conclusion

The most common mistake is treating AI as a replacement for thinking rather than a tool to amplify it. Successful AI-assisted development requires:

✅ **Understanding** - Read and understand all generated code
✅ **Incrementalism** - Small, tested changes
✅ **Planning** - Design before implementing
✅ **Testing** - Continuous verification
✅ **Review** - Critical evaluation of output
✅ **Documentation** - Capture decisions and learnings
✅ **Architecture** - Maintain design principles
✅ **Security** - Audit tools and protect secrets
✅ **Context management** - Efficient token usage
✅ **Team standards** - Shared practices and guidelines

**Remember:** AI is a powerful tool, but it's still a tool. You're the developer. You make the decisions. You're responsible for the quality. Use AI to go faster and do more, but never at the expense of understanding, quality, or maintainability.

The developers who succeed with AI are those who use it to amplify their skills, not replace them.
