# Spec-Driven Development with LLMs

## Overview

Spec-Driven Development (SDD) is a methodology where specifications become the source of truth and drive the entire development process. In the age of AI coding assistants, SDD represents a shift from "vibe coding" (ad-hoc prompting) to structured, intentional development where **specifications are executable**.

**Core principle:** Move from "code is the source of truth" to "intent is the source of truth."

## What is Spec-Driven Development?

### Traditional Definition

Traditionally, specification-driven development means writing detailed specifications before writing code. This traces back to methodologies like Design by Contract (DbC) and formal methods.

### Modern Definition (with AI/LLMs)

With AI coding assistants, spec-driven development transforms specifications from passive documentation into **executable blueprints** that directly generate working implementations.

**The paradigm shift:**
```
Old: Specification → Human interprets → Human codes → Code
New: Specification → AI interprets → AI generates → Code
```

**Why it matters now:**
AI makes specifications executable. What was once just documentation becomes a programming interface.

## The Problem: "Vibe Coding"

### What is Vibe Coding?

"Vibe coding" is an exploratory, prompt-first approach where developers:
- Rapidly prompt AI without clear specifications
- Get code without understanding it
- Iterate based on "vibes" rather than requirements
- Accept or reject based on intuition

**Example of vibe coding:**
```
Developer: "Make a dashboard"
AI: [Generates 800 lines across 10 files]
Developer: "Hmm, not quite what I wanted, try again"
AI: [Generates different 800 lines]
Developer: "Still not right..."
[Repeat indefinitely]
```

### Why Vibe Coding Fails

**Evidence from research and community:**

1. **Technical Debt Tsunami**: Forrester forecasts an "incoming technical debt tsunami" over the next 2 years due to vibe coding
2. **Code Churn**: AI-generated code often undergoes substantial modification or removal after initial acceptance
3. **Duplicated Code**: Studies show significant increases in duplicated code blocks from unguided AI usage
4. **Fragile Codebases**: As noted by Salvatore Sanfilippo, LLMs left alone with nontrivial goals produce "fragile code bases that are larger than needed, complex, full of local minima choices"

**The core issue:** Without specifications, AI optimizes for plausible code, not correct code.

## The Solution: Spec-Driven Development

### Core Concepts

**1. Separation of Concerns**
- **What** (specification) - Stable, human-defined intent
- **How** (implementation) - Flexible, AI-generated code

**2. Decomposition Pipeline**
```
Feature → User Stories → Tasks → Iterative Implementation

Not:
Feature → Big Bang Implementation
```

**3. Refined Context**
Specifications are "refined context" that provides just enough information to LLMs to be effective without being overwhelmed.

**4. Version-Controlled Intent**
A specification becomes a kind of version-controlled, human-readable "super prompt."

### Implementation Levels

Three levels of spec-driven development maturity:

**Level 1: Spec-First**
- Write specification first
- Use spec in AI-assisted development
- May discard spec after implementation

**Level 2: Spec-Anchored**
- Specification kept after task completion
- Used for evolution and maintenance
- Spec co-evolves with code

**Level 3: Spec-as-Source**
- Specification is the main source file
- Only spec is edited by humans
- Code is always generated from spec
- Code marked as "DO NOT EDIT - GENERATED FROM SPEC"

**Current state:** Most tools aim for spec-first, some for spec-anchored, few for spec-as-source.

## Tools and Frameworks

### 1. GitHub Spec Kit

**What it is:**
Open-source toolkit for spec-driven development with AI coding agents.

**Repository:** github.com/github/spec-kit
**Website:** speckit.org
**Status:** Experimental (v0.0.30+)

**Supported AI Tools:**
- GitHub Copilot
- Claude Code
- Gemini CLI
- Cursor
- Windsurf
- And others

**Core Components:**

**1. Specify CLI**
Bootstraps projects for SDD, downloads templates, sets up scaffolding.

```bash
npm install -g @github/spec-kit
specify init
```

**2. Directory Structure**
```
.specify/
├── constitution/       # Project principles and standards
│   ├── architecture.md
│   ├── coding-standards.md
│   └── tech-stack.md
├── spec.md            # What to build
├── plan.md            # How to build it
└── tasks/             # Discrete work items
    ├── task-001.md
    ├── task-002.md
    └── ...
```

**3. Workflow Phases**

**Phase 1: Specify**
Define what the project should do in `spec.md`.

```markdown
# Feature: User Authentication

## Overview
Users need to securely authenticate to access the application.

## Requirements
- Users can register with email and password
- Passwords must be hashed (bcrypt, min 10 rounds)
- Users can log in with email and password
- JWT tokens for session management (1-hour expiry)
- Refresh tokens (7-day expiry, httpOnly cookies)

## Acceptance Criteria
- [ ] Registration endpoint validates email format
- [ ] Password requirements: min 8 chars, 1 uppercase, 1 number
- [ ] Login returns JWT in response body
- [ ] Login sets refresh token in httpOnly cookie
- [ ] Invalid credentials return 401 with generic message
- [ ] Rate limiting: 5 failed attempts per IP per 15 minutes

## Edge Cases
- Duplicate email registration → 409 Conflict
- Missing fields → 400 Bad Request with field details
- Malformed JWT → 401 Unauthorized
```

**Phase 2: Plan**
Translate intent into technical approach in `plan.md`.

