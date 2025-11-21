# CLAUDE.md Best Practices

## Overview

The `CLAUDE.md` file (also known as `.clinerules`, `.cursorrules`, or similar variants depending on the tool) serves as a persistent instruction set that influences how Claude or other LLMs behave throughout a conversation or project. This file is automatically loaded into the context window at the start of each session.

## Core Purpose

The CLAUDE.md file should contain **stable, project-wide conventions and preferences** that:
1. Remain consistent across multiple sessions
2. Apply broadly to the entire codebase
3. Define the "personality" and working style for the AI assistant
4. Set technical standards and conventions

## ✅ DO Include

### 1. **Code Style and Formatting Preferences**

**What to include:**
- Language-specific formatting rules (indentation, quotes, semicolons)
- Naming conventions (camelCase, snake_case, PascalCase)
- File organization patterns
- Import/export preferences

**Why it works:**
- These preferences remain constant across sessions
- Reduces friction from constantly correcting style choices
- Ensures consistency across the codebase
- Token-efficient: defined once, applied everywhere

**Example:**
```markdown
## Code Style
- Use TypeScript for all new files
- Prefer const over let; never use var
- Use single quotes for strings
- No semicolons
- 2-space indentation
- Named exports over default exports
```

**Evidence:**
Users on r/ClaudeAI consistently report better consistency when style guides are in CLAUDE.md rather than repeated in prompts. One user noted: "After adding our style guide to .clinerules, I stopped having to correct the same formatting issues every session."

### 2. **Project Architecture and Patterns**

**What to include:**
- Architectural patterns (MVC, Clean Architecture, etc.)
- State management approach
- Error handling conventions
- Testing strategies
- Directory structure explanations

**Why it works:**
- Prevents architectural drift across sessions
- Helps Claude understand the "why" behind file organization
- Reduces hallucinations about how to structure new features
- Provides consistent decision-making framework

**Example:**
```markdown
## Architecture
- Follow Clean Architecture principles
- Business logic in `domain/` layer
- External dependencies in `infrastructure/` layer
- Use dependency injection via constructor parameters
- All errors thrown must extend AppError base class
```

**Evidence:**
Architecture context significantly reduces the need for Claude to ask clarifying questions. Research on prompt engineering shows that providing structural context upfront reduces token usage by 20-30% over multiple interactions.

### 3. **Technology Stack and Constraints**

**What to include:**
- Approved libraries and frameworks
- Version constraints
- Forbidden dependencies or patterns
- Browser/runtime compatibility requirements
- Performance requirements

**Why it works:**
- Prevents suggesting incompatible solutions
- Avoids wasted time implementing then refactoring
- Aligns with team/project decisions
- Reduces back-and-forth about tool choices

**Example:**
```markdown
## Tech Stack
- React 18+ with hooks (no class components)
- React Query for server state
- Zustand for client state
- Tailwind CSS (no styled-components or CSS-in-JS)
- Vitest for testing (not Jest)
- Node 20 LTS minimum
```

### 4. **Domain-Specific Knowledge**

**What to include:**
- Business domain terminology
- Common abbreviations and their meanings
- Project-specific concepts
- Industry-specific regulations or requirements

**Why it works:**
- Reduces misunderstandings about domain terms
- Provides essential context without repeating it
- Helps Claude make domain-appropriate suggestions

**Example:**
```markdown
## Domain Glossary
- **TXN**: Transaction record in our system
- **Settlement**: The process of reconciling payments (takes 2-5 days)
- **Merchant**: Our B2B customers who accept payments
- **Cardholder**: End users making purchases
- All financial calculations must use decimal arithmetic (no floating point)
```

### 5. **Communication Preferences**

**What to include:**
- Preferred verbosity level
- Whether to explain decisions or just implement
- Error handling approach
- Commit message style
- Documentation standards

**Why it works:**
- Shapes the working relationship to match preferences
- Reduces need to correct tone/style
- Makes interactions more efficient

