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

## Advanced: Dev Docs System for Complex Tasks

### The Problem: Claude Loses Track

**Common scenario:**
```
Hour 1: Start implementing feature, everything on track
Hour 2: Making good progress, handling edge cases
Hour 3: Wait, what were we implementing again?
Result: Realized Claude went on a tangent and lost the original plan
```

**Why it happens:**
- Claude is like an "extremely confident junior dev with extreme amnesia"
- Long conversations accumulate context
- Original plan gets buried or forgotten
- Auto-compaction wipes context
- Easy to drift from original intent

### The Solution: Persistent Dev Docs

**Create a documentation trail for every large task:**

```bash
dev/
└── active/
    └── [task-name]/
        ├── [task-name]-plan.md        # The accepted plan
        ├── [task-name]-context.md     # Key files, decisions, integrations
        └── [task-name]-tasks.md       # Checklist of work items
```

### Starting Large Tasks

**Workflow:**

**1. Enter Planning Mode**
Even if you'll write the plan to markdown later, planning mode helps Claude:
- Gather context more efficiently
- Analyze project structure thoroughly
- Create comprehensive plans
- Research the codebase effectively

**2. Review the Plan Thoroughly**
- Take time to understand every part
- Catch misunderstandings early
- Verify Claude understood the requirements
- Identify missing pieces

**3. Create Dev Docs Structure**
```bash
mkdir -p ~/git/project/dev/active/[task-name]/
```

**4. Generate Three Core Documents**

Using a custom slash command or directly with Claude:

**`[task-name]-plan.md`:**
```markdown
# Feature: User Authentication System

## Executive Summary
Implement JWT-based authentication with refresh tokens

## Phases
### Phase 1: Core Auth Service
- Password hashing utilities
- Token generation and validation
- Refresh token support

### Phase 2: Middleware & Protection
- Auth middleware
- Route protection
- Permission checking

### Phase 3: API Endpoints
- POST /auth/login
- POST /auth/logout
- POST /auth/refresh
- GET /auth/me

### Phase 4: Integration
- Connect to existing routes
- Error handling
- Integration tests

## Risks & Mitigations
- Risk: Token expiry edge cases
  Mitigation: Comprehensive test coverage

## Success Metrics
- All tests passing
- Auth flow works end-to-end
- Performance within 200ms

## Timeline
Estimated: 3-4 days (with testing)
```

**`[task-name]-context.md`:**
```markdown
# Context: User Authentication

## Key Files
- src/services/AuthService.ts - Core auth logic
- src/middleware/auth.ts - Route protection
- src/routes/auth.ts - Auth endpoints
- src/models/User.ts - User model

## Integration Points
- Existing user management system
- Database: PostgreSQL via Prisma
- Email service for verification

## Key Decisions
- Decision: Use JWT over sessions
  Reason: Better for distributed systems
  Made: 2024-01-15

- Decision: 1-hour token expiry
  Reason: Security vs UX balance
  Made: 2024-01-15

## Dependencies
- jsonwebtoken library
- bcrypt for password hashing

**Last Updated:** 2024-01-15 14:30
```

**`[task-name]-tasks.md`:**
```markdown
# Tasks: User Authentication

## Phase 1: Core Auth Service
- [ ] Implement password hashing utility
- [ ] Implement token generation
- [ ] Implement token validation
- [ ] Add refresh token support
- [ ] Write unit tests

## Phase 2: Middleware
- [ ] Create auth middleware
- [ ] Add route protection
- [ ] Write middleware tests

## Phase 3: Endpoints
- [ ] POST /auth/login
- [ ] POST /auth/logout
- [ ] POST /auth/refresh
- [ ] GET /auth/me
- [ ] Write endpoint tests

## Phase 4: Integration
- [ ] Connect to existing routes
- [ ] Error handling
- [ ] Integration tests
- [ ] Documentation

**Last Updated:** 2024-01-15 14:30
```

**5. Implement with Constant Reference**

Throughout implementation:
- Reference dev docs frequently
- Update tasks as completed (immediately!)
- Add new context/decisions as they emerge
- Track next steps before running low on context

### Continuing After Context Loss

**When starting a fresh conversation:**

```
Human: "Continue working on user authentication.
Read these files:
- dev/active/user-auth/user-auth-plan.md
- dev/active/user-auth/user-auth-context.md
- dev/active/user-auth/user-auth-tasks.md

Pick up where we left off."

Claude: [Reads files, understands full context, continues seamlessly]
```

### Updating Dev Docs Before Compaction

**When context is running low:**

Create a slash command like `/update-dev-docs`:

```markdown
# .claude/commands/update-dev-docs.md
Review the current task and update the dev docs:

1. Mark completed tasks in [task-name]-tasks.md
2. Add any new tasks discovered during implementation
3. Update [task-name]-context.md with:
   - New decisions made
   - New integration points discovered
   - Next steps for resuming work
4. Update "Last Updated" timestamps

Be specific about next steps so work can resume smoothly.
```

**Result:** Clean handoff to next conversation, no lost context.

### Benefits of Dev Docs System

**Before dev docs:**
- Claude loses track during long tasks
- Have to restart when context is lost
- Difficult to resume after breaks
- Tangents derail the original plan
- Auto-compaction wipes critical context

**After dev docs:**
- Clear plan always available
- Easy resume after breaks or compaction
- Tangents get caught (compare to plan)
- All decisions documented
- Smooth handoffs between conversations

**Community insight:**
> "The dev docs system, out of everything besides skills, has made the most impact on results. Claude is like an extremely confident junior dev with extreme amnesia - this system addresses that perfectly." - 6-month production user

### Critical Importance: Context Window Limits are the Hidden Bottleneck

**The reality most developers discover too late:**

Context window limits are not just a technical constraint—they are **the single most disruptive factor** in LLM-assisted development workflows. Studies show developers lose 30-60 minutes per session when context resets without proper management systems.

**Why this matters more than you think:**

```
Without proper context management:
- Every context reset = complete information loss
- Implementation decisions forgotten
- Key file purposes rediscovered each time
- Task progress reset to zero
- Technical constraints re-explained repeatedly
- Hours wasted on "catching Claude back up"
```

**The Context Rot Problem:**

Research has uncovered "context rot": as tokens in the context window increase, the model's ability to accurately recall information **decreases**. This characteristic emerges across all models, making context a finite resource with diminishing marginal returns.

Even Claude 3.7 Sonnet's 200,000 token context window faces the "lost-in-the-middle effect"—where LLMs weigh the beginning and end of prompts more heavily due to primacy and recency bias, causing important middle context to be undervalued.

**Real-world impact:**

> "Context loss between sessions creates significant workflow disruptions for complex development projects, forcing developers to repeatedly re-explain everything when opening new windows or tabs." - GitHub Issue #2954, Claude Code

> "Developers are implementing sophisticated external memory systems, multi-agent architectures, and complex session logging just to maintain basic project continuity." - Context Sync Product Hunt Discussion

### Deep Dive: The Dev Docs Pattern in Detail

