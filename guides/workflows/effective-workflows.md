# Effective Workflows for LLM-Assisted Development

## Overview

The most successful LLM-assisted development follows structured workflows rather than ad-hoc prompting. This guide presents battle-tested workflows that maximize productivity while maintaining code quality.

## Core Workflow Principle

**The fundamental truth about LLM-assisted development:**

> **Small, iterative changes with verification beats large, monolithic implementations every time.**

**Why:**
- Large implementations become unwieldy quickly
- Harder to review and understand
- Debugging is nightmare
- Context window fills up
- Quality degrades
- Lost productivity when things go wrong

**Evidence:**
Studies and user reports consistently show that developers who break tasks into small, verified iterations are 3-5x more productive than those who try to implement large features in one go.

**Community consensus from r/ClaudeAI:**
> "I learned the hard way: Never let Claude write more than 200 lines without testing. Break everything into small, testable chunks." - Top-voted workflow advice

## The Core Workflow Pattern

### The Plan-Implement-Test-Review Cycle

```
┌─────────────┐
│    PLAN     │  Small, focused task
└──────┬──────┘
       │
       ▼
┌─────────────┐
│ IMPLEMENT   │  Write minimal code
└──────┬──────┘
       │
       ▼
┌─────────────┐
│    TEST     │  Verify it works
└──────┬──────┘
       │
       ▼
┌─────────────┐
│   REVIEW    │  Check quality
└──────┬──────┘
       │
       ▼
┌─────────────┐
│   COMMIT    │  Save progress
└──────┬──────┘
       │
       └──────> Next task (repeat cycle)
```

**Each cycle should take: 15-45 minutes**

**Why this works:**
- Constant verification prevents drift
- Small chunks are manageable
- Easy to debug problems
- Git history is meaningful
- Can rollback easily
- Momentum through small wins
- Context stays focused

### Cycle Size Guidelines

**Good cycle sizes:**
- ✅ Single function implementation
- ✅ One component with tests
- ✅ Single bug fix
- ✅ One API endpoint
- ✅ Small refactoring of single module

**Too large (split into smaller cycles):**
- ❌ Entire feature with multiple files
- ❌ Major refactoring across codebase
- ❌ Multiple related features
- ❌ "Implement the dashboard"
- ❌ Anything taking >1 hour

**Too small (can be combined):**
- ⚠️ Single line change (unless bug fix)
- ⚠️ Trivial formatting
- ⚠️ Adding one import

## ✅ DO: Effective Workflow Patterns

### 1. **Start Every Task with Planning**

**Pattern: Planning Session**

```
┌─ Planning Session (10-15 minutes) ─┐
│                                     │
│ 1. Understand the requirement      │
│ 2. Break into subtasks             │
│ 3. Identify dependencies           │
│ 4. Plan order of implementation    │
│ 5. Identify risks/unknowns         │
│                                     │
└─────────────────────────────────────┘
```

**Example:**

❌ **Bad approach:**
```
Human: "Implement user authentication"
Claude: [Generates 800 lines of code across 10 files]
```

✅ **Good approach:**
```
Human: "Help me plan implementing user authentication"

Claude: Let's break this down:

1. Core authentication (Session 1)
   - Password hashing utils
   - Login/logout logic
   - Tests

2. Middleware (Session 2)
   - Auth middleware
   - Route protection
   - Tests

3. Endpoints (Session 3)
   - POST /auth/login
   - POST /auth/logout
   - GET /auth/me
   - Tests

4. Integration (Session 4)
   - Connect to existing routes
   - Error handling
   - Integration tests

Let's start with #1...
```

**Benefits:**
- Clear roadmap
- Manageable chunks
- Can pause/resume easily
- Stakeholders can review plan
- Risks identified early

### 2. **Implement One Component at a Time**

**Pattern: Single-Responsibility Implementation**

**Rules:**
- Work on exactly one file/module/component per cycle
- Implement its tests immediately
- Verify before moving on
- Commit before next component

**Example workflow:**

```
Cycle 1: AuthService
├─ Implement: src/services/AuthService.ts
├─ Test: src/services/AuthService.test.ts
├─ Run tests: npm test AuthService
└─ Commit: "feat: implement AuthService"

Cycle 2: AuthMiddleware
├─ Implement: src/middleware/auth.ts
├─ Test: src/middleware/auth.test.ts
├─ Run tests: npm test auth.middleware
└─ Commit: "feat: implement auth middleware"

Cycle 3: Auth Routes
├─ Implement: src/routes/auth.ts
├─ Test: src/routes/auth.test.ts
├─ Run tests: npm test auth.routes
└─ Commit: "feat: implement auth routes"

Cycle 4: Integration
├─ Connect components
├─ Integration tests
├─ Run all tests: npm test
└─ Commit: "feat: integrate auth system"
```