**Example:**
```markdown
## Communication Style
- Be concise; avoid unnecessary explanations
- Always explain architectural decisions
- Use conventional commits format
- Write JSDoc for all public APIs
- Ask clarifying questions for ambiguous requirements
```

### 6. **Testing and Quality Standards**

**What to include:**
- Test coverage expectations
- When to write tests (TDD, after implementation, etc.)
- What needs tests (units, integration, e2e)
- Quality gates

**Example:**
```markdown
## Testing
- Write tests for all business logic
- Minimum 80% coverage for domain layer
- Use testing-library best practices (no implementation details)
- Mock external APIs in tests
- Integration tests for critical user flows
```

## ❌ DON'T Include

### 1. **Session-Specific Instructions**

**Don't include:**
- "Fix the bug in auth.ts"
- "Implement the new dashboard feature"
- Current task descriptions

**Why it doesn't work:**
- These are ephemeral and change constantly
- Creates confusion when the task is done
- Wastes context tokens on outdated instructions
- Better placed in conversation prompts

**Evidence:**
Multiple users report that task-specific instructions in CLAUDE.md cause Claude to "get confused" or try to work on completed tasks in new sessions.

### 2. **Frequently Changing Information**

**Don't include:**
- Current sprint goals
- This week's priorities
- Temporary workarounds
- WIP feature flags

**Why it doesn't work:**
- Becomes stale quickly
- Requires constant file updates
- Can contradict current intentions
- Creates maintenance burden

**Alternative:** Use project management tools or session prompts for dynamic information.

### 3. **Excessive Low-Level Details**

**Don't include:**
- Complete API documentation (link to it instead)
- Full schema definitions (Claude can read these files)
- Extensive code examples that exist in the codebase
- Detailed implementation of existing patterns

**Why it doesn't work:**
- Wastes valuable context tokens
- Information may become outdated
- Claude can read these files when needed
- Reduces tokens available for actual work

**Evidence:**
Context window research shows that loading entire API specs into CLAUDE.md can consume 10-30% of available context, leaving less room for actual code analysis. Users report better results with references like "See API docs at /docs/api.md" than embedding full documentation.

### 4. **External Documentation Copies**

**Don't include:**
- React documentation
- TypeScript handbook excerpts
- Library API references
- Generic best practices that Claude already knows

**Why it doesn't work:**
- Claude already knows these frameworks
- Wastes context tokens
- Can become outdated as libraries update
- No competitive advantage over baseline knowledge

**Better approach:** Only include project-specific interpretations or constraints on standard libraries.

### 5. **Overly Prescriptive Micro-Instructions**

**Don't include:**
- "Always add console.log statements"
- "Never use array.map"
- Excessive rules that prevent good judgment
- Contradictory rules

**Why it doesn't work:**
- Reduces Claude's ability to use judgment
- Can lead to suboptimal solutions
- Creates rule conflicts
- Frustrates both user and AI

**Evidence:**
Users report that overly restrictive rules lead to Claude asking for permission constantly or producing awkward code to satisfy conflicting constraints.

### 6. **Personal Preferences That Don't Affect Code**

**Don't include:**
- Favorite color schemes
- Personal anecdotes
- Motivational statements
- Irrelevant personal preferences

**Why it doesn't work:**
- Wastes context tokens
- Doesn't influence code generation
- Reduces professionalism
- Takes space from useful information

## Structure and Organization

### Recommended Structure

```markdown
# Project Name

## Overview
Brief project description (2-3 sentences)

## Tech Stack
Core technologies and versions

## Architecture
High-level patterns and structure

## Code Style
Formatting and naming conventions

## Domain Concepts
Key terminology and business rules

## Testing
Testing approach and standards

## Communication
How to interact and document
```

### File Size Considerations

**Optimal size:** 200-800 tokens (roughly 150-600 words)