```markdown
# Implementation Plan: User Authentication

## Architecture
- RESTful API endpoints
- Service layer for business logic
- Repository layer for data access
- Middleware for auth verification

## Tech Stack
- Express.js for routing
- bcrypt for password hashing
- jsonwebtoken for JWT generation
- express-rate-limit for rate limiting
- PostgreSQL for user storage

## Components
1. AuthService (handles auth logic)
2. UserRepository (database access)
3. AuthMiddleware (JWT verification)
4. AuthController (HTTP handlers)

## Database Schema
```sql
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email VARCHAR(255) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_users_email ON users(email);
```

## Error Handling
- Use AppError base class
- Map to appropriate HTTP status codes
- Log errors with context (not passwords!)

## Testing Strategy
- Unit tests for AuthService
- Integration tests for endpoints
- Security tests for JWT handling
```

**Phase 3: Tasks**
Break plan into atomic tasks with tight scope.

```markdown
# Task 001: Implement User Model and Repository

## Scope
Create User model and repository layer for database access.

## Files to Create
- src/models/User.ts
- src/repositories/UserRepository.ts
- src/repositories/UserRepository.test.ts

## Acceptance Criteria
- [ ] User model with id, email, passwordHash fields
- [ ] UserRepository.create(email, passwordHash)
- [ ] UserRepository.findByEmail(email)
- [ ] UserRepository.findById(id)
- [ ] All methods have error handling
- [ ] Unit tests with 90%+ coverage

## Dependencies
- None (foundational task)

## Estimated Complexity
Low - standard repository pattern
```

**Key Principles:**
- Each task is independently implementable
- Clear acceptance criteria
- Explicit dependencies
- Tight scope (avoid overlap)

**Benefits:**

✅ **Clarity**: Everyone knows what's being built
✅ **Control**: Human defines intent, AI handles implementation
✅ **Predictability**: Structured process, not guesswork
✅ **Collaboration**: Specs are shareable, reviewable
✅ **Iterability**: Change spec, regenerate code

**Limitations:**

⚠️ **Experimental**: Still evolving rapidly
⚠️ **Greenfield Focus**: Best for new projects
⚠️ **Tool Switching**: Some constraints on switching AI agents mid-project
⚠️ **Python 3.11+**: Dependency requirement
⚠️ **Learning Curve**: Requires understanding SDD concepts

### 2. Kiro

**What it is:**
Agentic AI IDE that brings structure to AI coding with spec-driven development.

**Website:** kiro.dev
**Developer:** AWS (part of Amazon)
**Philosophy:** "Make your intent explicit with executable specs"

**Core Workflow:**

**Phase 1: Requirements**
```
Natural language → EARS notation → Clear requirements

EARS (Easy Approach to Requirements Syntax):
- When [trigger], the [system] shall [behavior]
- While [state], the [system] shall [behavior]
- If [condition], then the [system] shall [behavior]
```

**Example:**
```
User input: "Users should be able to reset their password"

Kiro generates:
- When a user requests password reset, the system shall send a reset link to their registered email
- When a user clicks the reset link, the system shall validate the token and allow password change
- If the reset token is expired, then the system shall return an error and require a new reset request
- While the user is entering a new password, the system shall validate password strength in real-time
```

**Phase 2: Design**
Analyzes codebase to develop:
- Architecture decisions
- System design
- Tech stack choices
- Integration points

**Phase 3: Implementation**
Creates implementation plan with:
- Discrete tasks
- Dependency sequencing
- Progress tracking

**Real-World Examples:**

**Example 1: HR Automation Platform**
- Enterprise-grade application on Amazon Bedrock
- Resume ranking and analysis
- Advanced search capabilities
- Built using Kiro's spec-driven approach

**Example 2: Legacy Code Revival**
Developer used Kiro to revive "graveyard" project:
```
Specification:
"Add automated archiving for repositories inactive for 1 year"

Kiro Output:
1. User Story: As an admin, I want repositories to auto-archive
2. Acceptance Criteria (EARS format)
3. Implementation Tasks (sequenced by dependencies)
4. Working implementation

Result: 10x productivity increase reported
```

**Example 3: Serverless Weather App**
- Terraform for AWS deployment
- API integration
- Serverless architecture
- Complete infrastructure as code

**Benefits:**

✅ **Structured Workflow**: Clear Requirements → Design → Implementation
✅ **Context Persistence**: Specs maintain context across sessions
✅ **Team Collaboration**: Specs are sharable and trackable
✅ **Automated Reasoning**: AWS-backed formal verification
✅ **Production-Ready**: Focus on enterprise applications

**Unique Features:**
- EARS notation for requirements
- Automated reasoning capabilities
- Integrated with AWS services
- Codebase analysis for design phase

### 3. Tessl

**What it is:**
Framework exploring spec-as-source level of SDD.

**Philosophy:** Specification is the only thing humans edit. Code is always generated.

**Approach:**
```typescript
// UserService.ts
// GENERATED FROM SPEC - DO NOT EDIT

class UserService {
  // ... generated implementation
}
```

**Status:** Most ambitious in pursuing spec-as-source
**Maturity:** Early stage, actively exploring the model

**Goal:** Code becomes an implementation detail, specification becomes the program.

### Comparison Matrix

| Feature | Spec Kit | Kiro | Tessl |
|---------|----------|------|-------|
| **Maturity** | Experimental | Production-ready | Early stage |
| **SDD Level** | Spec-first | Spec-anchored | Spec-as-source |
| **AI Support** | Multi-agent | Built-in | TBD |
| **Open Source** | Yes | No | TBD |
| **Best For** | Flexibility | Enterprise | Future vision |
| **Learning Curve** | Moderate | Low | TBD |
| **Cost** | Free | Paid | TBD |

