# Setup Plan: Claude Code for Large Legacy Java EE Project

**Project Context:** 2M+ LOC monolithic legacy Java EE application (25 years old) with custom in-house ORM, data model, and numerous violations of modern software engineering principles.

**Objective:** Set up Claude Code infrastructure to enable safe, incremental improvements while managing the extreme context window challenges posed by the codebase size and complexity.

---

## Executive Summary

This plan establishes a complete Claude Code setup optimized for working in a large, brittle legacy Java EE monolith. The setup emphasizes:

1. **Context Window Management** - Critical for 2M+ LOC codebase
2. **Safety-First Workflows** - Prevent breaking changes in brittle code
3. **Incremental Progress** - Small, testable steps to manage complexity
4. **Test-Driven Approach** - Required for confident refactoring

**Estimated Setup Time:** 4-6 hours

**Components to Create:**
- 1 CLAUDE.md file (project conventions and architecture overview)
- 6 specialized agents (planning, implementation, refactoring, testing, review, security)
- 4-6 skills (legacy code patterns, Java EE best practices, testing strategies)
- Dev docs infrastructure (for managing multi-session work)
- Supporting workflow scripts and templates

---

## Phase 1: Create CLAUDE.md (60-90 minutes)

### Purpose
Navigation hub and project-specific conventions that are ALWAYS loaded. Keep under 800 tokens (~600 words).

### Location
`CLAUDE.md` (project root)

### What to Include

#### 1. Project Overview (100-150 words)
```markdown
# Legacy Java EE Application

## Overview
25-year-old monolithic Java EE application (~2M LOC) with custom in-house ORM and data model. Original architecture predates modern practices; contains technical debt, anti-patterns, and brittle integrations.

**Critical Constraints:**
- Custom ORM (not JPA/Hibernate) - migration in progress
- Complex interdependencies - seemingly small changes can break distant components
- Limited test coverage - most code untested
- Mixed technology stack - Java EE 6/7/8 with custom frameworks
- Database-heavy - direct JDBC and proprietary ORM
- Monolithic deployment - single WAR file, long build times
```

#### 2. Technology Stack (100 words)
```markdown
## Tech Stack

**Core:**
- Java 8 (targeting Java 17 migration)
- Java EE 7 (EJB 3.2, CDI 1.2, JAX-RS 2.0, JPA 2.1 where used)
- Custom in-house ORM (proprietary, underdocumented)
- Oracle Database 12c
- WildFly 14 (upgrading from JBoss EAP 6)

**Build:**
- Maven 3.6+
- Build time: ~20 minutes full, ~5 minutes incremental

**Testing:**
- JUnit 4 (migrating to JUnit 5)
- Mockito 3.x
- Arquillian 1.x for integration tests
- Limited coverage: ~15% overall, <5% in legacy modules
```

#### 3. Architecture Patterns (150 words)
```markdown
## Architecture

**Current State (Legacy):**
- Monolithic layered architecture with significant violations
- Layers: JSP/JSF → Managed Beans → Service Beans (EJBs) → DAO → Database
- Common anti-patterns:
  - Business logic in JSPs
  - Direct database access bypassing DAOs
  - God classes (500-2000 lines)
  - Circular dependencies
  - Static utility hell
  - Transaction boundaries unclear

**Target State (Incremental Migration):**
- Clean layered architecture: API → Service → Repository → Database
- Domain-Driven Design principles where applicable
- SOLID principles
- Dependency Injection via CDI (replacing programmatic lookups)
- Proper transaction management

**Migration Strategy:**
- Strangler Fig Pattern - incrementally replace legacy code
- Add tests before refactoring (Working Effectively with Legacy Code)
- Small steps - 30-60 minute increments
- Always leave code in working state
```

#### 4. Critical Development Principles (200 words)
```markdown
## Development Principles (CRITICAL)

### Safety First
1. **NEVER refactor without tests** - Create characterization tests first
2. **Small increments** - 30-60 minute steps, <150 lines per change
3. **Continuous testing** - Run tests after every change
4. **Working state** - Every commit must compile and pass tests
5. **Reversibility** - Prefer easily undoable changes

### Test-Driven Approach
- **Characterization tests** - Document existing behavior before changes
- **Red-Green-Refactor** - Tests first, then implementation
- **Coverage targets** - 80%+ for new/modified code, 100% critical paths
- **Integration testing** - Required for database/external system interactions

### Code Quality Standards
- **Clean Code** - Methods <20 lines, classes <300 lines, clear naming
- **Effective Java** - Immutability, proper use of collections, defensive copies
- **SOLID principles** - Single Responsibility especially critical
- **Refactoring patterns** - Use proven patterns from Martin Fowler's catalog

### Context Window Management (CRITICAL FOR THIS PROJECT)
- **Use Dev Docs Pattern** - All tasks >2 hours require plan.md, context.md, tasks.md
- **Frequent updates** - Update context.md after every milestone
- **Break down work** - Never work on >2 hours of changes without checkpoints
- **Read docs first** - Always load relevant dev docs when resuming work
```

#### 5. Common Workflows and Commands (50 words)
```markdown
## Workflows

**For any task:**
1. Use Architecture Planning Agent for requirements analysis and planning
2. Create dev docs (plan.md, context.md, tasks.md) for complex work
3. Use appropriate specialized agent (implementation, refactoring, testing)
4. Use Code Review Agent before committing
5. Use Security Vulnerability Agent for security-sensitive changes

**See:** `/docs/workflows/` for detailed workflow documentation
```