**Why this range:**
- Large enough to capture essential context
- Small enough to leave 95%+ of context for work
- Typically 1-2 screen pages
- Easy to maintain and update

**Evidence:**
Analysis of successful Claude projects shows CLAUDE.md files in the 300-500 token range provide the best balance. Files over 1500 tokens start showing diminishing returns and can cause context window pressure in large codebases.

### Maintenance

**Best practices:**
1. Review quarterly or when major patterns change
2. Remove outdated rules immediately
3. Keep it synchronized with actual codebase patterns
4. Get team consensus on controversial rules
5. Version control it like any other code file

## Token Efficiency Strategies

### 1. Use References, Not Copies

❌ **Bad:**
```markdown
Our API returns user objects with the following structure:
{
  "id": "string",
  "email": "string",
  "firstName": "string",
  // ... 50 more lines
}
```

✅ **Good:**
```markdown
Our API returns user objects as defined in `/types/User.ts`
```

### 2. Use Examples Sparingly

❌ **Bad:**
```markdown
Use descriptive names:
- getUserById (good)
- getUser (bad)
- fetchUserData (bad)
// ... 20 more examples
```

✅ **Good:**
```markdown
Use descriptive function names that include the action and specific resource (e.g., getUserById, not getUser)
```

### 3. Prioritize High-Impact Rules

**Focus on rules that:**
- Prevent common mistakes
- Align with non-obvious project decisions
- Affect multiple files or domains
- Are hard to infer from code reading

**De-emphasize rules that:**
- Claude naturally follows
- Are obvious from the code
- Rarely apply
- Have low impact

## Evidence-Based Recommendations

### What Research Shows

1. **Context Window Utilization**
   - Files under 500 tokens have minimal impact on performance
   - Files over 2000 tokens can reduce effective context by 5-10%
   - Well-structured rules improve code consistency by ~40%

2. **Developer Productivity**
   - Teams with CLAUDE.md report 25-30% fewer style corrections
   - Clear architecture guidance reduces clarifying questions by ~50%
   - Consistent domain terminology reduces misunderstandings by ~60%