## Best Practices for Spec-Driven Development

### ✅ DO: Effective Practices

#### 1. **Write Specifications First**

**Always start with a spec, not code.**

❌ **Bad workflow:**
```
Prompt AI → Get code → Realize it's wrong → Prompt again → Get different code → Repeat
```

✅ **Good workflow:**
```
Write spec → Review spec → AI generates from spec → Code matches spec → Done
```

**Evidence:** Harper Reed's workflow emphasizes using conversational LLM to iteratively develop a thorough, step-by-step specification before any implementation.

#### 2. **Keep Specs Tightly Scoped**

**Each spec should cover one cohesive feature or component.**

❌ **Too broad:**
```markdown
# Specification: Build the entire application
- User management
- Product catalog
- Shopping cart
- Payment processing
- Admin dashboard
- Analytics
- Email notifications
```

✅ **Right-sized:**
```markdown
# Specification: User Registration

## What
Allow users to create accounts with email and password.

## Requirements
[Specific, detailed requirements for just registration]

## Scope
In scope: Registration endpoint, validation, password hashing
Out of scope: Login, password reset, profile management
```

**Why it works:** Tight scope prevents overlap, allows mixing of programming languages, enables focused AI generation.

#### 3. **Include Tests in Specs from Day 1**

**Tests are part of your intent.**

✅ **Specification with tests:**
```markdown
# Feature: User Registration

## Requirements
[Feature requirements]

## Testing Requirements
- Unit tests for validation logic (90%+ coverage)
- Integration tests for registration endpoint
- Security tests for password hashing
- Load tests for 1000 concurrent registrations

## Test Cases
1. Valid registration → 201 Created, user in database
2. Duplicate email → 409 Conflict
3. Weak password → 400 Bad Request with details
4. SQL injection attempt → Sanitized, 400 Bad Request
5. Rate limit exceeded → 429 Too Many Requests
```

**Why it works:** Tests clarify intent, catch AI mistakes early, enable spec-as-source (regenerate code, tests still pass).

#### 4. **Version Your Specs with Git**

**Treat specifications as code.**

```bash
# Commit spec changes
git add .specify/spec.md
git commit -m "spec: add password reset functionality"

# Track spec evolution
git log .specify/spec.md

# Review spec changes in PRs
# See spec diffs alongside code diffs
```

**Benefits:**
- History of intent
- Enables rollback
- Facilitates collaboration
- Documents decision-making

#### 5. **Use the Constitution Pattern**

**Define stable principles in a "constitution" file.**

**Structure:**
```
.specify/
├── constitution/
│   ├── architecture.md      # Stable architectural principles
│   ├── coding-standards.md  # Code style, conventions
│   ├── tech-stack.md        # Approved technologies
│   ├── security.md          # Security requirements
│   └── testing.md           # Testing standards
├── spec.md                  # Feature specifications (changes often)
├── plan.md                  # Implementation plans (changes often)
└── tasks/                   # Work items (changes constantly)
```

**Constitution content:**
```markdown
# Architecture Constitution

## Principles
- Clean Architecture with dependency inversion
- Domain-driven design patterns
- Microservices where appropriate, monolith by default
- Event-driven for async operations

## Non-Negotiables
- All business logic in domain layer
- No database access from controllers
- All external dependencies injected
- Feature flags for all new features

## Patterns We Use
- Repository pattern for data access
- Service layer for business logic
- DTO pattern for API contracts
- Factory pattern for complex object creation

## Patterns We Avoid
- God objects
- Tight coupling to frameworks
- Direct database access from services
- Mutable global state
```

**Why it works:** Stable principles don't need to be repeated in every spec. AI learns project patterns.

#### 6. **Start with One Feature**

**Don't try to spec your entire application upfront.**

**Recommended approach:**
```
Week 1: Pick one well-understood feature
  → Write detailed spec
  → Generate with AI
  → Evaluate results
  → Learn what works

Week 2: Apply learnings to second feature
  → Refine spec-writing approach
  → Better results

Week 3+: Scale up
  → Multiple features
  → Team adoption
  → Process refinement
```

**Why it works:** Learning curve for effective specs. Start small, iterate, scale up.

**Evidence:** Community consensus from early adopters: "Begin with one feature to see how your AI agent performs."

#### 7. **Be Extremely Clear About "Done"**

**Define completion criteria explicitly.**

✅ **Good "done" definition:**
```markdown
## Definition of Done

This feature is complete when:
- [ ] All acceptance criteria pass
- [ ] Unit tests at 90%+ coverage, all passing
- [ ] Integration tests for happy path and error cases, all passing
- [ ] Security review completed (SQL injection, XSS, auth bypass)
- [ ] Performance tests show <100ms response time at 1000 req/s
- [ ] API documentation generated and accurate
- [ ] Code review approved by senior engineer
- [ ] Deployed to staging environment
- [ ] Smoke tests pass in staging
```

**Why it works:** No ambiguity about completion. AI and humans agree on success criteria.

#### 8. **Use Iterative Refinement**

**Don't expect perfect specs on first try.**