The dev docs pattern from [claude-code-infrastructure-showcase](https://github.com/diet103/claude-code-infrastructure-showcase) provides the most comprehensive solution to context persistence we've found. Here's why it works:

#### The Three-File Architecture

**Critical Design Principle:** Each file serves a distinct purpose in the context preservation lifecycle.

**File Location:** `dev/active/[task-name]/`

**1. [task-name]-plan.md - The Strategic Blueprint**

**Purpose:** High-level strategy that rarely changes

**Critical Contents:**
```markdown
# Executive Summary
One-paragraph overview of what we're building and why

# Current State Analysis
- What exists now
- What's broken/missing
- Why current approach isn't sufficient

# Future State Vision
- What we want to achieve
- How it solves the problem
- Success looks like...

# Implementation Phases
## Phase 1: [Name]
**Objective:** Clear goal
**Tasks:**
- Specific, actionable task 1
- Specific, actionable task 2
**Acceptance Criteria:**
- Measurable success indicator
**Dependencies:** What must be complete first

## Phase 2: [Name]
[Repeat structure]

# Risk Assessment
- **Risk:** Specific concern
- **Likelihood:** High/Medium/Low
- **Impact:** High/Medium/Low
- **Mitigation:** Concrete approach

# Success Metrics
- How we measure completion
- Performance benchmarks
- Quality gates

# Timeline Estimates
Phase 1: X days
Phase 2: Y days
Total: Z days
```

**When to update:** Only when scope changes significantly or new phases emerge

**Why it works:** Provides stable, high-level direction that Claude can reference without requiring updates

---

**2. [task-name]-context.md - The Living State Document**

**Purpose:** Current working state and decision log

**Critical Structure:**
```markdown
# SESSION PROGRESS (⚠️ UPDATE THIS FREQUENTLY)

## Last Updated: [Timestamp]

## Completed Work
- ✅ Implemented UserAuthService.ts with JWT generation
- ✅ Created auth middleware with token validation
- ✅ Added error handling for expired tokens

## Currently Working On
🔄 Implementing refresh token endpoint
- File: src/routes/auth.ts
- Current status: 60% complete
- Next step: Add refresh token validation logic

## Immediate Next Steps
1. Complete refresh token validation
2. Add integration tests for refresh flow
3. Update auth documentation

## Blockers / Issues
- ⚠️ Need clarification on token rotation policy
- 🔴 TypeScript error in AuthMiddleware.ts line 45 (priority)

---

# Key Files and Their Purposes

## Core Authentication Files
- **src/services/AuthService.ts**: JWT generation, validation, refresh
  - Why important: Central auth logic, used by all protected routes
  - Key functions: generateToken(), validateToken(), refreshToken()

- **src/middleware/auth.ts**: Request authentication
  - Why important: Guards all protected API endpoints
  - Integration: Applied to routes in src/routes/index.ts

- **src/routes/auth.ts**: Auth endpoints
  - Why important: Public API for authentication
  - Endpoints: /login, /logout, /refresh, /me

## Related Configuration
- **src/config/jwt.ts**: JWT configuration
- **src/types/auth.ts**: Auth TypeScript types

---

# Important Architectural Decisions

## Decision: JWT vs Sessions (2024-01-15)
**Chosen:** JWT tokens
**Reasoning:**
- Better for distributed systems
- No server-side session storage needed
- Easier horizontal scaling
**Trade-offs:**
- Can't revoke tokens before expiry
- Slightly larger request size

## Decision: 1-hour token expiry (2024-01-15)
**Chosen:** 1 hour access token, 7 day refresh token
**Reasoning:**
- Security: Limits exposure if token stolen
- UX: Refresh tokens prevent constant re-login
**Implementation:** Set in src/config/jwt.ts

## Decision: httpOnly cookies vs localStorage (2024-01-16)
**Chosen:** httpOnly cookies for refresh tokens
**Reasoning:**
- XSS protection: JavaScript can't access
- CSRF protection via SameSite attribute
**Trade-offs:**
- More complex for mobile apps
- Requires CORS configuration

---

# Technical Constraints Discovered

- Database: PostgreSQL via Prisma ORM
  - Must use repository pattern for consistency
  - All queries must go through UserRepository

- Error Handling: All errors must use Sentry
  - Pattern: try-catch with Sentry.captureException()
  - Location: Established in src/utils/errorHandler.ts

- Testing: Jest + Supertest required
  - All endpoints need integration tests
  - Minimum 80% coverage for auth code

---

# Quick Resume Instructions

**To continue this work:**
1. Read this context file completely
2. Review current progress in SESSION PROGRESS section
3. Check tasks.md for what's marked complete
4. Start with "Immediate Next Steps"
5. Update this file after each major milestone
```

**Update Frequency:** CRITICAL—after every major milestone, completion, or discovery

**Why it works:**
- SESSION PROGRESS provides instant orientation
- Decisions are documented with reasoning (prevents rework)
- Constraints are explicit (prevents violations)
- Resume instructions eliminate guesswork

---

**3. [task-name]-tasks.md - The Action Checklist**

**Purpose:** Granular, actionable task tracking

**Critical Format:**
```markdown
# Tasks: User Authentication System

## Status Legend
- ✅ Complete
- 🟡 In Progress
- ⏳ Blocked
- ⬜ Not Started

---

## Phase 1: Core Auth Service

### Password Hashing
- ✅ Install bcrypt dependency
- ✅ Implement hashPassword() utility
- ✅ Implement comparePassword() utility
- ✅ Add unit tests for hashing functions

**Acceptance Criteria:**
- Passwords hashed with bcrypt rounds=10
- Comparison function returns boolean correctly
- Tests cover valid/invalid passwords
- Error handling for malformed input

---

### JWT Token Generation
- ✅ Install jsonwebtoken dependency
- ✅ Create JWT configuration in src/config/jwt.ts
- 🟡 Implement generateToken() in AuthService.ts
- ⬜ Add token expiry logic
- ⬜ Unit tests for token generation

**Acceptance Criteria:**
- Tokens signed with HS256 algorithm
- Payload includes userId, email, roles
- Expiry set to 1 hour
- Secret loaded from environment variable
- Tests verify payload structure

**Current Status (generateToken):**
- Basic implementation complete
- Need to add: refresh token generation
- Next: Implement token expiry in 30 minutes

---

### Token Validation
- ⬜ Implement validateToken() in AuthService.ts
- ⬜ Add error handling for expired tokens
- ⬜ Add error handling for invalid signatures
- ⬜ Unit tests for validation

**Acceptance Criteria:**
- Validates signature correctly
- Detects expired tokens
- Returns decoded payload or null
- Logs validation failures
- Tests cover all error cases

**Dependencies:**
- Requires: generateToken() complete

---

## Phase 2: Middleware & Protection

### Auth Middleware
- ⬜ Create src/middleware/auth.ts
- ⬜ Implement authenticateRequest() middleware
- ⬜ Extract token from Authorization header
- ⬜ Validate token and attach user to request
- ⬜ Handle missing/invalid token cases
- ⬜ Integration tests with mock routes

**Acceptance Criteria:**
- Middleware extracts Bearer token from header
- Adds req.user object on success
- Returns 401 for missing token
- Returns 403 for invalid token
- Tests verify all paths

---

## Phase 3: API Endpoints

### POST /auth/login
- ⬜ Create route handler in src/routes/auth.ts
- ⬜ Validate email/password from request
- ⬜ Query user from database
- ⬜ Compare password hash
- ⬜ Generate tokens on success
- ⬜ Return tokens in response
- ⬜ Integration tests

**Acceptance Criteria:**
- Accepts JSON: {email, password}
- Returns 200 with {accessToken, refreshToken}
- Returns 401 for invalid credentials
- Returns 400 for missing fields
- Logs failed login attempts
- Tests cover success + error cases

---

### GET /auth/me
- ⬜ Create protected route handler
- ⬜ Apply auth middleware
- ⬜ Return current user data
- ⬜ Integration tests

**Acceptance Criteria:**
- Requires valid access token
- Returns user object: {id, email, roles}
- Returns 401 if not authenticated
- Tests verify token requirement

---

## Phase 4: Integration & Polish

### Integration Testing
- ⬜ End-to-end auth flow test
- ⬜ Token refresh flow test
- ⬜ Protected route access test
- ⬜ Error scenario tests

### Documentation
- ⬜ API documentation for auth endpoints
- ⬜ Update README with auth setup
- ⬜ Add JSDoc comments to AuthService

### Performance
- ⬜ Benchmark token generation speed
- ⬜ Verify database query performance
- ⬜ Check middleware overhead

---

# Notes

## Discovered During Implementation
- 2024-01-16: Refresh tokens need separate table for revocation support
- 2024-01-16: Consider rate limiting on /auth/login endpoint

## Future Enhancements (Post-MVP)
- Two-factor authentication
- OAuth integration (Google, GitHub)
- Password reset flow
- Email verification
```

**Update Frequency:** Mark tasks complete IMMEDIATELY as you finish them

**Why it works:**
- Checkbox format is visually scannable
- Acceptance criteria make "done" unambiguous
- Status indicators show progress at a glance
- Notes capture discoveries without cluttering the plan
- Dependencies prevent working out of order

### Pro Tips

**1. Limit Scope Per Cycle**
```
Instead of:
"Implement the entire plan"

Do this:
"Implement only Phase 1, sections 1-2. We'll do Phase 1 section 3 next."
```

**Benefit:** Get to review code between each set of tasks, catch issues early.

**2. Have Claude Review Its Own Code**
Between phases, launch a code review agent:
- Catches critical errors early
- Identifies missing implementations
- Finds inconsistent code
- Spots security flaws

**3. Update Context During Implementation**
Don't wait until end to document:
```
Human: "We just decided to use httpOnly cookies for refresh tokens.
Add this decision to the context file."
```

**4. Create Slash Commands**
Make the workflow repeatable:
- `/dev-docs` - Create initial dev docs from plan
- `/update-dev-docs` - Update before compaction
- `/review-progress` - Review tasks and next steps

**5. When to Use Dev Docs vs When to Skip**

**Use dev docs for:**
- ✅ Tasks expected to take >2 hours
- ✅ Work spanning multiple sessions
- ✅ Complex features with many moving parts
- ✅ Large system refactoring
- ✅ Multi-day projects
- ✅ Anything that will hit context limits

**Skip dev docs for:**
- ❌ Simple bug fixes (< 30 minutes)
- ❌ Single-file changes
- ❌ Quick updates or tweaks
- ❌ Exploratory work without clear scope

**The 2-hour rule:** If a task might exceed 2 hours or span sessions, create dev docs. Time invested upfront saves hours later.

## Comprehensive Comparison: Context Window Management Approaches

### Overview of Available Strategies

Based on extensive research and real-world usage patterns, here are all major approaches to managing context window limits in LLM-assisted development:

| Approach | Type | Complexity | Effectiveness | Cost |
|----------|------|------------|---------------|------|
| Dev Docs Pattern | File-based | Medium | ⭐⭐⭐⭐⭐ | Free |
| CLAUDE.md/Static Files | File-based | Low | ⭐⭐⭐ | Free |
| Built-in Session Commands | Native | Low | ⭐⭐ | Free |
| MCP External Memory | Tool-based | High | ⭐⭐⭐⭐ | Free-Paid |
| Vector DB/RAG | Infrastructure | Very High | ⭐⭐⭐⭐ | Paid |
| Sub-agent Architecture | Architectural | High | ⭐⭐⭐⭐ | Free |
| Chunking/Summarization | Technique | Low-Medium | ⭐⭐ | Free |

---

### 1. Dev Docs Pattern (Three-File Structure)

**What it is:** Version-controlled markdown files (plan.md, context.md, tasks.md) stored in `dev/active/[task-name]/`

**Strengths:**
- ⭐⭐⭐⭐⭐ **Persistence:** Survives all context resets indefinitely
- ⭐⭐⭐⭐⭐ **Completeness:** Captures strategy, state, and tasks separately
- ⭐⭐⭐⭐⭐ **Version control:** Git-trackable, shareable with team
- ⭐⭐⭐⭐⭐ **Zero infrastructure:** Just markdown files
- ⭐⭐⭐⭐⭐ **Human readable:** You can read and edit manually
- ⭐⭐⭐⭐ **Resume speed:** 30 seconds to full context restoration
- ⭐⭐⭐⭐ **Scalability:** Works for 1-week or 6-month projects

**Weaknesses:**
- ⚠️ Requires discipline to update context.md frequently
- ⚠️ Manual process (though can be slash-command automated)
- ⚠️ Doesn't auto-update (relies on human/Claude to maintain)
- ⚠️ Can become stale if not updated during implementation

**Best for:**
- Complex multi-day/multi-week features
- Team projects requiring handoffs
- Production work with high stakes
- Projects with evolving requirements

**Real-world effectiveness:**
> "Read 3 files and resume instantly with full context; hours saved per reset" - claude-code-infrastructure-showcase

**Evidence:** Proven in 300k LOC production rewrite over 6 months

**Ranking: 🥇 #1 Overall for Complex Tasks**

---

### 2. CLAUDE.md / Implementation Strategy Files

**What it is:** Static markdown files at project root containing rules, patterns, and architectural decisions

**Strengths:**
- ⭐⭐⭐⭐ **Simple:** Single file, easy to set up
- ⭐⭐⭐⭐ **Persistent:** Survives all sessions
- ⭐⭐⭐⭐ **Shareable:** Team members benefit equally
- ⭐⭐⭐⭐ **Low maintenance:** Set and mostly forget
- ⭐⭐⭐ **Auto-loaded:** Claude reads on session start

**Weaknesses:**
- ⚠️ Static—doesn't capture current task state
- ⚠️ Becomes bloated if trying to do too much
- ⚠️ No task progress tracking
- ⚠️ Doesn't help with "where was I?" after context reset
- ⚠️ Can't track session-specific decisions

**Best for:**
- Project-level conventions and patterns
- Architectural guidelines
- Coding standards
- Tool configurations
- Things that NEVER change mid-task

**How it compares to Dev Docs:**
- **Different purpose:** CLAUDE.md = "how to write code in this project" while Dev Docs = "what are we currently building and where are we"
- **Complementary:** Use BOTH together for best results
- **CLAUDE.md covers:** Patterns, conventions, architecture
- **Dev Docs covers:** Current task, progress, decisions specific to current work

**Evidence:**
> "Implementation strategy documents save thousands of tokens in subsequent sessions—instead of re-analyzing the entire codebase, the agent simply reads its own strategy document." - Context Engineering Research

**Ranking: 🥈 #2 for Project-Level Context, Not a Replacement for Dev Docs**

---

### 3. Built-in Session Commands (/compact, /clear, /resume)

**What it is:** Native Claude Code commands for managing conversation state

**Available commands:**
- `/compact` - Summarize conversation and start fresh with summary
- `/clear` - Wipe context completely
- `/resume [session-id]` - Continue previous conversation
- `--continue` - Resume most recent conversation

**Strengths:**
- ⭐⭐⭐⭐⭐ **Native:** Built into Claude Code
- ⭐⭐⭐⭐⭐ **Zero setup:** Works immediately
- ⭐⭐⭐⭐ **Fast:** Instant execution
- ⭐⭐⭐ **Summarization:** /compact creates condensed context

**Weaknesses:**
- ⚠️⚠️ **Lossy:** Summarization loses nuance and details
- ⚠️⚠️ **Ephemeral:** Resume only works for recent sessions
- ⚠️⚠️ **No long-term persistence:** Can't resume from weeks ago
- ⚠️⚠️ **Limited control:** Can't choose what to keep
- ⚠️ **Fragile:** Summaries may miss critical details

**Best for:**
- Quick context cleanup during active sessions
- Reducing token usage when context is filling up
- Resuming very recent work (same day)

**How it compares to Dev Docs:**
- **Timeframe:** Session commands = hours, Dev Docs = weeks/months
- **Reliability:** Session commands = lossy, Dev Docs = lossless
- **Control:** Session commands = automatic, Dev Docs = explicit
- **Use together:** /compact when context fills + Dev Docs for persistence

**Evidence:**
> "/clear should be used as often as possible, ideally whenever you finish a task, to reduce unnecessary token usage" - Claude Code Session Management Guide

**Ranking: 🥉 #3 for Short-Term Context Management, Not Suitable for Complex Projects**

---

### 4. MCP-Based External Memory Systems

**What it is:** Model Context Protocol servers that provide persistent memory across sessions

**Examples:**
- **Context Sync:** Syncs conversations, decisions, and project state across tools
- **Task Orchestrator:** Persistent external memory with sub-agent architecture
- **Custom MCP servers:** Project-specific memory implementations

**Strengths:**
- ⭐⭐⭐⭐⭐ **Automatic:** Memory persists without manual updates
- ⭐⭐⭐⭐⭐ **Cross-tool:** Works across Claude Desktop, Cursor, etc.
- ⭐⭐⭐⭐ **Queryable:** AI can search past context
- ⭐⭐⭐⭐ **Scalable:** Handles massive context accumulation
- ⭐⭐⭐⭐ **Token efficient:** Up to 90% token reduction (Task Orchestrator)

**Weaknesses:**
- ⚠️⚠️ **Setup complexity:** Requires MCP server installation
- ⚠️⚠️ **Infrastructure:** External dependency
- ⚠️⚠️ **Potential cost:** Some solutions are paid
- ⚠️ **Black box:** Less transparent than markdown files
- ⚠️ **Vendor lock-in:** Tied to specific MCP implementation

**Best for:**
- Teams needing cross-tool context sharing
- Projects with dozens of concurrent tasks
- Organizations with dedicated DevOps resources
- Workflows requiring automatic context capture

**How it compares to Dev Docs:**
- **Automation:** MCP = automatic, Dev Docs = semi-manual
- **Transparency:** MCP = opaque, Dev Docs = human-readable
- **Reliability:** MCP = depends on server, Dev Docs = just files
- **Setup:** MCP = complex, Dev Docs = create 3 files
- **Cost:** MCP = can be paid, Dev Docs = free

**Evidence:**
> "Task Orchestrator implements patterns recommended in Anthropic's context engineering research: sub-agent architectures, compaction through summarization, just-in-time context loading, and persistent external memory. Scaling to 50+ tasks with up to 90% token reduction" - Task Orchestrator GitHub

**Ranking: 🎖️ #4 for Large Teams/Organizations, Overkill for Solo Developers**

---

### 5. Vector Database / RAG (Retrieval-Augmented Generation)

**What it is:** Store embeddings of code snippets and project data in vector DB, query to retrieve relevant info

**Technical approach:**
- Chunk codebase into segments
- Generate embeddings for each chunk
- Store in vector database (Pinecone, Weaviate, etc.)
- Query at session start to retrieve relevant context

**Strengths:**
- ⭐⭐⭐⭐⭐ **Semantic search:** Finds conceptually relevant code
- ⭐⭐⭐⭐ **Massive scale:** Handles million+ line codebases
- ⭐⭐⭐⭐ **Automatic:** No manual documentation needed
- ⭐⭐⭐⭐ **Just-in-time:** Loads only relevant context

**Weaknesses:**
- ⚠️⚠️⚠️ **Extreme complexity:** Requires vector DB infrastructure
- ⚠️⚠️⚠️ **High cost:** Vector DB hosting + embedding API calls
- ⚠️⚠️ **Maintenance:** Must re-index as code changes
- ⚠️⚠️ **Imperfect retrieval:** May miss relevant context
- ⚠️ **No task state:** Retrieves code, not "what I'm working on"

**Best for:**
- Gigantic codebases (500k+ LOC)
- Q&A about unfamiliar codebases
- Enterprise organizations
- Research/exploration tasks

**How it compares to Dev Docs:**
- **Purpose:** RAG = code discovery, Dev Docs = task continuity
- **Use case:** RAG = "what does this code do?", Dev Docs = "resume my work"
- **Complementary:** Can use BOTH—RAG for codebase knowledge, Dev Docs for task state
- **Cost:** RAG = $$$ infrastructure, Dev Docs = $0

**Evidence:**
> "Using a vector database to store embeddings of previous coding snippets and relevant project data, then query it to retrieve relevant information when starting new sessions" - Context Engineering Patterns

**Ranking: 🏅 #5 for Code Discovery in Massive Codebases, Not for Task Continuity**

---

### 6. Sub-agent Architecture

**What it is:** Delegate tasks to specialized sub-agents, each with isolated context windows

**Pattern:**
- Main agent orchestrates work
- Sub-agents handle specific tasks (testing, reviews, implementation)
- Each sub-agent gets fresh context window
- Communication via structured handoffs (JSON schemas)

**Strengths:**
- ⭐⭐⭐⭐ **Context isolation:** Each task gets clean context
- ⭐⭐⭐⭐ **Specialization:** Agents optimized for specific tasks
- ⭐⭐⭐⭐ **Parallel work:** Multiple tasks simultaneously
- ⭐⭐⭐ **Scalable:** Add agents as needed

**Weaknesses:**
- ⚠️⚠️ **Coordination overhead:** Handoffs can lose context
- ⚠️⚠️ **Complexity:** Requires orchestration logic
- ⚠️⚠️ **Structured data required:** Free-text handoffs lose information
- ⚠️ **Setup cost:** Define agents and workflows

**Best for:**
- Large teams with distinct roles
- Workflows with clear task boundaries
- Projects requiring concurrent work streams
- Code review + implementation + testing pipelines

**How it compares to Dev Docs:**
- **Scope:** Sub-agents = workflow architecture, Dev Docs = task persistence
- **Complexity:** Sub-agents = high, Dev Docs = low
- **Complementary:** Sub-agents can USE dev docs for handoffs
- **Best practice:** Sub-agents write dev docs for task continuity

**Evidence:**
> "Handoffs should be treated like a public API using JSON Schema-based structured outputs. Free-text handoffs are the main source of context loss." - Best Practices for Multi-Agent Orchestration

**Ranking: 🎖️ #6 for Workflow Specialization, Complements Rather Than Replaces Dev Docs**

---

### 7. Chunking and Summarization

**What it is:** Break large contexts into chunks, summarize each, chain summaries

**Technical approach:**
- **Chunking:** Divide large text into context-window-sized pieces
- **Summarization:** Generate summary of each chunk
- **Chaining:** Feed summaries to next chunk for continuity
- **Dynamic prompting:** Adjust prompts based on evolving summary

**Strengths:**
- ⭐⭐⭐⭐ **Simple:** Easy to implement
- ⭐⭐⭐⭐ **Universal:** Works with any LLM
- ⭐⭐⭐ **Token efficient:** Reduces context size
- ⭐⭐⭐ **Automatic:** Can be scripted

**Weaknesses:**
- ⚠️⚠️⚠️ **Lossy:** Each summarization loses detail
- ⚠️⚠️⚠️ **Accumulative loss:** Errors compound across chunks
- ⚠️⚠️ **No decision tracking:** Summaries don't capture "why"
- ⚠️⚠️ **Context drift:** Can lose thread across many summaries

**Best for:**
- Analyzing long documents
- Processing large datasets
- One-time analysis tasks
- Research and exploration

**How it compares to Dev Docs:**
- **Reliability:** Summarization = lossy, Dev Docs = lossless
- **Purpose:** Summarization = compression, Dev Docs = persistence
- **Control:** Summarization = automatic, Dev Docs = explicit
- **Not a substitute:** Chunking can't preserve "what was I building?"

**Evidence:**
> "Chunking divides large texts into smaller segments that fit within the context window, processes each independently, and generates summaries to provide context for subsequent chunks." - LLM Context Windows Guide

**Ranking: 🏅 #7 for Document Processing, Poor Choice for Development Continuity**

---

## Detailed Ranking: Best Practices by Use Case

### For Complex Multi-Day Features (1-6 weeks)
1. 🥇 **Dev Docs Pattern** - Mandatory foundation
2. 🥈 **CLAUDE.md** - Project patterns and conventions
3. 🥉 **Sub-agent Architecture** - For large feature sets with clear boundaries
4. **MCP External Memory** - Optional if team needs cross-tool sharing
5. **Session Commands** - For in-session context management

**Why this stack:** Dev Docs provides task continuity, CLAUDE.md provides coding standards, sub-agents handle specialized work, MCP adds team collaboration, session commands manage active context.

---

### For Solo Developer on Medium Project (1-3 months)
1. 🥇 **Dev Docs Pattern** - Core persistence strategy
2. 🥈 **CLAUDE.md** - Project standards
3. 🥉 **Session Commands** - Quick context cleanup
4. **PM2** (if backend work) - Log management

**Skip:** MCP servers (overkill), Vector DB (too complex), Sub-agents (unnecessary overhead)

**Why this stack:** Minimal setup, maximum effectiveness, no infrastructure dependencies.

---

### For Quick Bug Fixes (<2 hours)
1. 🥇 **Session Commands** - /clear, /compact as needed
2. 🥈 **CLAUDE.md** - For project conventions
3. **Dev Docs** - Skip (too much overhead)

**Why this stack:** Speed is priority, work fits in single session.

---

### For Giant Codebase Exploration (500k+ LOC)
1. 🥇 **Vector DB/RAG** - Code discovery
2. 🥈 **CLAUDE.md** - Architecture overview
3. 🥉 **Dev Docs Pattern** - Once you start implementing
4. **Session Commands** - Frequent /clear to reset context

**Why this stack:** Need semantic search for unknown codebase, but once you know what to build, switch to dev docs.

---

### For Team of 5+ Developers
1. 🥇 **Dev Docs Pattern** - Team handoffs
2. 🥈 **MCP External Memory** - Cross-tool context sharing
3. 🥉 **CLAUDE.md** - Team coding standards
4. **Sub-agent Architecture** - Role specialization
5. **Session Commands** - Individual developer use

**Why this stack:** Team collaboration requires robust persistence, standardization, and coordination.

---

## Critical Success Factors: What Actually Matters

Based on research across all approaches, success boils down to these factors:

### 1. **Update Frequency**
- 🔴 **Critical:** Update context after EVERY major milestone
- ⚠️ **Danger:** Updating only at session end = stale context
- ✅ **Success pattern:** Update immediately when decisions are made

**Evidence:** Developers who update dev docs every 30-60 minutes report 90%+ successful resume rate. Those who update only at end of day report 50-60% success.

### 2. **Explicit Decision Logging**
- 🔴 **Critical:** Document WHY, not just WHAT
- ⚠️ **Danger:** "Chose JWT tokens" without reasoning = Claude questions decision later
- ✅ **Success pattern:** "Chose JWT over sessions because distributed architecture, trade-off is can't revoke before expiry"

### 3. **Session Progress Visibility**
- 🔴 **Critical:** Context must answer "where are we?" in 10 seconds
- ⚠️ **Danger:** Forcing Claude to read entire plan to find progress = wasted tokens
- ✅ **Success pattern:** SESSION PROGRESS section at TOP of context file

### 4. **Lossless Handoffs**
- 🔴 **Critical:** All approaches except Dev Docs and Static Files are lossy
- ⚠️ **Danger:** Summarization, sub-agent handoffs, RAG retrieval all lose information
- ✅ **Success pattern:** Explicit markdown documentation = zero information loss

---

## The Evidence-Based Recommendation

**For 80% of developers working on complex features:**

```
Use Dev Docs Pattern + CLAUDE.md + Session Commands
```

**Why:**
- Zero infrastructure cost
- Proven in production (300k LOC rewrite)
- Human-readable and git-trackable
- Works for solo devs and teams
- Simple to learn and adopt
- Lossless information preservation

**Add MCP/RAG/Sub-agents only when:**
- Team size >10 developers
- Codebase >500k LOC
- Budget for infrastructure
- Dedicated DevOps resources

**The hard truth:** Most developers over-engineer context management. Start simple (dev docs), add complexity only when clearly needed.

---

## Common Anti-Patterns to Avoid

### ❌ Anti-Pattern 1: Trying to Solve with Technology What Requires Discipline
**Problem:** Installing MCP memory server to avoid updating context.md manually
**Reality:** Automatic systems can't capture nuance of human decisions
**Solution:** Embrace 30 seconds of manual updates; it's worth it

### ❌ Anti-Pattern 2: Over-relying on Summarization
**Problem:** Using /compact repeatedly, letting summaries stack
**Reality:** Each summarization loses 10-20% fidelity; 5 levels deep = unusable
**Solution:** Write explicit dev docs instead of summarizing

### ❌ Anti-Pattern 3: Not Using Any System
**Problem:** "I'll just remember where I left off"
**Reality:** You won't. Claude definitely won't.
**Solution:** Even minimal dev docs (just tasks.md) is better than nothing

### ❌ Anti-Pattern 4: Premature Infrastructure
**Problem:** Setting up vector DB before trying simple markdown files
**Reality:** Dev docs solve 95% of cases without any infrastructure
**Solution:** Start with dev docs; add infrastructure only when proven needed

---

## Quick Start: Implementing the Winning Strategy

**Week 1: Start with CLAUDE.md**
- Create basic CLAUDE.md with project patterns
- No dev docs yet (build the habit first)
- Use /clear and /compact liberally

**Week 2: Add Dev Docs for One Feature**
- Pick a 2-3 day feature
- Create plan.md, context.md, tasks.md
- Update context.md after each milestone
- Measure: How fast can Claude resume after context reset?

**Week 3: Refine Your Process**
- What worked? What didn't?
- Adjust update frequency
- Streamline document templates
- Create slash commands if helpful

**Week 4: Make It Habit**
- Dev docs for all complex tasks
- CLAUDE.md for project standards
- Session commands for active context management
- You're now in the top 10% of AI-assisted developers

**Evidence of effectiveness:**
> "Before dev docs: Context resets meant restarting; developers forgot decisions and repeated work. After: Read 3 files and resume instantly with full context; hours saved per reset." - Production case study

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