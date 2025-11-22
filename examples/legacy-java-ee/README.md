# Legacy Java EE Agent Examples

This directory contains specialized agent definitions for common use cases when working with legacy monolithic Java EE backend projects. Each agent is carefully designed to follow evidence-based best practices and incorporate principles from:

- **Clean Code** by Robert C. Martin
- **Refactoring** by Martin Fowler
- **Effective Java** by Joshua Bloch
- **Working Effectively with Legacy Code** by Michael Feathers

## 📋 Overview

All agents in this collection follow a **mandatory four-phase workflow**:

1. **Requirements Understanding (FIRST)** - Fully comprehend what needs to be done
2. **Test Infrastructure (SECOND)** - Write tests in TDD approach
3. **Skeleton Design (THIRD)** - Create APIs, interfaces, classes, method signatures
4. **Implementation (LAST)** - Write actual code inside methods

This approach ensures high quality, well-tested, and maintainable code that adheres to industry best practices.

## 🎯 Available Agents

### 1. Legacy Code Refactoring Agent
**Location:** `refactoring/legacy-code-refactoring-agent.md`

**Purpose:** Safely refactor legacy Java EE code while maintaining behavior and minimizing risk.

**Key Capabilities:**
- Identify code smells and refactoring opportunities
- Create characterization tests before refactoring (safety net)
- Apply proven refactoring patterns (Extract Method, Introduce Parameter Object, etc.)
- Break dependencies to enable testability
- Follow Clean Code principles throughout

**Best For:**
- Simplifying complex methods (> 50 lines)
- Reducing cyclomatic complexity
- Extracting reusable components
- Making legacy code testable
- Improving code maintainability

**Example Use Cases:**
- Refactor 180-line method with multiple responsibilities → 7-line orchestrator with extracted methods
- Break hard-coded dependencies to enable unit testing
- Introduce Parameter Object for methods with 12+ parameters

---

### 2. Test Coverage Agent
**Location:** `testing/test-coverage-agent.md`

**Purpose:** Add comprehensive test coverage to legacy code with little or no existing tests.

**Key Capabilities:**
- Create characterization tests that document current behavior
- Break dependencies using seams and dependency breaking techniques
- Write unit, integration, and component tests
- Apply TDD principles even to existing code
- Test Java EE components (EJBs, REST endpoints, JPA entities)

**Best For:**
- Adding tests to untested legacy code
- Achieving 80%+ code coverage
- Testing components with complex dependencies
- Creating regression test suites
- Enabling safe refactoring through comprehensive tests

**Example Use Cases:**
- Add tests to UserService EJB with database dependencies
- Test JAX-RS REST endpoints with multiple service dependencies
- Create characterization tests for 200-line legacy method

---

### 3. API Modernization Agent
**Location:** `api-modernization/api-modernization-agent.md`

**Purpose:** Modernize legacy APIs (SOAP, RMI) into RESTful APIs while maintaining backward compatibility.

**Key Capabilities:**
- Analyze legacy API implementations
- Design RESTful APIs following REST maturity model
- Implement proper versioning strategies
- Create adapter layers for backward compatibility
- Apply security best practices (OAuth2, JWT)
- Generate OpenAPI/Swagger documentation

**Best For:**
- Converting SOAP services to REST
- Modernizing proprietary protocols
- Adding pagination, filtering, and sorting to legacy endpoints
- Implementing proper error handling and status codes
- Creating well-documented, modern APIs

**Example Use Cases:**
- Convert SOAP OrderService (8 operations) to RESTful API
- Add pagination to endpoint returning 10,000+ records
- Modernize authentication from WS-Security to OAuth2

---

### 4. Security Vulnerability Agent
**Location:** `security/security-vulnerability-agent.md`

**Purpose:** Identify and remediate security vulnerabilities in legacy Java EE applications.

**Key Capabilities:**
- Audit code for OWASP Top 10 vulnerabilities
- Fix SQL injection, XSS, CSRF, and authentication issues
- Implement proper input validation and output encoding
- Apply secure coding practices
- Create security tests to prevent regression

**Best For:**
- Security audits and vulnerability remediation
- Fixing SQL injection and XSS vulnerabilities
- Implementing proper authentication and authorization
- Securing sensitive data (encryption, hashing)
- Compliance requirements (PCI-DSS, HIPAA, GDPR)

**Example Use Cases:**
- Fix SQL injection in UserDAO.findUserByUsername()
- Remediate XSS vulnerability in user profile page
- Implement proper password hashing (bcrypt)
- Add input validation to prevent injection attacks

---

### 5. Performance Optimization Agent
**Location:** `performance/performance-optimization-agent.md`

**Purpose:** Identify and resolve performance bottlenecks in legacy Java EE applications.

**Key Capabilities:**
- Profile applications to identify actual bottlenecks
- Optimize slow database queries (N+1 problems)
- Implement effective caching strategies
- Reduce memory consumption and eliminate leaks
- Optimize algorithms and data structures
- Measure and validate improvements

