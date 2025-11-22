# CLAUDE.md Best Practices - Executive Summary

## Core Purpose
CLAUDE.md contains stable, project-wide conventions loaded automatically into every session. It defines project personality, technical standards, and working style.

## Critical Inclusions

**Code Style & Formatting**
- Language-specific rules, naming conventions, file organization
- Reduces repetitive corrections across sessions

**Project Architecture & Patterns**
- Architectural patterns, state management, error handling, testing strategies
- Prevents architectural drift and reduces clarifying questions by ~50%

**Technology Stack & Constraints**
- Approved libraries, version requirements, forbidden dependencies
- Prevents incompatible solutions and wasted refactoring

**Domain-Specific Knowledge**
- Business terminology, abbreviations, project concepts
- Reduces misunderstandings by ~60%

**Communication Preferences**
- Verbosity level, explanation depth, commit message style
- Makes interactions more efficient

**Testing & Quality Standards**
- Coverage expectations, testing approach, quality gates

## Critical Exclusions

**Session-Specific Instructions**
- Current tasks or bugs cause confusion and waste context

**Frequently Changing Information**
- Sprint goals, temporary workarounds become stale quickly

**Excessive Low-Level Details**
- Full API docs, schemas, extensive examples waste 10-30% of context
- Link to documentation instead

**External Documentation Copies**
- React/TypeScript docs already in Claude's knowledge
- Wastes tokens with no competitive advantage

**Overly Prescriptive Rules**
- Excessive restrictions reduce judgment and create conflicts

**Irrelevant Personal Preferences**
- Wastes context tokens without affecting code

## Optimal Structure & Size

**Target Size:** 200-800 tokens (150-600 words)
- Balances essential context with available workspace
- Files over 1,500 tokens show diminishing returns

**Token Efficiency**
- Use references to files, not copies of content
- Minimize examples to single-sentence illustrations
- Prioritize high-impact rules that prevent common mistakes

## Modern Separation Principle

**CLAUDE.md** - Project-specific navigation and quick start commands

**Skills** - Reusable coding patterns and framework best practices

**/docs/** - Detailed architecture, component documentation, integration guides

## Tiered Documentation for Large Projects

**Level 1:** CLAUDE.md as navigation hub (200 lines)

**Level 2:** Domain documentation linked from hub

**Level 3:** Specific component documentation loaded on demand

**Benefit:** Claude loads only what's needed, reducing context usage

## Maintenance Requirements

- Review quarterly or when major patterns change
- Remove outdated rules immediately
- Keep synchronized with actual codebase
- Start minimal, add reactively, remove proactively

## Evidence-Based Outcomes

- 25-30% fewer style corrections
- 50% reduction in clarifying questions
- 40% improvement in code consistency
- 60% reduction in domain terminology misunderstandings

## Migration Strategy for Oversized Files

1. Keep project commands in CLAUDE.md
2. Extract general patterns to skills
3. Move architecture details to /docs/
4. Transform CLAUDE.md into lean navigation hub (200 lines)

## Golden Rule

If guidance repeats across sessions, it belongs in CLAUDE.md or skills. If it's task-specific or changes frequently, use prompts. If it's detailed documentation, place in /docs/ and reference from CLAUDE.md.