#### 6. Documentation Structure (50 words)
```markdown
## Documentation

**Project Docs:**
- `/docs/architecture/` - System architecture, integration points
- `/docs/legacy-patterns/` - Common legacy patterns and how to refactor them
- `/docs/workflows/` - Development workflows and procedures
- `/docs/domain/` - Business domain concepts and glossary

**Active Work:**
- `/dev/active/[task-name]/` - Dev docs for in-progress work (plan, context, tasks)
```

### What NOT to Include
- ❌ Detailed API documentation (reference file paths instead)
- ❌ Complete coding standards (put in skills instead)
- ❌ Specific feature requirements (put in dev docs instead)
- ❌ Generic best practices Claude already knows (focus on project-specific)
- ❌ Temporary task lists or current sprint goals

### Validation Checklist
- [ ] Under 800 tokens (~600 words)
- [ ] Project-specific (not generic advice)
- [ ] References detailed docs (doesn't duplicate them)
- [ ] Emphasizes critical constraints (brittle codebase, context management)
- [ ] Clear navigation to specialized agents and workflows

---

## Phase 2: Create Specialized Agents (90-120 minutes)

### Purpose
Specialized agents enforce safety guardrails and best practices for working in brittle legacy code.

### Location
`.claude/agents/` directory structure:
```
.claude/agents/
├── planning/
│   └── architecture-planning-agent.md
├── implementation/
│   └── feature-implementation-agent.md
├── refactoring/
│   └── legacy-refactoring-agent.md
├── testing/
│   └── test-coverage-agent.md
├── review/
│   └── code-review-agent.md
└── security/
    └── security-vulnerability-agent.md
```

### Agent Definitions to Create

#### 1. Architecture Planning Agent
**File:** `.claude/agents/planning/architecture-planning-agent.md`

**Purpose:** Ensure complete requirements understanding and create safe, incremental plans

**Key Sections:**
- Role: Requirements analysis and TDD-first planning for legacy Java EE
- Responsibilities: Clarify requirements, identify use/edge cases, design test-first plans
- Critical Workflow:
  - Phase 1: Requirements Clarification (40% time) - Socratic questioning
  - Phase 2: Test Case Design (25%) - Before implementation planning
  - Phase 3: Architecture & Implementation Plan (25%) - Tests → Interface → Skeleton → Implementation
  - Phase 4: Plan Review (10%) - Validation and user approval
- Output: Requirements doc, test plan, architecture design, 10-20 small steps
- Quality Standards: Completeness checklist, test coverage standards, small steps (30-60 min)

**Use the condensed example:** `/examples/legacy-java-ee/planning/architecture-planning-agent-condensed.md`

**Customizations for your project:**
- Add section on "Legacy System Integration Considerations"
- Include checklist for custom ORM compatibility
- Add decision tree for when to use custom ORM vs JPA
- Include risk assessment for changes in brittle areas

#### 2. Feature Implementation Agent
**File:** `.claude/agents/implementation/feature-implementation-agent.md`

**Purpose:** Execute implementation plans in small, safe TDD increments

**Key Sections:**
- Role: TDD execution specialist for legacy Java EE
- Responsibilities: Execute step-by-step, tests-first, small increments, keep working state
- Critical Workflow:
  - Step 0: Validate plan exists
  - For each step: RED (tests) → Interface → Skeleton → GREEN (implementation) → Refactor
  - Between steps: Validation (tests pass, compiles, working state)
- Output: Working, tested code with clear commit history
- Quality Standards: F.I.R.S.T. test principles, Clean Code, max 150 lines per step
- Breaking down large steps: STOP if >1 hour or >150 lines

**Use the condensed example:** `/examples/legacy-java-ee/implementation/feature-implementation-agent-condensed.md`

**Customizations for your project:**
- Add section on "Testing with Custom ORM" (mocking strategies)
- Include guidelines for transaction boundary testing
- Add checklist for verifying no breaking changes to distant components
- Include performance testing requirements (critical for DB-heavy operations)

#### 3. Legacy Code Refactoring Agent
**File:** `.claude/agents/refactoring/legacy-refactoring-agent.md`

**Purpose:** Safely transform legacy code using proven techniques

**Key Sections:**
- Role: Safe refactoring specialist for brittle legacy code
- Responsibilities: Characterization tests, dependency breaking, incremental refactoring
- Critical Workflow:
  - Phase 1: Requirements Understanding (20%) - Document actual behavior
  - Phase 2: Test-Driven Safety Net (30%) - Characterization tests, break dependencies
  - Phase 3: Skeleton Design (15%) - Interfaces and contracts only
  - Phase 4: Incremental Implementation (35%) - Small, tested steps
- Output: Analysis report, test suite, skeleton, refactored code, summary
- Quality Standards: Behavior preservation, max 100 lines per commit, 80%+ coverage
- Refactoring Patterns: Extract Method, Replace Conditional, Introduce Parameter Object

**Use the condensed example:** `/examples/legacy-java-ee/refactoring/legacy-code-refactoring-agent-condensed.md`

**Customizations for your project:**
- Add section on "Refactoring Custom ORM Usage"
- Include patterns for "Breaking Static Dependencies" (common in legacy code)
- Add checklist for "Circular Dependency Detection and Resolution"
- Include guidelines for "Strangler Fig Pattern" application

#### 4. Test Coverage Agent
**File:** `.claude/agents/testing/test-coverage-agent.md`

**Purpose:** Bring untested legacy code under test coverage

**Key Sections:**
- Role: Legacy code testing specialist
- Responsibilities: Characterization tests, dependency breaking, comprehensive coverage
- Critical Workflow:
  - Phase 1: Requirements Understanding (25%) - Map behavior and dependencies
  - Phase 2: Test Infrastructure Design (20%) - Seams, test doubles, fixtures
  - Phase 3: Write Tests (40%) - Characterization first, then comprehensive
  - Phase 4: Validate and Document (15%) - Coverage metrics, recommendations
- Output: Behavior map, test infrastructure, passing test suite, coverage report
- Quality Standards: F.I.R.S.T. principles, AAA pattern, 80%+ coverage
- Techniques: Seams, Extract and Override, Parameterize Constructor

**Use the condensed example:** `/examples/legacy-java-ee/testing/test-coverage-agent-condensed.md`

**Customizations for your project:**
- Add section on "Testing Code Using Custom ORM" (specific mocking strategies)
- Include patterns for "Testing Legacy EJBs Without Container"
- Add guidelines for "Database Integration Testing" (test containers vs in-memory)
- Include section on "Arquillian Best Practices for Legacy Code"

#### 5. Code Review Agent
**File:** `.claude/agents/review/code-review-agent.md`

**Purpose:** Thorough reviews focused on safety in brittle codebase

**Key Sections:**
- Role: Senior reviewer for legacy Java EE
- Responsibilities: Clean Code, Effective Java, security (OWASP), performance, maintainability
- Critical Workflow:
  - Context Understanding (15%)
  - Code Analysis (50%) - violations, security, performance
  - Feedback Formulation (25%) - prioritized, actionable, with examples
  - Review Summary (10%)
- Output: Review summary (approve/request changes), prioritized comments, recommendations
- Quality Standards: Clean Code checklist, Effective Java, Security (OWASP), Performance
- Decision Criteria: Request changes for critical issues, approve only when safe

**Use the condensed example:** `/examples/legacy-java-ee/code-review/code-review-agent-condensed.md`

**Customizations for your project:**
- Add section on "Legacy Code Compatibility Review" (breaking changes detection)
- Include checklist for "Custom ORM Usage Review" (anti-patterns)
- Add focus on "Performance Review for Database Operations" (N+1 queries)
- Include "Circular Dependency Detection" in review checklist

#### 6. Security Vulnerability Agent
**File:** `.claude/agents/security/security-vulnerability-agent.md`

**Purpose:** Identify and remediate security vulnerabilities

**Key Sections:**
- Role: Security specialist for legacy Java EE
- Responsibilities: OWASP Top 10 audits, secure remediation, defense in depth
- Critical Workflow:
  - Phase 1: Security Assessment (30%) - OWASP framework, threat model, prioritization
  - Phase 2: Security Test Design (20%) - Vulnerability demonstrations
  - Phase 3: Remediation Implementation (35%) - Fix by priority, apply secure patterns
  - Phase 4: Validation and Documentation (15%)
- Output: Security assessment, test suite, remediated code, documentation
- Quality Standards: Input validation, output encoding, parameterized queries, secure auth/authz
- Example Remediations: SQL injection, XSS, broken authentication

**Use the condensed example:** `/examples/legacy-java-ee/security/security-vulnerability-agent-condensed.md`

**Customizations for your project:**
- Add section on "Custom ORM Security Considerations" (injection risks)
- Include guidelines for "Legacy Authentication/Authorization Audit"
- Add checklist for "Database Direct Access Security Review"
- Include section on "Session Management in Legacy Java EE"

### Agent Setup Instructions

For each agent:

1. **Copy condensed example** to appropriate `.claude/agents/` subdirectory
2. **Customize** with project-specific sections (listed above)
3. **Test agent** by invoking it with a sample task
4. **Refine** based on effectiveness

---

## Phase 3: Create Skills (60-90 minutes)

### Purpose
Reusable coding patterns and best practices that Claude automatically applies when relevant.

### Location
`skills/` directory (in user's home or project-specific):
```
skills/
├── legacy-java-ee-patterns/
│   └── SKILL.md
├── clean-code-java/
│   └── SKILL.md
├── working-with-custom-orm/
│   └── SKILL.md
├── characterization-testing/
│   └── SKILL.md
├── database-performance/
│   └── SKILL.md
└── safe-refactoring-techniques/
    └── SKILL.md
```

### Skill Definitions to Create

#### 1. Legacy Java EE Patterns Skill
**File:** `skills/legacy-java-ee-patterns/SKILL.md`

**Metadata:**
```yaml
---
name: legacy-java-ee-patterns
description: Working with legacy Java EE patterns, anti-patterns, and migration strategies for 25-year-old monolithic applications. Use when encountering God classes, circular dependencies, static utility hell, or complex legacy EJB code.
---
```

**Core Content:**
- Common legacy anti-patterns and how to recognize them
- Safe refactoring strategies for each anti-pattern
- Migration paths (custom ORM → JPA, programmatic lookups → CDI)
- Strangler Fig Pattern application
- Breaking circular dependencies
- Extracting business logic from JSPs
- Progressive resources: `/docs/legacy-patterns/`

**Keep SKILL.md under 500 lines** - reference detailed docs for specifics

#### 2. Clean Code for Java Skill
**File:** `skills/clean-code-java/SKILL.md`

**Metadata:**
```yaml
---
name: clean-code-java
description: Applying Clean Code and Effective Java principles to legacy Java codebases. Use when writing new code, refactoring existing code, or reviewing implementations for maintainability and readability.
---
```

**Core Content:**
- Meaningful names (classes=nouns, methods=verbs, reveal intent)
- Function rules (small <20 lines, single responsibility, <4 params)
- Error handling (exceptions not codes, provide context, don't return null)
- Effective Java key items:
  - Item 1: Static factory methods
  - Item 17: Minimize mutability
  - Item 49: Check parameters
  - Item 54: Return empty collections not nulls
- Code organization (single level of abstraction, organize by proximity)
- Examples of before/after refactorings

#### 3. Working with Custom ORM Skill
**File:** `skills/working-with-custom-orm/SKILL.md`

**Metadata:**
```yaml
---
name: working-with-custom-orm
description: Best practices for working with the proprietary in-house ORM, understanding its patterns, limitations, and migration strategies to JPA. Use when dealing with database access, ORM-related bugs, or planning migrations.
---
```

**Core Content:**
- Custom ORM API overview (common patterns)
- Key differences from JPA/Hibernate
- Common pitfalls and how to avoid them
- Performance considerations (N+1 queries, lazy loading)
- Testing strategies (mocking the custom ORM)
- Migration patterns (Custom ORM → JPA Repository pattern)
- When to use custom ORM vs when to migrate to JPA
- Examples of safe migration steps

**Note:** You'll need to fill this in based on your actual custom ORM

#### 4. Characterization Testing Skill
**File:** `skills/characterization-testing/SKILL.md`

**Metadata:**
```yaml
---
name: characterization-testing
description: Creating characterization tests to document existing behavior of legacy code before refactoring. Use when preparing to refactor untested code, fixing bugs in legacy systems, or documenting unclear behavior.
---
```

**Core Content:**
- What characterization tests are and why they matter
- How to write characterization tests (document actual behavior, not intended)
- Breaking dependencies for testing:
  - Seam identification
  - Extract and Override
  - Parameterize Constructor
  - Sprout Method/Class
  - Wrap Method/Class
- Testing strategies for untestable code
- Examples from "Working Effectively with Legacy Code"
- Step-by-step process:
  1. Identify behavior to test
  2. Find seam
  3. Break dependency
  4. Write test
  5. Verify test locks down behavior

#### 5. Database Performance Skill
**File:** `skills/database-performance/SKILL.md`

**Metadata:**
```yaml
---
name: database-performance
description: Identifying and fixing database performance issues in Java EE applications including N+1 queries, missing indexes, inefficient queries, and connection pool management. Use when investigating slow database operations or optimizing data access.
---
```

**Core Content:**
- Common database performance issues:
  - N+1 query problem (detection and fixing)
  - SELECT * anti-pattern
  - Missing indexes
  - Inefficient joins
  - Lazy loading pitfalls
  - Connection pool exhaustion
- Profiling database access (logging, query analysis)
- Optimization strategies:
  - Batch operations
  - Proper fetch strategies
  - Query optimization
  - Caching strategies
- Testing performance (benchmarking, test containers)
- ORM-specific considerations (custom ORM vs JPA)

#### 6. Safe Refactoring Techniques Skill
**File:** `skills/safe-refactoring-techniques/SKILL.md`

**Metadata:**
```yaml
---
name: safe-refactoring-techniques
description: Applying safe, behavior-preserving refactoring techniques from Martin Fowler's Refactoring catalog. Use when improving code structure, reducing complexity, or preparing code for new features while maintaining safety in brittle codebases.
---
```

**Core Content:**
- Refactoring catalog (most common patterns):
  - Extract Method (>50 lines → smaller methods)
  - Extract Class (>300 lines → multiple classes)
  - Introduce Parameter Object (>3 params → object)
  - Replace Conditional with Polymorphism (complex if/else → polymorphism)
  - Replace Temp with Query (temporary variables → methods)
  - Move Method (Feature Envy → move to appropriate class)
- When to apply each refactoring
- Step-by-step mechanics for each
- Safety checks (tests must pass after each micro-step)
- Examples specific to legacy Java EE code
- IDE refactoring support (when safe to use automated tools)

### Skill Setup Instructions

For each skill:

1. **Create SKILL.md** with proper YAML frontmatter
2. **Keep core instructions under 500 lines**
3. **Use progressive disclosure** - reference detailed docs for advanced topics
4. **Test skill** - verify Claude invokes it automatically when relevant
5. **Iterate** - refine description if Claude doesn't invoke when expected

---

## Phase 4: Set Up Dev Docs Infrastructure (30-45 minutes)

### Purpose
Enable context persistence across sessions for multi-day/multi-week work in 2M+ LOC codebase.

### Directory Structure
```
dev/
├── active/           # Current work in progress
│   └── [task-name]/
│       ├── [task-name]-plan.md      # Strategy (rarely changes)
│       ├── [task-name]-context.md   # Living state (update frequently)
│       └── [task-name]-tasks.md     # Actionable checklist
├── completed/        # Archived completed work (for reference)
└── templates/        # Templates for creating new dev docs
```

### Create Templates

#### 1. Plan Template
**File:** `dev/templates/plan-template.md`

```markdown
# [Feature/Task Name]

## Executive Summary
[One paragraph: what we're building and why]

## Current State Analysis
- What exists now
- What's broken/missing
- Why current approach isn't sufficient

## Future State Vision
- What we want to achieve
- How it solves the problem
- Success looks like...

## Implementation Phases

### Phase 1: [Name]
**Objective:** [Clear goal]

**Tasks:**
- [ ] Specific, actionable task 1
- [ ] Specific, actionable task 2

**Acceptance Criteria:**
- Measurable success indicator 1
- Measurable success indicator 2

**Dependencies:** [What must be complete first]

### Phase 2: [Name]
[Repeat structure]

## Risk Assessment

### Risk 1: [Description]
- **Likelihood:** High/Medium/Low
- **Impact:** High/Medium/Low
- **Mitigation:** [Concrete approach]

## Success Metrics
- How we measure completion
- Performance benchmarks (if applicable)
- Quality gates

## Timeline Estimates
Phase 1: X days
Phase 2: Y days
Total: Z days
```

#### 2. Context Template
**File:** `dev/templates/context-template.md`

```markdown
# Context: [Feature/Task Name]

## SESSION PROGRESS (⚠️ UPDATE FREQUENTLY)

### Last Updated: [Timestamp]

### Completed Work
- ✅ [What's been completed]

### Currently Working On
🔄 [Current focus]
- File: [file path]
- Current status: [X% complete]
- Next step: [Immediate next action]

### Immediate Next Steps
1. [Next action 1]
2. [Next action 2]
3. [Next action 3]

### Blockers / Issues
- ⚠️ [Any blockers or decisions needed]
- 🔴 [Critical issues requiring immediate attention]

---

## Key Files and Their Purposes

### [Component Name]
- **[File path]**: [Brief description]
  - Why important: [Reason]
  - Key functions/classes: [List]

---

## Important Architectural Decisions

### Decision: [Title] ([Date])
**Chosen:** [What was decided]
**Reasoning:**
- [Reason 1]
- [Reason 2]
**Trade-offs:**
- [Trade-off 1]
**Implementation:** [Where/how implemented]

---

## Technical Constraints Discovered

- **[System/Component]**: [Constraint description]
  - [Implication]

---

## Quick Resume Instructions

**To continue this work:**
1. Read this context file completely
2. Review current progress in SESSION PROGRESS section
3. Check tasks.md for what's marked complete
4. Start with "Immediate Next Steps"
5. Update this file after each major milestone
```

#### 3. Tasks Template
**File:** `dev/templates/tasks-template.md`

```markdown
# Tasks: [Feature/Task Name]

## Status Legend
- ✅ Complete
- 🟡 In Progress
- ⏳ Blocked
- ⬜ Not Started

---

## Phase 1: [Name]

### [Component/Feature 1]
- ⬜ Task 1
- ⬜ Task 2
- ⬜ Task 3

**Acceptance Criteria:**
- [ ] Criterion 1
- [ ] Criterion 2

**Dependencies:** [If any]

---

### [Component/Feature 2]
[Repeat structure]

---

## Phase 2: [Name]
[Repeat structure]

---

## Notes

### Discovered During Implementation
- [Date]: [Discovery or decision made during work]

### Future Enhancements (Post-MVP)
- [Enhancement idea 1]
- [Enhancement idea 2]
```

### Create Helper Slash Commands

#### 1. Create Dev Docs Command
**File:** `.claude/commands/create-dev-docs.md`

```markdown
---
description: Create dev docs structure for a new task from an approved plan
---

Create dev docs for the task we just planned.

1. Ask me for the task name (kebab-case format)
2. Create directory: `dev/active/[task-name]/`
3. Copy templates from `dev/templates/` to new directory
4. Rename template files to match task name:
   - `[task-name]-plan.md`
   - `[task-name]-context.md`
   - `[task-name]-tasks.md`
5. Fill in the plan file based on our planning conversation
6. Fill in initial context file with:
   - Key files we identified
   - Architectural decisions we made
   - Initial next steps
7. Fill in tasks file with the step-by-step implementation plan
8. Confirm files created and ready to begin work
```

#### 2. Update Dev Docs Command
**File:** `.claude/commands/update-dev-docs.md`

```markdown
---
description: Update dev docs before context compaction or end of session
---

Update the dev docs for the current task before we lose context.

1. Identify which task we're working on
2. Update `[task-name]-context.md`:
   - Mark completed work in SESSION PROGRESS
   - Update "Currently Working On" with current status
   - Update "Immediate Next Steps" with what to do next
   - Add any new architectural decisions made
   - Add any new technical constraints discovered
   - Update timestamp
3. Update `[task-name]-tasks.md`:
   - Mark completed tasks with ✅
   - Update in-progress tasks with 🟡
   - Add any new tasks discovered during implementation
   - Add notes about discoveries
4. Summarize what was updated so I can verify
```

#### 3. Resume Work Command
**File:** `.claude/commands/resume-work.md`

```markdown
---
description: Resume work on a task from dev docs
---

Help me resume work on a task.

1. Ask which task I want to resume (or auto-detect from `dev/active/`)
2. Read all three dev docs files:
   - `[task-name]-plan.md` (understand the strategy)
   - `[task-name]-context.md` (understand current state)
   - `[task-name]-tasks.md` (see what's done and what's next)
3. Provide a brief summary:
   - What the task is about (from plan)
   - What's been completed (from context + tasks)
   - Current status (from context)
   - Immediate next steps (from context)
4. Ask if I want to continue with the next steps or if priorities have changed
```

### Setup Instructions

1. **Create directory structure:** `mkdir -p dev/{active,completed,templates}`
2. **Create templates** in `dev/templates/`
3. **Create slash commands** in `.claude/commands/`
4. **Test the workflow:**
   - Use `/create-dev-docs` after planning a sample task
   - Work on task for 30 minutes
   - Use `/update-dev-docs`
   - Start new session
   - Use `/resume-work` and verify you can pick up seamlessly

---

## Phase 5: Create Supporting Documentation (30-60 minutes)

### Purpose
Reference documentation for complex topics that are too detailed for CLAUDE.md or skills.

### Directory Structure
```
docs/
├── architecture/
│   ├── README.md                 # Architecture overview
│   ├── system-components.md      # Major components and responsibilities
│   └── integration-points.md     # External system integrations
├── legacy-patterns/
│   ├── README.md                 # Overview of legacy anti-patterns
│   ├── god-classes.md            # Recognizing and refactoring God classes
│   ├── circular-dependencies.md  # Breaking circular dependencies
│   ├── static-utility-hell.md    # Refactoring static utility classes
│   └── business-logic-in-ui.md   # Extracting logic from JSPs
├── workflows/
│   ├── feature-development.md    # End-to-end feature development workflow
│   ├── bug-fixing.md             # Bug investigation and fixing workflow
│   ├── refactoring-workflow.md   # Safe refactoring workflow
│   └── testing-workflow.md       # Testing untested code workflow
└── domain/
    ├── glossary.md               # Business domain terminology
    └── core-concepts.md          # Key business concepts
```

### Key Documents to Create

#### 1. Architecture Overview
**File:** `docs/architecture/README.md`

**Content:**
- High-level architecture diagram
- Major subsystems and their responsibilities
- Technology stack details (not just names, but how they're used)
- Data flow overview
- Deployment architecture
- Build and release process

**Purpose:** Provide Claude with architectural context without bloating CLAUDE.md

#### 2. System Components
**File:** `docs/architecture/system-components.md`

**Content:**
- Detailed description of each major component/module
- Component responsibilities
- Key classes/packages
- Dependencies (what it depends on, what depends on it)
- Known issues/technical debt per component

**Purpose:** Help Claude understand which components are affected by changes

#### 3. Legacy Anti-Patterns Guide
**File:** `docs/legacy-patterns/README.md`

**Content:**
- Overview of common anti-patterns in the codebase
- How to recognize each pattern
- Why it's problematic
- Refactoring strategy for each
- Examples from actual codebase (anonymized if needed)
- Priority for addressing (some may need to remain for now)

**Purpose:** Educate Claude about project-specific challenges and solutions

#### 4. Feature Development Workflow
**File:** `docs/workflows/feature-development.md`

**Content:**
```markdown
# Feature Development Workflow

## Overview
End-to-end process for developing new features in legacy codebase.

## Prerequisites
- [ ] Feature requirements documented
- [ ] Architectural impact assessed
- [ ] Dev docs created (`/create-dev-docs`)

## Phase 1: Planning (Use Architecture Planning Agent)
1. Invoke planning agent: `/agent architecture-planning`
2. Answer clarification questions thoroughly
3. Review generated plan:
   - Requirements analysis
   - Test plan
   - Architecture design
   - 10-20 implementation steps
4. Approve plan or request revisions
5. Create dev docs: `/create-dev-docs`

## Phase 2: Implementation (Use Feature Implementation Agent)
1. Invoke implementation agent: `/agent feature-implementation`
2. Provide path to dev docs
3. Agent executes plan step-by-step:
   - Tests first (RED)
   - Interfaces/contracts
   - Skeleton implementation
   - Full implementation (GREEN)
   - Refactor if needed
4. Review after each step
5. Update dev docs every 30-60 minutes: `/update-dev-docs`

## Phase 3: Code Review
1. When phase complete, invoke review agent: `/agent code-review`
2. Provide paths to changed files
3. Review agent feedback:
   - Critical issues (MUST fix)
   - High priority issues (SHOULD fix)
   - Medium/Low suggestions
4. Address critical and high priority issues
5. Re-review if needed

## Phase 4: Security Review (For Security-Sensitive Changes)
1. Invoke security agent: `/agent security-vulnerability`
2. Provide scope of changes
3. Review security assessment
4. Address all critical and high vulnerabilities
5. Implement security tests

## Phase 5: Integration and Testing
1. Run full test suite
2. Manual testing of feature
3. Performance testing (if applicable)
4. Update documentation
5. Mark dev docs as complete, move to `dev/completed/`

## Context Management
- **Short session (<2 hours):** May not need dev docs
- **Long session (2+ hours):** Create dev docs, update frequently
- **Multi-session work:** MUST use dev docs, update before ending each session
- **Context running low:** `/update-dev-docs` then `/compact`
- **Resuming work:** `/resume-work [task-name]`

## Emergency Procedures
- **Tests failing:** STOP, don't continue. Fix or revert.
- **Breaking change detected:** STOP, assess impact, plan mitigation
- **Circular dependency created:** STOP, use refactoring agent to break cycle
- **Performance regression:** Investigate immediately, don't defer
```

#### 5. Domain Glossary
**File:** `docs/domain/glossary.md`

**Content:**
- Business terms and their precise meanings
- Acronyms and abbreviations
- Legacy terminology vs modern terminology
- Terms that are confusing/overloaded

**Purpose:** Ensure Claude uses correct business terminology and understands domain concepts

---

## Phase 6: Validation and Testing (30 minutes)

### Test the Setup

#### Test 1: Simple Feature Development
1. Start a small feature (e.g., "Add email validation to user registration")
2. Use Architecture Planning Agent: verify it asks clarifying questions
3. Create dev docs: `/create-dev-docs`
4. Use Feature Implementation Agent: verify it follows TDD approach
5. Verify dev docs were helpful for context

**Success Criteria:**
- Planning agent asked 5+ clarification questions
- Implementation agent wrote tests first
- Dev docs captured key decisions
- Code compiled and tests passed

#### Test 2: Legacy Code Refactoring
1. Identify a God class or method >100 lines
2. Use Legacy Refactoring Agent
3. Verify it creates characterization tests first
4. Verify it refactors incrementally (small steps)
5. Verify all tests pass after refactoring

**Success Criteria:**
- Characterization tests written before refactoring
- Refactoring done in <100 line increments
- Tests passed after each step
- Behavior preserved (tests didn't change)

#### Test 3: Context Persistence
1. Start a multi-hour task
2. Create dev docs
3. Work for 1 hour
4. Update dev docs: `/update-dev-docs`
5. Start new session (simulate context loss)
6. Resume work: `/resume-work [task-name]`
7. Verify Claude understands where you left off

**Success Criteria:**
- Resume took <1 minute
- Claude accurately summarized progress
- Claude knew exact next steps
- No lost context or repeated explanations

#### Test 4: Agent Coordination
1. Plan a feature (Planning Agent)
2. Implement first step (Implementation Agent)
3. Review implementation (Code Review Agent)
4. Fix issues found in review
5. Verify workflow is smooth

**Success Criteria:**
- Each agent stayed in its lane (no overlapping responsibilities)
- Handoffs were clear
- Review caught real issues
- Overall process felt efficient

#### Test 5: Skill Invocation
1. Write code that violates Clean Code principles (e.g., long method)
2. Verify Clean Code skill automatically suggests improvements
3. Work with custom ORM
4. Verify Custom ORM skill provides guidance
5. Write tests for legacy code
6. Verify Characterization Testing skill activates

**Success Criteria:**
- Skills invoked automatically without manual prompting
- Skill guidance was relevant and helpful
- Skills didn't trigger on irrelevant code

---

## Phase 7: Iteration and Refinement (Ongoing)

### Monitor and Improve

#### Week 1: Baseline Measurement
Track:
- How often you use each agent
- How helpful dev docs are for resuming work
- How often skills are auto-invoked
- Time spent on context management vs actual coding

#### Week 2-4: Refinement
Based on Week 1 data:
- **Agents used frequently:** Keep as-is or expand
- **Agents rarely used:** Investigate why, improve or remove
- **Skills not auto-invoked:** Improve description for better discovery
- **Dev docs too heavyweight:** Simplify templates
- **Dev docs insufficient:** Add more structure

#### Common Refinements

**If Planning Agent asks too many questions:**
- Create a "Quick Plan" variant for simple tasks
- Add decision trees to streamline common scenarios

**If Implementation Agent steps too large:**
- Reduce maximum lines per step from 150 to 100
- Add more granular checkpoints

**If Dev Docs feel like overhead:**
- Raise threshold from 2 hours to 3-4 hours
- Create lighter-weight "context notes" for medium tasks

**If Skills don't activate:**
- Review and improve skill descriptions
- Add more keywords that match how you phrase requests
- Consider merging underused skills

**If Context management still challenging:**
- Consider MCP external memory (see alternatives in different-patterns guide)
- Increase update frequency for context.md
- Add automatic context save hooks

### Continuous Improvement Checklist

Monthly review:
- [ ] Are agents being used effectively?
- [ ] Are dev docs saving time on multi-session work?
- [ ] Are skills auto-invoking when relevant?
- [ ] Is CLAUDE.md still current and concise?
- [ ] Have new legacy patterns emerged that need documentation?
- [ ] Have we learned new best practices to codify?
- [ ] Are there repetitive tasks that could be automated?

---

## Success Metrics

### Immediate (Week 1)
- ✅ All infrastructure files created (CLAUDE.md, 6 agents, 6 skills, dev docs templates)
- ✅ All 5 validation tests passing
- ✅ Team members can use the setup
- ✅ First feature developed using the workflow

### Short-term (Month 1)
- ✅ 10+ features developed using agents
- ✅ Dev docs successfully used for 3+ multi-session tasks
- ✅ Skills auto-invoke 80%+ of the time when relevant
- ✅ Code review agent catching real issues
- ✅ Test coverage increasing (measure weekly)

### Long-term (Month 3+)
- ✅ Development velocity increased (measure story points or tasks completed)
- ✅ Fewer bugs in production (measure defect rate)
- ✅ Code quality improving (measure with SonarQube or similar)
- ✅ Developer confidence higher when working in legacy code
- ✅ Context window management not a bottleneck
- ✅ Knowledge transfer faster (new developers productive quicker)

---

## Troubleshooting Common Issues

### Issue: Context window fills up too quickly

**Symptoms:**
- Claude forgets important details mid-session
- Auto-compaction happens frequently
- Responses become less accurate over time

**Solutions:**
1. Use dev docs more aggressively (even for 1-hour tasks)
2. Update context.md every 30 minutes instead of 60
3. Use `/compact` manually before automatic compaction
4. Break work into smaller increments
5. Consider using Task Orchestrator MCP for automatic memory management

### Issue: Agents not following their instructions

**Symptoms:**
- Implementation agent skips tests
- Planning agent jumps to solutions
- Refactoring agent makes changes without tests

**Solutions:**
1. Make agent instructions more explicit (add "MUST", "NEVER")
2. Add validation checkpoints in agent workflows
3. Invoke agents with explicit reminders (e.g., "Use TDD approach")
4. Review and tighten agent constraints
5. Add examples of correct behavior to agent definitions

### Issue: Skills not auto-invoking

**Symptoms:**
- Claude doesn't apply skill knowledge automatically
- Have to manually mention skill name
- Generic responses instead of skill-specific guidance

**Solutions:**
1. Improve skill descriptions with more keywords
2. Add "Use when..." triggers to description
3. Test different phrasings to find what triggers skill
4. Merge related skills (may be too narrowly scoped)
5. Reduce total number of skills (too many can dilute discovery)

### Issue: Dev docs feel like busywork

**Symptoms:**
- Reluctance to create/update dev docs
- Dev docs not actually helpful when resuming
- Spending more time on docs than coding

**Solutions:**
1. Raise threshold for when to use dev docs (3-4 hours instead of 2)
2. Simplify templates (remove unnecessary sections)
3. Focus on SESSION PROGRESS section (most valuable part)
4. Use `/update-dev-docs` command to automate updates
5. Create voice-to-text workflow for faster updates

### Issue: Too many agents, unclear which to use

**Symptoms:**
- Analysis paralysis about which agent to invoke
- Using wrong agent for task
- Agents overlapping in responsibility

**Solutions:**
1. Create decision tree in `docs/workflows/agent-selection.md`
2. Merge similar agents (e.g., Implementation + Refactoring)
3. Add clear "Use this agent when..." to agent descriptions
4. Create workflow documentation with agent sequences
5. Start with just 3 core agents (Planning, Implementation, Review)

---

## Appendix: Quick Reference

### Agent Summary

| Agent | When to Use | Primary Output |
|-------|-------------|----------------|
| **Architecture Planning** | Starting new feature, unclear requirements | Requirements doc, test plan, implementation steps |
| **Feature Implementation** | Executing an approved plan | Working, tested code with commits |
| **Legacy Refactoring** | Improving existing code | Refactored code with characterization tests |
| **Test Coverage** | Adding tests to untested code | Comprehensive test suite with coverage report |
| **Code Review** | Before committing changes | Prioritized feedback and approve/request changes decision |
| **Security Vulnerability** | Security-sensitive changes | Security assessment, remediated code, security tests |

### Skill Summary

| Skill | Auto-Invokes When | Provides |
|-------|-------------------|----------|
| **Legacy Java EE Patterns** | Working with God classes, circular dependencies, legacy EJBs | Refactoring strategies, migration paths |
| **Clean Code Java** | Writing or refactoring any Java code | Code quality guidance, examples |
| **Working with Custom ORM** | Database access, ORM operations | ORM-specific patterns, migration guidance |
| **Characterization Testing** | Testing untested legacy code | Dependency breaking techniques, test strategies |
| **Database Performance** | Slow queries, database operations | Performance issue identification, optimization |
| **Safe Refactoring** | Improving code structure | Refactoring catalog, step-by-step mechanics |

### Slash Command Summary

| Command | Purpose | When to Use |
|---------|---------|-------------|
| `/create-dev-docs` | Create plan, context, tasks files | After planning, before implementation |
| `/update-dev-docs` | Update context and tasks | Every 30-60 min, before compaction, end of session |
| `/resume-work` | Load context and continue task | Starting new session on existing task |

### Dev Docs Structure

- **plan.md** - Strategy (rarely changes) - Read at task start
- **context.md** - Living state (update frequently) - Read every session, update every milestone
- **tasks.md** - Actionable checklist - Update immediately as tasks complete

---

## Getting Started Checklist

Use this checklist when you begin setup in your actual project:

### Setup Phase
- [ ] Create `CLAUDE.md` in project root
- [ ] Create `.claude/agents/` directory structure
- [ ] Create all 6 agent definition files
- [ ] Create `skills/` directory structure
- [ ] Create all 6 skill definition files
- [ ] Create `dev/` directory structure (active, completed, templates)
- [ ] Create dev docs templates (plan, context, tasks)
- [ ] Create slash commands (create-dev-docs, update-dev-docs, resume-work)
- [ ] Create `docs/` directory structure
- [ ] Create key documentation files (architecture, legacy patterns, workflows)

### Validation Phase
- [ ] Test 1: Simple feature development
- [ ] Test 2: Legacy code refactoring
- [ ] Test 3: Context persistence
- [ ] Test 4: Agent coordination
- [ ] Test 5: Skill invocation

### Customization Phase
- [ ] Customize CLAUDE.md with actual project details
- [ ] Customize agents with project-specific sections
- [ ] Fill in Custom ORM skill with actual ORM details
- [ ] Create architecture documentation from actual system
- [ ] Create domain glossary with actual business terms
- [ ] Document actual legacy anti-patterns from codebase

### Launch Phase
- [ ] Train team on setup and workflows
- [ ] Conduct first real feature using full workflow
- [ ] Gather feedback and iterate
- [ ] Document lessons learned
- [ ] Refine based on real-world usage

---

## Final Notes

### This Setup is Optimized For:
- ✅ Very large codebases (2M+ LOC)
- ✅ Brittle legacy systems
- ✅ Multi-session work requiring context persistence
- ✅ Safety-first approach (prevent breaking changes)
- ✅ Test-driven development
- ✅ Incremental improvement over big rewrites

### This Setup May Be Overkill If:
- ❌ Small codebase (<100k LOC)
- ❌ Modern, well-tested codebase
- ❌ All work fits in single sessions
- ❌ Team already has established workflows that work

### Evolution Path:
1. **Week 1:** Start with basic setup, all components
2. **Week 2-4:** Identify what's working, what's not
3. **Month 2:** Streamline (remove unused, enhance used)
4. **Month 3+:** Fully customized to your team's workflow

### Remember:
- **Start simple, add complexity as needed**
- **Update frequently (especially context.md)**
- **Iterate based on real usage, not assumptions**
- **The goal is productivity, not perfect process**

### Questions or Issues?
- Review troubleshooting section
- Check community resources (r/ClaudeAI, GitHub discussions)
- File issues or ask for help

---

**You're now ready to set up Claude Code for your legacy Java EE project!**

**Next Step:** Feed this plan to Claude in your project and begin Phase 1: Creating CLAUDE.md

Good luck! 🚀