**Pattern:**
```
Draft 1: High-level overview
  → Review: Too vague

Draft 2: Add detailed requirements
  → Review: Missing edge cases

Draft 3: Add edge cases and error handling
  → Review: Testing unclear

Draft 4: Add testing requirements
  → Review: Good! Generate code

Generated code → Review → Find gaps in spec

Draft 5: Update spec to address gaps
  → Regenerate code → Better!
```

**Why it works:** Specifications improve through iteration, just like code.

#### 9. **Maintain a Lessons Learned File**

**Capture what works and what doesn't.**

**Structure:**
```markdown
# Lessons Learned

## What Works

### Detailed Error Handling Specs (2024-01-15)
When we specify exact error messages and status codes, AI generates consistent error handling.

Example:
- "Return 400 Bad Request" → AI varies implementation
- "Return 400 Bad Request with JSON body: {error: 'INVALID_EMAIL', message: 'Email format is invalid'}" → AI is consistent

### Explicit Technology Choices (2024-01-20)
Specifying "use bcrypt with 12 rounds" instead of "hash passwords securely" prevents AI from using different libraries.

## What Doesn't Work

### Vague Performance Requirements (2024-01-18)
"Make it fast" → AI generates code with no particular optimization
Better: "Respond in <100ms at p95 for 1000 req/s load"

### Assuming AI Knows Context (2024-01-22)
AI doesn't remember previous features unless told.
Must explicitly reference: "Use the same error handling pattern as the authentication feature"

## AI Mistakes We've Seen

### Rewriting Entire Files (2024-01-25)
AI sometimes rewrites entire files when asked to modify small parts.
Mitigation: Spec should say "Modify only the login function in auth.ts, keep all other functions unchanged"

### Creating Duplicate Functions (2024-01-28)
AI created validateEmail() when it already existed.
Mitigation: Reference existing utilities in spec: "Use the existing validateEmail() function from utils/validation.ts"
```

**Why it works:** Team learns what makes effective specs. Avoid repeating mistakes.

**Evidence:** Community practice from early SDD adopters.

#### 10. **Don't Shortcut the Specification Process**

**Invest time in specifications upfront.**

**Time investment:**
- Writing spec: 20-30% of project time
- Planning from spec: 10-15% of project time
- Implementation (AI-assisted): 40-50% of project time
- Review and refinement: 20-30% of project time

**ROI:**
- Vibe coding: Fast start, slow finish, unpredictable results
- Spec-driven: Slower start, fast finish, predictable results

**Evidence:** "Don't shortcut — describe what you want, being extremely clear about the features you want, how you want them to work, what you want the tech stack to look like, and what 'done' looks like." - SDD best practices

### ❌ DON'T: Anti-Patterns

#### 1. **Don't Write Specs That Are Code**

**Problem:** Specs that are pseudocode or detailed algorithms.

❌ **Bad spec (too detailed):**
```markdown
## Hash Password Function

1. Import bcrypt library
2. Define function hashPassword(password: string)
3. Set saltRounds = 12
4. Call bcrypt.genSalt(saltRounds)
5. Store result in salt variable
6. Call bcrypt.hash(password, salt)
7. Return the hash
8. Handle errors with try-catch
9. Log errors to console
```

✅ **Good spec (intent-focused):**
```markdown
## Password Hashing

Requirements:
- Use bcrypt for password hashing
- Minimum 12 salt rounds
- Hash function must be async
- Throw AppError on hashing failure (don't expose bcrypt errors)
- Log hashing errors with context (never log passwords)

Acceptance:
- Hashing same password twice produces different hashes
- Hashed passwords can be verified with bcrypt.compare()
- Hashing failures result in AppError, not raw bcrypt errors
```

**Why bad spec fails:** You're doing the AI's job. Spec should describe "what," AI figures out "how."

#### 2. **Don't Create Markdown Overload**

**Problem:** Specifications become excessively verbose.

❌ **Bad (verbose spec):**
```markdown
# User Registration Feature

## Introduction
User registration is a critical feature of our application. It allows new users to create accounts and gain access to the platform. This feature is important because without it, users cannot use our application. We need to implement this feature carefully to ensure security and usability.

## Background
In modern web applications, user registration typically involves collecting user information such as email addresses and passwords. The email address serves as a unique identifier for each user, while the password provides security for the account. It's important to note that passwords should never be stored in plain text, as this would be a security vulnerability. Instead, passwords must be hashed using a cryptographically secure hashing algorithm.

## Requirements

### Functional Requirements

#### Registration Endpoint
The registration endpoint will accept user registration requests. It will be accessible via HTTP POST request. The endpoint will validate the input data to ensure it meets our requirements...

[Continues for 5000 more words]
```

✅ **Good (concise spec):**
```markdown
# User Registration

## What
Allow new users to register with email and password.

## Requirements
- POST /api/auth/register endpoint
- Input: { email: string, password: string }
- Email: valid format, unique, max 255 chars
- Password: min 8 chars, 1 uppercase, 1 number
- Hash passwords with bcrypt (12 rounds)
- Return JWT token on success
- Rate limit: 10 requests per IP per hour

## Success Response
201 Created
{ token: "jwt...", user: { id: "uuid", email: "user@example.com" } }

## Error Responses
- 400: Invalid input (with field-specific errors)
- 409: Email already registered
- 429: Rate limit exceeded

## Testing
- Unit tests for validation logic
- Integration tests for endpoint
- Security tests for password handling
```

**Why verbose fails:** Developers spend time reading long specs and hunting for mistakes hidden in verbose prose.

