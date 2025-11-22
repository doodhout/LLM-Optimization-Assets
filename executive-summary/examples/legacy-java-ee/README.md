# Legacy Java EE Agents - Executive Summary

## Core Concept

Nine specialized agents for legacy Java EE applications, all following a mandatory four-phase workflow based on industry-leading software engineering books.

## Mandatory Four-Phase Workflow

All agents must follow this sequence:

1. **Requirements Understanding** (20-30% time) - Fully comprehend requirements, read existing code, identify dependencies
2. **Test Infrastructure** (20-30% time) - Write failing tests BEFORE implementation using TDD
3. **Skeleton Design** (15-20% time) - Create interfaces, method signatures, class structures with no implementation
4. **Implementation** (30-40% time) - Write code to pass tests, work incrementally, refactor for quality

## Nine Specialized Agents

### 1. Legacy Code Refactoring Agent
Safely refactor legacy Java EE code using characterization tests, proven patterns, and Clean Code principles.

### 2. Test Coverage Agent
Add comprehensive test coverage to untested legacy code using characterization tests and dependency-breaking techniques.

### 3. API Modernization Agent
Convert SOAP/RMI to RESTful APIs with backward compatibility, proper versioning, and OpenAPI documentation.

### 4. Security Vulnerability Agent
Identify and fix OWASP Top 10 vulnerabilities including SQL injection, XSS, CSRF, and authentication issues.

### 5. Performance Optimization Agent
Profile and optimize bottlenecks, fix N+1 queries, implement caching, and reduce memory consumption.

### 6. Database Refactoring Agent
Safely evolve schemas using expand-contract pattern, write migration scripts, migrate JDBC to JPA with zero downtime.

### 7. Code Review Agent
Review code for Clean Code principles, Effective Java violations, security issues, and provide prioritized feedback.

### 8. Architecture Planning Agent
Plan features through requirements analysis, Socratic questioning, comprehensive test planning, and step-by-step implementation roadmaps.

### 9. Feature Implementation Agent
Execute plans incrementally in 30-60 minute TDD-driven steps, maintaining working state after each change.

## Foundation Principles

Based on four authoritative books:

- **Clean Code** (Robert C. Martin) - Small functions, meaningful names, Single Responsibility, DRY
- **Refactoring** (Martin Fowler) - Code smells, proven patterns, small safe steps, behavior preservation
- **Effective Java** (Joshua Bloch) - 90 best practices for Java development
- **Working Effectively with Legacy Code** (Michael Feathers) - Seams, characterization tests, dependency breaking

## Critical Rules

**Always:**
- Follow four-phase workflow in order
- Write tests before implementation
- Work in small increments
- Run tests after every change
- Understand requirements before coding

**Never:**
- Skip requirements phase
- Write code before tests
- Make large untested changes
- Implement before designing skeleton
- Refactor without characterization tests

## Usage Options

1. **Reference Material** - Study as best practices checklists
2. **Agent Definitions** - Use directly with AI coding assistants
3. **Team Standards** - Adopt as coding standards and review processes

## Key Success Factor

Success requires strict adherence to the four-phase workflow: Understand → Test → Design → Implement.
