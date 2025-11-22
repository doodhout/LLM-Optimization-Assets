# Dev Docs Pattern for Complex Tasks

## The Problem: Claude Loses Track

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

## The Solution: Persistent Dev Docs

**Create a documentation trail for every large task:**

```bash
dev/
└── active/
    └── [task-name]/
        ├── [task-name]-plan.md        # The accepted plan
        ├── [task-name]-context.md     # Key files, decisions, integrations
        └── [task-name]-tasks.md       # Checklist of work items
```

## Starting Large Tasks

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

## Continuing After Context Loss

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

## Updating Dev Docs Before Compaction

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

## Benefits of Dev Docs System

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

## Critical Importance: Context Window Limits are the Hidden Bottleneck

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

## Deep Dive: The Dev Docs Pattern in Detail

The dev docs pattern from [claude-code-infrastructure-showcase](https://github.com/diet103/claude-code-infrastructure-showcase) provides the most comprehensive solution to context persistence we've found. Here's why it works:

### The Three-File Architecture

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

## Pro Tips

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