**Best For:**
- Fixing slow page loads and high response times
- Eliminating N+1 query problems
- Implementing caching for expensive operations
- Optimizing database access patterns
- Meeting performance SLAs

**Example Use Cases:**
- Fix N+1 query problem: 101 queries → 1 query (56x faster)
- Implement caching: 2,550ms → 0.5ms for cache hits (5,000x faster)
- Optimize slow order search: 14 seconds → 250ms (56x speedup)

---

### 6. Database Refactoring Agent
**Location:** `database/database-refactoring-agent.md`

**Purpose:** Safely evolve database schemas and migrate from JDBC to JPA with zero downtime.

**Key Capabilities:**
- Refactor database schemas safely (expand-contract pattern)
- Write versioned migration scripts (Flyway/Liquibase)
- Migrate legacy JDBC code to JPA/Hibernate
- Implement zero-downtime database migrations
- Ensure data integrity and referential consistency

**Best For:**
- Normalizing tables and fixing schema issues
- Migrating from direct JDBC to JPA
- Adding/removing columns with zero downtime
- Splitting or merging tables
- Data migrations with large data volumes

**Example Use Cases:**
- Normalize users table: extract address fields to separate table (5M records, zero downtime)
- Migrate JDBC UserDAO to JPA entities
- Add foreign key constraints to legacy schema

---

### 7. Code Review Agent
**Location:** `code-review/code-review-agent.md`

**Purpose:** Provide thorough, constructive code reviews focused on quality and best practices.

**Key Capabilities:**
- Review code for Clean Code principles
- Identify Effective Java best practice violations
- Spot security vulnerabilities (OWASP Top 10)
- Detect performance issues and anti-patterns
- Assess test coverage and quality
- Provide actionable, prioritized feedback

**Best For:**
- Pull request reviews
- Pre-merge code quality checks
- Mentoring developers on best practices
- Ensuring code meets team standards
- Preventing technical debt accumulation

**Example Use Cases:**
- Review REST endpoint implementation for security issues
- Identify violations of Clean Code principles
- Check for proper error handling and input validation
- Verify test coverage is adequate

---

### 8. Architecture Planning Agent
**Location:** `planning/architecture-planning-agent.md`

**Purpose:** Plan new features or changes through deep requirements analysis and TDD-first design.

**Key Capabilities:**
- Ask clarifying questions to fully understand requirements
- Validate user understanding through Socratic questioning
- Identify use cases, edge cases, and error scenarios
- Design comprehensive test plans BEFORE implementation
- Create detailed, step-by-step implementation plans
- Break down complex features into small increments
- Assess risks and define mitigation strategies

**Best For:**
- Planning new features from requirements
- Changing existing features safely
- Ensuring all edge cases are considered
- Creating TDD-first implementation roadmaps
- Breaking complex work into manageable steps
- Validating requirements completeness

**Example Use Cases:**
- Plan "User Favorites" feature with 20 implementation steps
- Design test-first approach for payment processing
- Break down complex reporting feature into increments
- Identify edge cases for authentication system changes

**Workflow:**
1. Ask clarifying questions (validate understanding)
2. Design test cases (TDD-first approach)
3. Create architecture and implementation plan
4. Review and get user approval

---

### 9. Feature Implementation Agent
**Location:** `implementation/feature-implementation-agent.md`

**Purpose:** Execute implementation plans in small, TDD-driven steps with continuous validation.

**Key Capabilities:**
- Execute plans step-by-step following strict TDD workflow
- Work in very small increments (30-60 minute steps)
- Follow Tests → Interfaces → Skeleton → Implementation sequence
- Provide clear status updates after each step
- Ensure tests pass after every change
- Break down large steps automatically
- Keep system in working state continuously

**Best For:**
- Implementing features from Architecture Planning Agent
- Executing complex changes incrementally
- Maintaining user oversight during implementation
- Ensuring TDD discipline throughout development
- Delivering working software in small increments

**Example Use Cases:**
- Implement UserFavorite entity with TDD (5 commits per step)
- Build REST API endpoint following plan step-by-step
- Add feature incrementally with continuous testing
- Refactor large component in small, safe steps

**Workflow (per step):**
1. Write failing tests (RED)
2. Define interfaces/signatures
3. Create skeleton (empty methods)
4. Implement to make tests pass (GREEN)
5. Refactor and commit

**Note:** Works in tandem with Architecture Planning Agent - use Planning Agent to create the plan, then Implementation Agent to execute it.

---

## 🚀 How to Use These Agents

### Option 1: As Reference Material

Study these agents to understand best practices and workflows for each use case. Use them as checklists when performing similar tasks manually.

### Option 2: As Agent Definitions (Recommended)

These agents can be used directly with AI coding assistants that support custom agents/skills:

1. **Copy the agent definition** from the appropriate MD file
2. **Create a custom agent/skill** in your AI assistant (Claude Code, Cursor, GitHub Copilot, etc.)
3. **Invoke the agent** when working on the relevant task
4. **Follow the four-phase workflow** the agent prescribes

### Option 3: As Team Standards

Use these as the foundation for your team's coding standards and review processes:

1. **Adopt the workflows** described in each agent
2. **Customize** for your specific tech stack and requirements
3. **Train team members** on the principles and patterns
4. **Reference in code reviews** and documentation

## 📚 Core Principles (All Agents)

### The Four-Phase Workflow

Every agent follows this mandatory workflow:

#### Phase 1: Requirements Understanding (FIRST - 20-30% of time)
- **Never skip this step!** Fully understand what needs to be done
- Read and comprehend existing code
- Identify dependencies and constraints
- Document current behavior
- Understand the business context
- **Output:** Clear understanding and written analysis

#### Phase 2: Test Infrastructure (SECOND - 20-30% of time)
- **Test-Driven Development approach**
- Write tests BEFORE implementing changes
- Create characterization tests for existing behavior
- Design test doubles and fixtures
- Tests should FAIL initially (Red phase of TDD)
- **Output:** Comprehensive failing test suite

#### Phase 3: Skeleton Design (THIRD - 15-20% of time)
- Design APIs, interfaces, and class structures
- Create method signatures with JavaDoc
- Define DTOs and value objects
- Plan the implementation approach
- **NO implementation logic yet** - only structure
- **Output:** Complete skeleton with contracts

#### Phase 4: Implementation (LAST - 30-40% of time)
- Implement code to make tests pass (Green phase)
- Work incrementally (small steps)
- Run tests after every change
- Refactor for quality (Refactor phase)
- Keep the system working after each step
- **Output:** Fully implemented, tested, working code

### Book-Based Principles

All agents incorporate these principles:

**Clean Code (Robert C. Martin):**
- Meaningful names that reveal intent
- Small functions (< 50 lines, ideally < 20)
- Single Responsibility Principle
- Don't Repeat Yourself (DRY)
- Clear error handling with exceptions
- Comprehensive unit tests

**Refactoring (Martin Fowler):**
- Identify code smells before refactoring
- Apply proven refactoring patterns
- Refactor in small, safe steps
- Keep tests green throughout
- Behavior preservation is paramount

**Effective Java (Joshua Bloch):**
- Minimize mutability (Item 17)
- Use builders for complex objects (Item 2)
- Override equals/hashCode correctly (Item 11)
- Return empty collections, not null (Item 54)
- Prefer interfaces to abstract classes (Item 20)
- Check parameters for validity (Item 49)

**Working Effectively with Legacy Code (Michael Feathers):**
- Identify seams for testing
- Use characterization tests
- Break dependencies safely
- Sprout Method/Class for new functionality
- Extract and Override technique
- Preserve behavior while refactoring

## ⚠️ Important Notes

### DO:
- ✅ Always follow the four-phase workflow
- ✅ Write tests BEFORE implementation
- ✅ Work incrementally (small steps)
- ✅ Run tests after every change
- ✅ Fully understand requirements before coding
- ✅ Create skeleton (signatures) before implementation
- ✅ Apply principles from the four books

### DON'T:
- ❌ Skip requirements understanding phase
- ❌ Write code before writing tests
- ❌ Make large changes without testing
- ❌ Assume you understand without reading code
- ❌ Implement logic before designing skeleton
- ❌ Refactor without characterization tests
- ❌ Ignore the book principles

## 🎓 Learning Resources

To get the most out of these agents, familiarize yourself with:

**Books (Essential):**
1. **Clean Code** by Robert C. Martin - Foundational principles for code quality
2. **Refactoring** by Martin Fowler - Catalog of refactoring patterns
3. **Effective Java** by Joshua Bloch - Java best practices (90 items)
4. **Working Effectively with Legacy Code** by Michael Feathers - Techniques for legacy code

**Online Resources:**
- OWASP Top 10: https://owasp.org/www-project-top-ten/
- OWASP Cheat Sheet Series: https://cheatsheetseries.owasp.org/
- Martin Fowler's Refactoring Catalog: https://refactoring.com/catalog/
- Effective Java 3rd Edition summary
- Java EE 7 Tutorial: https://docs.oracle.com/javaee/7/tutorial/

## 🤝 Contributing

These agents are based on evidence-based best practices from the LLM-Optimization-Assets repository. If you have improvements or additional use cases:

1. Follow the same structure and principles
2. Include concrete, detailed examples
3. Reference authoritative sources (books, OWASP, etc.)
4. Emphasize the four-phase workflow
5. Include both DO and DON'T examples

## 📄 License

See the main repository LICENSE file.

## 🙏 Acknowledgments

These agents are built on principles from:
- Robert C. Martin (Clean Code)
- Martin Fowler (Refactoring)
- Joshua Bloch (Effective Java)
- Michael Feathers (Working Effectively with Legacy Code)
- OWASP (Security best practices)
- The Java/Java EE community

---

**Remember:** The key to success with these agents is following the four-phase workflow religiously:
1. **Understand** requirements FIRST
2. **Test** with TDD approach SECOND
3. **Design** skeleton THIRD
4. **Implement** code LAST

This approach ensures high-quality, well-tested, maintainable code that stands the test of time.