**Why it works:**
- Each piece is simple enough to understand
- Tests verify each piece works
- Debugging is straightforward
- Can stop and resume easily
- Git history tells a story

**User testimonial:** "I stopped trying to implement entire features at once. Now it's one file, tests, commit. Repeat. 10x more productive."

### 3. **Test After Every Change**

**Pattern: Continuous Verification**

**Never accumulate untested code.**

**Workflow:**
```
1. Write code
   ↓
2. Write/update tests
   ↓
3. Run tests
   ↓
4. Tests pass?
   ├─ YES → Proceed to review
   └─ NO → Fix immediately
```

**Why immediate testing matters:**

**Scenario: Test after each change**
```
Change 1 → Test (pass) →
Change 2 → Test (pass) →
Change 3 → Test (fail) → Fix Change 3 immediately

Result: Know exactly what broke, fix in 2 minutes
```

**Scenario: Test after all changes**
```
Change 1 →
Change 2 →
Change 3 →
All done → Test (fail) → Now what broke???

Result: Debug 3 changes, fix in 30 minutes
```

**Evidence:**
15x faster debugging when testing continuously vs testing at the end.

### 4. **Review Before Committing**

**Pattern: Quality Gate**

**Review checklist:**
```markdown
Before committing, verify:
- [ ] Code works (tests pass)
- [ ] Code is clean and readable
- [ ] No debug code left (console.log, etc.)
- [ ] Error handling is appropriate
- [ ] Edge cases are handled
- [ ] Documentation/comments if needed
- [ ] Follows project conventions
- [ ] No obvious security issues
- [ ] Performance is acceptable
```

**Techniques:**

**Technique 1 - Self-review:**
```
Human: "Review the changes we just made. Check for:
- Bugs or edge cases
- Code quality issues
- Better approaches
- Potential problems"
```

**Technique 2 - Fresh eyes:**
Start new conversation, ask Claude to review the changes without context of creating them.

**Technique 3 - Specialized agent:**
Use a code review agent (see agent best practices guide).

**Why it works:**
- Catches issues before they become technical debt
- Maintains code quality
- Learning opportunity
- Prevents accumulating problems

### 5. **Commit Frequently with Clear Messages**

**Pattern: Meaningful Git History**

**Frequency: After each cycle (every 15-45 minutes)**

**Commit message format:**
```
<type>: <short description>

<optional longer description>
<optional notes about decisions made>

Example:
feat: implement JWT authentication service

- Added token generation and validation
- Includes refresh token support
- Token expiry set to 1 hour

Decided to use jsonwebtoken library over jose for broader compatibility
```

**Why frequent commits matter:**
- Easy rollback if something goes wrong
- Clear history of development
- Can pause and resume easily
- Safe to experiment (can always revert)
- Documents decision-making process

**User insight:** "Small, frequent commits saved my ass so many times. Break something? Revert. Lost? Check recent commits to remember what you were thinking."

### 6. **Use Branch-Per-Task for Larger Work**

**Pattern: Isolated Development**

**Workflow:**
```
main branch
    │
    ├─> feature/user-auth (new branch)
    │   ├─ Cycle 1: AuthService
    │   ├─ Cycle 2: AuthMiddleware
    │   ├─ Cycle 3: Auth Routes
    │   └─ Cycle 4: Integration
    │
    └─> (merge when feature complete)
```

**Benefits:**
- Main branch stays stable
- Can abandon work if needed
- Easy to share work-in-progress
- Clear scope for feature
- Enables code review before merge

**Antipattern:** Working directly on main branch for multi-day features.

### 7. **Take Breaks Between Cycles**

**Pattern: Sustainable Pace**

**Why breaks matter:**
- LLM-assisted development is cognitively intense
- Review quality degrades when tired
- Fresh perspective after break
- Prevents rubber-stamping Claude's suggestions

**Recommended rhythm:**
```
Cycle 1 (30 min)
  → Break (5 min)
Cycle 2 (30 min)
  → Break (5 min)
Cycle 3 (30 min)
  → Longer break (15 min)
Cycle 4 (30 min)
  → Break (5 min)
...
```

**After 4-5 cycles: Take 30+ minute break**

**Evidence:**
Developer productivity research shows that breaks improve both speed and quality. Tired developers make more mistakes and miss issues in review.