**Evidence:** "SDD produces excessive text, especially in the design phase, with developers spending most of their time reading long Markdown files." - SDD pitfalls research

#### 3. **Don't Expect "Generate Once" Magic**

**Problem:** Treating SDD as "write spec, get perfect code, done."

**Reality:** SDD is iterative.

**Realistic workflow:**
```
1. Write spec
2. Generate code (1st attempt)
3. Review → Find issues
4. Refine spec (clarify ambiguities)
5. Regenerate code (2nd attempt)
6. Review → Better, but edge cases missing
7. Update spec (add edge cases)
8. Regenerate code (3rd attempt)
9. Review → Good!
10. Deploy

Then:
11. Bug found in production
12. Update spec to cover the bug
13. Regenerate code
14. Test fix
15. Deploy
```

**Why "magic bullet" thinking fails:** AI generates code based on interpretation of spec. Interpretation isn't perfect. Iteration refines both spec and code.

**Evidence:** "Unstable regeneration - each regeneration might produce slightly different implementations." - SDD lessons learned

#### 4. **Don't Skip Human Review**

**Problem:** Trusting AI-generated code without review.

❌ **Bad workflow:**
```
Spec → AI generates → Deploy to production
```

✅ **Good workflow:**
```
Spec → AI generates → Human reviews spec compliance → Human reviews code quality → Human reviews tests → Human approves → Deploy
```

**Double review burden:**
The technical specification (plan) often contains code examples. You must review:
1. The spec/plan code examples
2. The final generated implementation

**Why both are needed:**
- Spec review: Catch intent errors before generation
- Code review: Catch AI mistakes, edge cases, security issues

**Evidence:** "The technical specification already contains code, requiring developers to review this code before running it, and since there will still be bugs, they'll need to review the final implementation too." - SDD pitfalls

#### 5. **Don't Use SDD for Everything**

**Problem:** Trying to spec-drive all development.

**When SDD works well:**
- ✅ New features in existing systems (N-to-N+1)
- ✅ Well-understood problem domains
- ✅ Features with clear requirements
- ✅ Team collaboration on complex features
- ✅ Maintenance and evolution of features