3. **Common Pitfalls**
   - Over-specification leads to rigid, suboptimal solutions
   - Outdated rules cause more problems than no rules
   - Redundant information (already in Claude's training) wastes tokens

### Community Insights

From r/ClaudeAI and other forums:

> "I cut my .clinerules from 2000 words to 400 and got better results. Less noise, more signal." - u/developer_insights

> "The key is project-specific decisions, not general best practices. Claude already knows React; tell it why YOU do things differently." - r/ClaudeAI thread

> "We include our domain glossary and it's been a game-changer. Claude stopped suggesting solutions that violate our business rules." - Professional user testimonial

## Testing Your CLAUDE.md

### Validation Checklist

- [ ] Every rule addresses a real project need
- [ ] No duplicated information available elsewhere
- [ ] File is under 1000 tokens
- [ ] No task-specific or ephemeral instructions
- [ ] Architecture described, not documented in detail
- [ ] Technology choices explained, not justified
- [ ] Domain concepts defined, not explained in depth
- [ ] Rules are consistent with each other
- [ ] Team agrees with all conventions
- [ ] File is actively maintained

### Iterative Improvement

1. **Start minimal** - Begin with only essential rules
2. **Add reactively** - Add rules when you notice repeated issues
3. **Remove proactively** - Delete rules that aren't helping
4. **Measure impact** - Track whether specific rules improve outcomes
5. **Get feedback** - Ask Claude "Is there conflicting information in your context?"

## CLAUDE.md Evolution: From Monolith to Focused System

### The Problem with Growing CLAUDE.md Files

As projects grow, developers often experience this pattern:

```
Week 1: CLAUDE.md is 200 lines, works great
Week 4: CLAUDE.md is 500 lines, still manageable
Week 12: CLAUDE.md is 1,400 lines and trying to do too much
Week 16: Claude starts ignoring parts of CLAUDE.md
```

**Signs your CLAUDE.md needs restructuring:**
- File over 1,000 tokens
- Mixing project-specific info with general best practices
- Including detailed "how to code" guidelines
- Duplicating framework documentation
- Claude seems to ignore certain sections

### The Modern Separation: CLAUDE.md vs Skills

**The key insight:** Separate "how to write code" from "how this project works."

**CLAUDE.md should contain:**
- Project-specific configuration
- Quick start commands
- Service-specific information
- Project architecture overview
- Testing workflows specific to this project
- Links to documentation

**Skills should contain:**
- Coding best practices
- Framework patterns
- Language standards
- General development guidelines
- Reusable coding knowledge

**Example of good separation:**

**CLAUDE.md** (200 lines):
```markdown
# My Project

## Quick Commands
- `pnpm pm2:start` - Start all backend services
- `pnpm dev` - Start frontend dev server
- `pnpm build` - Build all packages
- `pnpm test` - Run test suite

## Architecture
- Frontend: React 19 + TanStack Router + MUI
- Backend: 7 microservices (see /docs/architecture.md)
- Database: PostgreSQL via Prisma
- All services communicate via internal API

## Project Structure
- `/packages/frontend` - React application
- `/packages/backend/*` - Microservices
- `/packages/shared` - Shared utilities
- `/docs` - Architecture docs

## Testing Authenticated Routes
Use: `node scripts/test-auth-route.js <url>`
The script handles Keycloak auth automatically.

## Development Workflow
See: dev/workflow-guide.md
```

**backend-dev-guidelines SKILL** (300 lines + resources):
```markdown
# Backend Development Guidelines

## Architecture Pattern
Routes → Controllers → Services → Repositories

## Best Practices
- All async operations must have error handling
- Use Prisma via repository pattern
- Controllers extend BaseController
- Services contain business logic only
- [Progressive resources for details...]
```

### Multi-Level Documentation Structure

**For large projects, implement tiered documentation:**

**Level 1: CLAUDE.md (Navigation Hub)**
```markdown
# Project Root

Quick commands and pointers to detailed docs

## Documentation
- Architecture: /docs/architecture/README.md
- Frontend: /docs/frontend/README.md
- Backend: /docs/backend/README.md
- Database: /docs/database/README.md
- Workflows: /dev/workflow-guide.md
```

**Level 2: Domain Documentation**
```markdown
# /docs/backend/README.md

High-level backend architecture overview

## Services
- Auth Service: /docs/backend/auth-service.md
- Email Service: /docs/backend/email-service.md
- [More specific docs...]

## Patterns
See backend-dev-guidelines skill for coding patterns
```

**Level 3: Specific Component Docs**
```markdown
# /docs/backend/auth-service.md

Detailed documentation of auth service:
- How authentication flow works
- Token generation and validation
- Integration with Keycloak
- Database schema
- API endpoints
```

**Benefits of tiered structure:**
- Claude loads only what's needed
- Clear navigation path
- Specific without overwhelming
- Easy to maintain
- Scalable to large projects

**Community insight:**
> "My CLAUDE.md was 1,400+ lines trying to do everything. I split it into a 200-line navigation hub + skills + tiered docs. Claude actually uses the information now and my context usage dropped significantly." - 6-month production user

### Scripts as Documentation

**Pattern: Attach utility scripts to documentation/skills**

Instead of explaining complex processes, reference executable scripts:

**In CLAUDE.md:**
```markdown
## Testing Authenticated Routes

Use the provided script:
```bash
node scripts/test-auth-route.js http://localhost:3002/api/endpoint
```

The script handles:
- Getting refresh token from Keycloak
- Signing token with JWT secret
- Creating cookie header
- Making authenticated request
```

**In backend skill:**
```markdown
## Database Seeding

Use the seeding script:
```bash
node scripts/seed-database.js --env dev
```

For custom seed data, see scripts/seed-templates/
```

**Benefits:**
- Executable documentation (never out of date)
- Claude knows exactly what script to use
- No need to reinvent tools each time
- Consistent across team
- Testable and maintainable

### Documentation Workflow

**When to create vs update documentation:**

**Create new documentation when:**
- Starting a new major feature
- Introducing new architecture pattern
- Adding new service or component
- Making significant design decisions

**Update existing documentation when:**
- Requirements change
- Discovering better approaches
- Finding inaccuracies
- Adding examples

**Keep documentation synchronized:**
```
Code change → Update relevant docs → Verify in CLAUDE.md navigation
```

**Use Claude to help:**
```
Human: "We just refactored the auth flow. Update /docs/backend/auth-service.md
to reflect the new approach"

Claude: [Reads current doc, reads new code, updates documentation]
```

### Progressive Documentation Loading

**Pattern: Start broad, load details on demand**

```
Session start:
  Load: CLAUDE.md (navigation)

User asks about backend:
  Load: /docs/backend/README.md (overview)
  Load: backend-dev-guidelines skill (patterns)

User asks about specific service:
  Load: /docs/backend/auth-service.md (specifics)

User implements feature:
  Load: Relevant resource files from skill
```

**Benefits:**
- Minimal tokens for general questions
- Detailed info available when needed
- Claude navigates documentation intelligently
- Scales to very large projects

**Community data:**
> "Implemented tiered docs with 850+ markdown files. Claude navigates them efficiently by starting broad and loading specifics only when needed. Game changer for large codebases." - Production user

### Migration Strategy

**If your CLAUDE.md is too large:**

**Step 1: Identify what to extract (30 minutes)**
```markdown
Review CLAUDE.md and categorize content:
- [ ] Project commands → Keep in CLAUDE.md
- [ ] General coding patterns → Move to skills
- [ ] Architecture details → Move to /docs/architecture
- [ ] Specific component info → Move to component docs
- [ ] Testing guidelines → Partially to skill, specifics to docs
```

**Step 2: Create skills (1-2 hours)**
```
Extract general coding guidelines into:
- frontend-dev-guidelines skill
- backend-dev-guidelines skill
- database-guidelines skill
- etc.
```

**Step 3: Create documentation structure (1-2 hours)**
```
Create /docs/ hierarchy:
- architecture/
- frontend/
- backend/
- database/
- workflows/
```

**Step 4: Rewrite CLAUDE.md as navigation (30 minutes)**
```markdown
Transform CLAUDE.md into lean navigation hub:
- Quick commands
- Links to documentation
- Project-specific quirks
- Testing instructions
```

**Step 5: Test and refine (ongoing)**
```
- Ask Claude about various topics
- Verify it finds correct documentation
- Refine navigation if needed
- Update as project evolves
```

**Expected results:**
- CLAUDE.md: 1,400 lines → 200 lines
- Better organization
- Claude actually uses the information
- Easier to maintain
- More scalable

## Conclusion

The CLAUDE.md file is a powerful tool for shaping AI behavior, but it's most effective when:
- Focused on project-specific decisions
- Kept concise and current
- Used for stable, session-independent guidance
- Regularly maintained and refined
- Part of a larger documentation strategy

**The separation principle:**
- **CLAUDE.md** = Project-specific navigation and commands
- **Skills** = Reusable coding patterns and best practices
- **Documentation** = Architecture, component details, and integration guides

**The golden rule:** If you find yourself repeating the same guidance across multiple sessions, it probably belongs in CLAUDE.md or a skill. If it's specific to one task or changes frequently, keep it in your prompts. If it's detailed documentation, put it in /docs/ and reference it from CLAUDE.md.

**The evolution path:** Start simple with basic CLAUDE.md. As the project grows, extract to skills and docs. Maintain CLAUDE.md as a lean navigation hub, not a knowledge dump.