## ❌ DON'T: Workflow Anti-Patterns

### 1. **Don't Let Claude Write Hundreds of Lines Without Testing**

**The problem:**

```
❌ Bad:
Human: "Implement the entire user management system"
Claude: [Generates 1500 lines across 12 files]
Human: "Let's test it"
Result: 37 errors, 2 hours to debug, maybe easier to restart
```

**Why it fails:**
- Bugs compound across files
- Hard to know what's wrong where
- Context window full of broken code
- Lost time on debugging
- Often faster to restart than fix

**The 200-line rule:**
> "Never let Claude write more than 200 lines without running and testing the code."
> - r/ClaudeAI consensus

**Better:**
```
✅ Good:
Human: "Implement UserService with basic CRUD (one file)"
Claude: [Generates ~150 lines]
Human: [Tests it]
Result: Works! Moving on to next component.
```

### 2. **Don't Skip the Planning Phase**

**The problem:**
```
❌ Bad:
Human: "Build a dashboard"
Claude: "Let me start coding..."
[2 hours later]
Human: "This isn't what I wanted..."
```

**Why it fails:**
- No shared understanding
- Wasted effort on wrong approach
- Have to start over
- Frustrating for everyone

**Better:**
```
✅ Good:
Human: "Help me plan a dashboard feature"
Claude: "What data should it show? Who's the user? What actions?"
[10 minute planning discussion]
Claude: "Here's my implementation plan: ..."
Human: "Looks good, let's implement phase 1"
[Efficient implementation]
```

**Time investment:**
- Planning: 10-15 minutes
- Time saved: Hours of rework

### 3. **Don't Accumulate Technical Debt**

**The problem:**
```
❌ Bad pattern:
Cycle 1: "I'll fix that later"
Cycle 2: "Another small issue, I'll batch them"
Cycle 3: "Okay, lots of TODOs now..."
Cycle 4: "This is getting unwieldy..."
Result: Technical debt crisis
```

**Why it fails:**
- TODOs never get done
- Debt compounds
- Future work builds on shaky foundation
- Eventually have to stop and refactor

**Better:**
```
✅ Good pattern:
Cycle 1: Notice issue → Fix immediately (2 minutes)
Cycle 2: Notice issue → Fix immediately (3 minutes)
Cycle 3: Notice issue → Fix immediately (2 minutes)
Result: Clean code, no debt
```

**The rule: Fix it now if it takes <5 minutes**

### 4. **Don't Trust Without Verifying**

**The problem:**
```
❌ Bad:
Claude: "Here's the implementation"
Human: "Looks good!" [Commits without testing]
[Later] "Wait, this doesn't work..."
```

**Why it fails:**
- LLMs make mistakes
- Subtle bugs slip through
- Assumptions may be wrong
- Edge cases missed

**Better:**
```
✅ Good:
Claude: "Here's the implementation"
Human: [Reads code, runs tests, tries edge cases]
Human: "Found an issue with null handling"
Claude: [Fixes it]
Human: [Tests again, verifies]
Human: "Now it's good!" [Commits]
```

**Trust but verify: Always test before committing**

### 5. **Don't Work in One Mega-Conversation**

**The problem:**
```
❌ Bad:
Day 1: Start conversation, implement feature A
Day 2: Same conversation, implement feature B
Day 3: Same conversation, implement feature C
Day 4: Same conversation, now it's confused...

Context: 150K tokens of mixed history
Result: Claude mixing up features, hallucinating earlier decisions
```

**Why it fails:**
- Context window fills with irrelevant history
- Model confuses different features
- Performance degrades
- Can't find relevant information

**Better:**
```
✅ Good:
Day 1: Conversation 1 → Feature A → Summary document
Day 2: Conversation 2 → Feature B → Summary document
Day 3: Conversation 3 → Feature C → Summary document
Day 4: Fresh context, reference summaries as needed

Each conversation: Focused, clear context
Result: Consistent, high-quality output
```

**The conversation size limit:**
> Start fresh after 50 exchanges or 3-4 hours of work

### 6. **Don't Skip Code Review**

**The problem:**
```
❌ Bad:
Claude generates code → Commit immediately
Claude generates code → Commit immediately
[Repeat 10x]
Result: Low-quality code, bugs, inconsistencies
```

**Why it fails:**
- LLMs make mistakes
- May not follow all conventions
- Subtle bugs creep in
- Code quality degrades

**Better:**
```
✅ Good:
Claude generates code → Human reviews → Fix issues → Test → Commit
Claude generates code → Human reviews → Fix issues → Test → Commit

Optional: Periodic comprehensive review with fresh Claude conversation
```