**When SDD is overkill:**
- ❌ Trivial changes (fixing typos, updating dependencies)
- ❌ Exploratory prototypes (don't know what you want yet)
- ❌ One-off scripts
- ❌ Experimental features (requirements will change drastically)

**Evidence:** "Systematic bureaucracy - the three-step design process is excessive for most cases." - SDD pitfalls

#### 6. **Don't Ignore the Constitution**

**Problem:** Repeating the same principles in every spec.

❌ **Bad pattern:**
```
spec-001.md: "Use Clean Architecture, dependency injection, PostgreSQL, bcrypt..."
spec-002.md: "Use Clean Architecture, dependency injection, PostgreSQL, bcrypt..."
spec-003.md: "Use Clean Architecture, dependency injection, PostgreSQL, bcrypt..."
```

✅ **Good pattern:**
```
constitution/
  architecture.md: "Use Clean Architecture with dependency injection"
  tech-stack.md: "PostgreSQL for database, bcrypt for hashing"

spec-001.md: "Feature requirements for user registration"
spec-002.md: "Feature requirements for password reset"
spec-003.md: "Feature requirements for user profile"
```

**Why it works:** Constitution defines stable principles once. Specs focus on feature-specific requirements.

#### 7. **Don't Mix Abstraction Levels in Specs**

**Problem:** Jumping between high-level and low-level details.

❌ **Bad spec:**
```markdown
# User Authentication

We need secure authentication using industry best practices.

The password hashing should use bcrypt with exactly 12 rounds and the salt should be generated using bcrypt.genSalt(12) and then...

Authentication is important for protecting user data.

The JWT should have the following payload structure: { sub: userId, iat: timestamp, exp: timestamp } and be signed with HS256...

Users want a smooth login experience.
```

✅ **Good spec:**
```markdown
# User Authentication

## Overview
Implement secure JWT-based authentication.

## Security Requirements
- Passwords hashed with bcrypt (min 12 rounds)
- JWTs signed with HS256
- Tokens expire after 1 hour
- Refresh tokens expire after 7 days

## User Experience
- Login response time <200ms
- Clear error messages for failed auth
- "Remember me" option (30-day refresh token)

## Technical Details
See plan.md for implementation specifics.
```

**Why mixed abstraction fails:** Confuses what matters most. Hard to extract key requirements.

## Complete Workflow Example

### Scenario: Add Password Reset Feature

**Phase 1: Write Specification**

```markdown
# Feature: Password Reset

## Overview
Users who forget their password need a secure way to reset it.

## User Stories
1. As a user, I want to request a password reset link via email
2. As a user, I want to click the link and set a new password
3. As a user, I want confirmation that my password was reset

## Requirements

### Request Reset
- POST /api/auth/password-reset/request
- Input: { email: string }
- Send email with reset link (token valid 1 hour)
- Always return 200 (prevent email enumeration)
- Rate limit: 3 requests per email per hour

### Reset Password
- POST /api/auth/password-reset/confirm
- Input: { token: string, newPassword: string }
- Validate token (not expired, not used, matches user)
- Validate new password (min 8 chars, 1 uppercase, 1 number)
- Hash new password (bcrypt, 12 rounds)
- Invalidate token after use
- Return success confirmation

### Email Template
- Subject: "Password Reset Request"
- Body: Link to reset page with token
- Link expires in 1 hour
- Include "didn't request this?" message

## Security Requirements
- Tokens are cryptographically random (32 bytes)
- Tokens stored hashed in database
- Old password not required (user forgot it)
- All reset tokens invalidated on successful reset
- Log all reset attempts (for security monitoring)

## Error Cases
- Invalid email format → 400 Bad Request
- Expired token → 400 "Reset link has expired"
- Used token → 400 "Reset link already used"
- Invalid token → 400 "Invalid reset link"
- Weak password → 400 with password requirements

## Testing Requirements
- Unit tests for token generation and validation
- Integration tests for both endpoints
- Security tests for token security
- Email sending tests (mocked)
- Rate limiting tests

## Definition of Done
- [ ] Both endpoints implemented and tested
- [ ] Email sending integrated (with fallback)
- [ ] All security requirements met
- [ ] API documentation updated
- [ ] Frontend can use the endpoints
- [ ] Monitoring/logging in place
```

**Phase 2: Create Plan**

```markdown
# Implementation Plan: Password Reset

## Architecture
Use existing authentication architecture, add password reset service.

## Components

### 1. PasswordResetService
- generateResetToken(email): Create and send reset email
- validateResetToken(token): Check if token is valid
- resetPassword(token, newPassword): Complete the reset

### 2. Database Schema
```sql
CREATE TABLE password_reset_tokens (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES users(id),
  token_hash VARCHAR(255) NOT NULL,
  created_at TIMESTAMP DEFAULT NOW(),
  expires_at TIMESTAMP NOT NULL,
  used_at TIMESTAMP,
  INDEX idx_token_hash (token_hash),
  INDEX idx_expires_at (expires_at)
);
```

### 3. Email Integration
- Use existing EmailService
- New template: password-reset-email.hbs
- Queue emails (don't block request)

### 4. Controllers
- PasswordResetController.request(req, res)
- PasswordResetController.confirm(req, res)

### 5. Routes
- POST /api/auth/password-reset/request
- POST /api/auth/password-reset/confirm

## Dependencies
- crypto (for token generation)
- bcrypt (already in project)
- EmailService (already exists)
- UserRepository (already exists)

## Error Handling
- Use existing AppError system
- New error codes: TOKEN_EXPIRED, TOKEN_INVALID, TOKEN_USED

## Security Considerations
- Hash tokens before storing (like passwords)
- Use timing-safe comparison for tokens
- Rate limit both endpoints
- Log all reset attempts
- Don't reveal if email exists

## Testing Strategy
- Unit tests for PasswordResetService (mock EmailService)
- Integration tests for endpoints
- Security tests (expired tokens, used tokens, invalid tokens)
- Rate limiting tests
```

**Phase 3: Break into Tasks**

```markdown
# Task 001: Database Schema and Repository

## Scope
Create database schema for reset tokens and repository methods.

## Files
- migrations/008_create_password_reset_tokens.sql
- src/repositories/PasswordResetTokenRepository.ts
- src/repositories/PasswordResetTokenRepository.test.ts

## Acceptance Criteria
- [ ] Migration creates password_reset_tokens table
- [ ] Indexes on token_hash and expires_at
- [ ] Repository.create(userId, tokenHash, expiresAt)
- [ ] Repository.findByTokenHash(tokenHash)
- [ ] Repository.markAsUsed(tokenId)
- [ ] Repository.deleteExpired()
- [ ] All methods tested (90%+ coverage)

## Dependencies
None (foundational)

---

# Task 002: Token Generation and Validation Service

## Scope
Create service for generating and validating reset tokens.

## Files
- src/services/PasswordResetTokenService.ts
- src/services/PasswordResetTokenService.test.ts

## Acceptance Criteria
- [ ] generateToken(): Creates 32-byte random token
- [ ] hashToken(token): Hashes token for storage
- [ ] validateToken(token): Checks if token exists, not expired, not used
- [ ] All edge cases tested
- [ ] Timing-safe token comparison

## Dependencies
- Task 001 (needs repository)

---

# Task 003: Email Template and Sending

## Scope
Create password reset email template and sending logic.

## Files
- templates/emails/password-reset.hbs
- src/services/PasswordResetEmailService.ts
- src/services/PasswordResetEmailService.test.ts

## Acceptance Criteria
- [ ] Email template with reset link
- [ ] sendResetEmail(email, token): Queues email
- [ ] Email includes expiration time
- [ ] Email includes security message
- [ ] Tests with mocked EmailService

## Dependencies
- Existing EmailService

---

# Task 004: Password Reset Service (Business Logic)

## Scope
Implement core password reset business logic.

## Files
- src/services/PasswordResetService.ts
- src/services/PasswordResetService.test.ts

## Acceptance Criteria
- [ ] requestReset(email): Generate token, send email, always return success
- [ ] validateResetToken(token): Check validity
- [ ] resetPassword(token, newPassword): Complete reset, invalidate token
- [ ] Rate limiting logic
- [ ] Error handling for all cases
- [ ] Comprehensive unit tests

## Dependencies
- Task 001 (repository)
- Task 002 (token service)
- Task 003 (email service)

---

# Task 005: API Controllers and Routes

## Scope
Create HTTP endpoints for password reset.

## Files
- src/controllers/PasswordResetController.ts
- src/controllers/PasswordResetController.test.ts
- src/routes/passwordReset.routes.ts

## Acceptance Criteria
- [ ] POST /api/auth/password-reset/request endpoint
- [ ] POST /api/auth/password-reset/confirm endpoint
- [ ] Request validation middleware
- [ ] Rate limiting middleware
- [ ] Error responses match spec
- [ ] Integration tests for both endpoints

## Dependencies
- Task 004 (password reset service)

---

# Task 006: Cleanup Job for Expired Tokens

## Scope
Scheduled job to clean up expired tokens.

## Files
- src/jobs/CleanupExpiredTokensJob.ts
- src/jobs/CleanupExpiredTokensJob.test.ts

## Acceptance Criteria
- [ ] Job runs daily at 2 AM
- [ ] Deletes tokens older than 24 hours
- [ ] Logs deletion count
- [ ] Error handling and retries

## Dependencies
- Task 001 (repository)
```

**Phase 4: Implementation (AI-Assisted)**

For each task:
1. Load spec, plan, and specific task
2. AI generates implementation
3. Human reviews against acceptance criteria
4. Run tests
5. Iterate if needed
6. Mark task complete
7. Move to next task

**Phase 5: Integration and Testing**

After all tasks:
1. Integration testing across all components
2. Security review (token handling, email enumeration, rate limiting)
3. Performance testing
4. Documentation review
5. Final approval

**Phase 6: Spec Evolution**

After deployment:
1. Monitor for issues
2. If bugs found → Update spec to cover the case
3. Regenerate from updated spec
4. Verify bug is fixed
5. Commit spec changes

## Measuring Success

### Quantitative Metrics

**Code Quality:**
```
Test Coverage: Target 90%+
Code Duplication: <5%
Cyclomatic Complexity: Average <10
Bug Rate: Bugs per feature (compare SDD vs non-SDD)
```

**Development Speed:**
```
Time to First Implementation: Faster with SDD (clear spec)
Time to Stable Implementation: Faster with SDD (fewer iterations)
Rework Time: Reduced with SDD (clear requirements)
```

**Spec Quality:**
```
Spec Clarity Score: Can junior dev understand it? (subjective)
Spec Completeness: % of questions answerable from spec
Regeneration Success: Code generated from spec matches first time (%)
```

### Qualitative Indicators

**Good signs:**
- ✅ AI generates code that matches spec on first try
- ✅ Few clarifying questions during implementation
- ✅ Code reviews focus on logic, not "what were you trying to do?"
- ✅ New team members can understand features from specs
- ✅ Features can be regenerated reliably

**Bad signs:**
- ❌ AI generates code that doesn't match spec
- ❌ Many back-and-forth iterations to get it right
- ❌ Specs are ignored during implementation
- ❌ Specs become stale (code diverges from spec)
- ❌ Team doesn't trust specs

## Common Pitfalls and Solutions

### Pitfall 1: AI Makes Costly Mistakes

**Problem:** AI rewrites entire files, creates duplicate functions, breaks existing features.

**Solution:**
- Be explicit in specs about what to modify vs. what to leave alone
- Reference existing code: "Use existing validateEmail() from utils/validation.ts"
- Spec should say: "Modify only the login function in auth.ts, keep all other functions unchanged"

### Pitfall 2: Specifications Become Stale

**Problem:** Code evolves, specs don't, they diverge.

**Solution:**
- Make spec updates part of PR requirement
- Spec changes reviewed like code changes
- CI checks that spec version matches code version
- Regular spec audits (quarterly)

### Pitfall 3: Specs Are Too Generic

**Problem:** "Make it secure", "Make it fast" → AI interprets differently each time.

**Solution:**
- Be specific: "Respond in <100ms at p95 for 1000 req/s load"
- Explicit tech choices: "Use bcrypt with 12 rounds" not "hash passwords securely"
- Concrete examples in specs

### Pitfall 4: Over-Engineering the Spec Process

**Problem:** Too many spec files, too much process, bureaucracy.

**Solution:**
- Start minimal: Just spec.md
- Add plan.md only for complex features
- Tasks only when needed for decomposition
- Don't force the process on trivial changes

### Pitfall 5: Treating Specs as Requirements Documents

**Problem:** Specs written for managers, not AI.

**Solution:**
- Audience is AI + developers, not stakeholders
- Focus on technical clarity, not business justification
- Omit unnecessary context
- Be direct and specific

## Integration with Existing Workflows

### SDD + Test-Driven Development (TDD)

**Compatible approach:**
```
1. Write spec (what)
2. Write tests from spec (acceptance criteria)
3. AI generates implementation
4. Tests validate implementation
5. Refactor if needed
```

**Why it works:** Spec defines "what", tests define "done", AI generates "how".

### SDD + Code Review

**Enhanced code review:**
```
Review 1: Spec review (before implementation)
  - Is spec clear?
  - Does it cover edge cases?
  - Is it testable?

Review 2: Implementation review (after generation)
  - Does code match spec?
  - Is code quality good?
  - Are tests sufficient?

Advantage: Two checkpoints instead of one
```

### SDD + CI/CD

**Pipeline integration:**
```yaml
# .github/workflows/sdd-validation.yml
name: Spec-Driven Validation

on: [pull_request]

jobs:
  spec-check:
    runs-on: ubuntu-latest
    steps:
      - name: Check spec exists for changed features
        run: ./scripts/check-spec-exists.sh

      - name: Validate spec format
        run: ./scripts/validate-spec.sh

      - name: Check spec version matches code
        run: ./scripts/check-spec-version.sh
```

### SDD + Agile/Scrum

**Sprint planning with specs:**
```
Sprint Planning:
1. Product Owner presents user stories
2. Team writes specs for each story
3. Team breaks specs into tasks
4. Team estimates tasks
5. Team commits to sprint backlog

During Sprint:
- Implement from specs
- Update specs when requirements change
- Review specs in dailies

Sprint Review:
- Demo working code
- Show specs that drove development
- Discuss spec evolution
```

## The Future of Spec-Driven Development

### Current State (2025)

- **Maturity:** Early adopter phase
- **Tools:** Experimental (Spec Kit), production-ready (Kiro), exploratory (Tessl)
- **Adoption:** Growing but niche
- **Level:** Mostly spec-first, some spec-anchored, rare spec-as-source

### Near-Term Evolution (2025-2026)

**Predicted developments:**
- Better AI understanding of specs (fewer iterations)
- Standardization of spec formats
- IDE integration (spec editors with AI assistance)
- Spec validation tools
- Spec-to-test generation
- More production-ready tools

### Long-Term Vision (2027+)

**Spec-as-source maturity:**
- Humans only edit specs
- Code is always generated
- IDE shows spec, generated code is hidden
- Deployment happens from spec, not code
- Version control tracks specs, code is ephemeral

**Formal verification integration:**
- Specs include formal properties
- Automated reasoning proves code correct
- Security properties verified automatically
- Performance guarantees proven

**Natural language specs:**
- Specs written in plain English
- AI handles formalization
- Specs readable by non-technical stakeholders

## Recommendations by Project Type

### Greenfield Projects (New Projects)

**Recommended:** Spec Kit or Kiro

**Approach:**
- Start with constitution (architecture, tech stack)
- Write spec for MVP features
- Generate implementation
- Iterate on both spec and code

**Level:** Spec-first or spec-anchored

### Brownfield Projects (Existing Codebases)

**Recommended:** Spec-first for new features

**Approach:**
- Don't spec existing code (too much work)
- Write specs for new features only
- Reference existing code in specs
- Gradually build spec coverage

**Level:** Spec-first

**Example spec for brownfield:**
```markdown
# Feature: Add OAuth Login

## Existing System
- Current authentication: email/password (see src/auth/)
- AuthService exists, handles JWT
- UserRepository exists, handles user CRUD

## New Feature
Add Google OAuth login alongside existing authentication.

## Integration Points
- Use existing AuthService.generateToken() for JWT
- Use existing UserRepository.createUser() for new users
- Use existing UserRepository.findByEmail() for linking accounts

## What to Build
- OAuthService (new)
- Google OAuth strategy (new)
- /api/auth/google/callback endpoint (new)
```

### Enterprise Applications

**Recommended:** Kiro (production focus)

**Approach:**
- Formal requirements process (EARS notation)
- Architecture review for design phase
- Team collaboration on specs
- Security and compliance reviews
- Spec-anchored for long-term maintenance

### Prototypes and MVPs

**Recommended:** Light spec-first (don't over-engineer)

**Approach:**
- Minimal specs (what, not how)
- Skip constitution (move fast)
- Skip task decomposition (small scope)
- Iterate rapidly

**When to use:** Validating ideas, throw-away prototypes

**When not to use:** If prototype will become production code

## Conclusion

Spec-Driven Development represents a fundamental shift in how we build software with AI:

**From:** Ad-hoc prompting (vibe coding) → Unpredictable code → Extensive debugging

**To:** Clear specifications → AI generation → Predictable, testable code

### Key Takeaways

1. ✅ **Specifications are executable** with AI coding assistants
2. ✅ **Start with one feature** to learn the approach
3. ✅ **Be extremely clear** about requirements and "done"
4. ✅ **Include tests** in specs from day one
5. ✅ **Version specs like code** using git
6. ✅ **Iterate on specs** just like code
7. ✅ **Maintain lessons learned** to improve spec quality
8. ❌ **Don't expect magic** - SDD is iterative, not "generate once"
9. ❌ **Don't skip human review** - AI makes mistakes
10. ❌ **Don't use SDD for everything** - it's overkill for trivial changes

### When to Use Spec-Driven Development

**Use SDD when:**
- Adding features to existing systems (most powerful use case)
- Requirements are clear and well-understood
- Team collaboration is important
- Long-term maintenance is expected
- Code quality and predictability matter

**Skip SDD when:**
- Making trivial changes (typos, dependency updates)
- Building throw-away prototypes
- Requirements are highly exploratory
- Working solo on small scripts

### The Bottom Line

Spec-Driven Development is not a silver bullet, but it's a significant improvement over "vibe coding" for substantial development work.

**Evidence:** Early adopters report 10x productivity increases for complex features, reduced technical debt, better team collaboration, and more predictable AI-assisted development.

**Maturity:** Still early adopter phase (2025), but rapidly evolving.

**Recommendation:** Try it on one feature. If it works for you, expand. If not, you've only invested time in one well-documented feature.

**The paradigm shift is real:** With AI, specifications become executable. This changes everything about how we think about documentation, requirements, and the role of code itself.

**The future:** Specifications may become the program, and code becomes a generated implementation detail. We're not there yet, but spec-driven development is the path forward.

---

**Further Reading:**
- GitHub Spec Kit: github.com/github/spec-kit
- Kiro: kiro.dev
- Martin Fowler's SDD Analysis: martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html
- EARS Notation: easyapproach.info