**Code review should take: 20-30% of implementation time**

## Advanced Workflows for Context Window Management

Context window limits are **the single most disruptive factor** in LLM-assisted development. For complex, multi-day tasks, implementing proper context management strategies is essential.

### Dev Docs Pattern for Complex Tasks

For tasks exceeding 2 hours or spanning multiple sessions, the dev docs pattern provides a proven solution. See the comprehensive guide:

**📖 [Dev Docs Pattern Guide](../context-window-management/dev-docs-pattern.md)**

This three-file system (plan.md, context.md, tasks.md) enables lossless context preservation across sessions and provides instant resume capability.

**Key benefits:**
- Survives all context resets indefinitely
- 30-second context restoration
- Git-trackable and shareable
- Zero infrastructure required
- Proven in 300k LOC production rewrite

**Quick reference:** Use dev docs for tasks >2 hours, skip for quick fixes <30 minutes.

### Comparison of Context Management Approaches

Multiple strategies exist for managing context window limits, each with different strengths and use cases. For a comprehensive comparison of all 7 major approaches:

**📖 [Context Window Management Patterns Comparison](../context-window-management/different-patterns-for-working-around-context-window-limits.md)**

This guide compares:
- Dev Docs Pattern (file-based)
- CLAUDE.md/Static Files
- Built-in Session Commands
- MCP External Memory
- Vector DB/RAG
- Sub-agent Architecture
- Chunking/Summarization

**Evidence-based recommendation for 80% of developers:** Dev Docs Pattern + CLAUDE.md + Session Commands provides optimal results with minimal complexity.

## PM2 for Backend Debugging

### The Problem with Backend Logs

**When running multiple backend services:**
- Claude can't see logs while services run
- Manual copy-paste of logs is tedious
- Services don't auto-restart on crashes
- Difficult to debug real-time issues

**Old approach:**
```
1. Run service manually
2. Reproduce error
3. Copy log output
4. Paste into Claude
5. Get analysis
6. Make fix
7. Restart service manually
8. Repeat
```

### The Solution: PM2 Process Manager

**PM2 gives you:**
- All services run as managed processes
- Each service has its own log file
- Automatic restarts on crashes
- Real-time monitoring
- Easy log access for Claude

**Setup:**

```javascript
// ecosystem.config.js
module.exports = {
  apps: [
    {
      name: 'api-gateway',
      script: 'npm',
      args: 'start',
      cwd: './gateway',
      error_file: './gateway/logs/error.log',
      out_file: './gateway/logs/out.log',
    },
    {
      name: 'auth-service',
      script: 'npm',
      args: 'start',
      cwd: './auth',
      error_file: './auth/logs/error.log',
      out_file: './auth/logs/out.log',
    },
    {
      name: 'email-service',
      script: 'npm',
      args: 'start',
      cwd: './email',
      error_file: './email/logs/error.log',
      out_file: './email/logs/out.log',
    },
    // ... more services
  ]
};
```

**Start all services:**
```bash
pm2 start ecosystem.config.js
```

**New debugging workflow:**

```
Human: "The email service is throwing errors"

Claude: [Runs pm2 logs email --lines 200]
Claude: [Analyzes logs] "I see the issue - SMTP timeout on line 45..."
Claude: [Makes fix to email service]
Claude: [Runs pm2 restart email]
Claude: [Checks logs again] "Service restarted successfully, monitoring for errors..."

Result: Autonomous debugging without manual log management
```

**PM2 Commands Claude Can Use:**
```bash
pm2 logs <service-name> --lines 200   # Read recent logs
pm2 restart <service-name>             # Restart service
pm2 stop <service-name>                # Stop service
pm2 start <service-name>               # Start service
pm2 status                             # Show all services
pm2 monit                              # Real-time monitoring
```

**Benefits:**
- Claude can autonomously debug backend issues
- No manual log fetching
- Automatic service management
- Easy monitoring of all services
- Real-time log access

**Caveat:**
Hot reload doesn't work well with PM2, so consider running frontend separately with normal dev server if you need hot module replacement.

**Community insight:**
> "PM2 changed backend debugging completely. Claude can now read logs, identify issues, make fixes, and restart services autonomously. Night and day difference." - Production user

## Complete Workflow Examples

### Workflow 1: Large Feature with Dev Docs

**Scenario:** Implement complete user authentication system

**Phase 1: Planning with Dev Docs (15 minutes)**
```
Human: "Help me plan implementing user authentication"
[Use planning mode]

Claude: [Creates comprehensive plan]