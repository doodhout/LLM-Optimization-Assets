# Spec-Driven Development Executive Summary

## Core Concept

SDD shifts from "code as source of truth" to "intent as source of truth." With AI, specifications become executable blueprints that directly generate working implementations.

**Paradigm:** Specification → AI interprets → AI generates → Code

## The Problem

**Vibe Coding:** Ad-hoc prompting without clear specifications produces:
- Technical debt tsunami (Forrester forecast)
- High code churn and substantial post-acceptance modifications
- Duplicated code blocks
- Fragile, oversized codebases with local minima choices
- AI optimizes for plausible code, not correct code

## Implementation Levels

**Level 1 - Spec-First:** Write spec first, use in development, may discard after.

**Level 2 - Spec-Anchored:** Spec kept after completion, used for evolution, co-evolves with code.

**Level 3 - Spec-as-Source:** Spec is main source file, only spec edited by humans, code always generated and marked "DO NOT EDIT."

## Tools

**GitHub Spec Kit** (github.com/github/spec-kit) - Experimental, open-source, multi-agent support. Directory structure: .specify/ with constitution/, spec.md, plan.md, tasks/.

**Kiro** (kiro.dev) - AWS-backed, production-ready, uses EARS notation for requirements, includes automated reasoning.

**Tessl** - Early stage, pursuing spec-as-source model where code becomes implementation detail.

## Critical Best Practices

**DO:**
1. Write specifications before code - iteratively develop thorough spec before implementation
2. Keep specs tightly scoped - one cohesive feature per spec, prevents overlap
3. Include tests in specs from day 1 - tests clarify intent, catch AI mistakes early
4. Version specs with git - treat as code, track evolution, enable rollback
5. Use constitution pattern - stable principles (architecture, standards) separate from feature specs
6. Start with one feature - learn curve, iterate approach, scale after validation
7. Be extremely clear about "done" - explicit completion criteria eliminate ambiguity
8. Iterative refinement - specs improve through iteration like code
9. Maintain lessons learned - capture what works, avoid repeating mistakes
10. Invest time upfront - 20-30% spec writing, 10-15% planning saves debugging time

**DON'T:**
1. Write specs that are code - describe "what" not "how," avoid pseudocode
2. Create markdown overload - concise over verbose, developers spend time reading/hunting errors
3. Expect "generate once" magic - SDD is iterative, regeneration produces variations
4. Skip human review - review both spec AND generated code, catch intent/implementation errors
5. Use SDD for everything - overkill for trivial changes, exploratory prototypes, one-off scripts
6. Mix abstraction levels - maintain consistent level, separate overview from details

## When to Use

**Use SDD:**
- Adding features to existing systems (most powerful)
- Clear, well-understood requirements
- Team collaboration important
- Long-term maintenance expected
- Code quality and predictability matter

**Skip SDD:**
- Trivial changes (typos, dependency updates)
- Throw-away prototypes
- Highly exploratory requirements
- Solo work on small scripts

## Key Evidence

- Early adopters report 10x productivity for complex features
- Forrester forecasts technical debt tsunami from unguided AI usage
- Studies show significant duplicated code from vibe coding
- Harper Reed workflow emphasizes thorough specification before implementation

## Workflow Summary

1. **Specify:** Define what to build (requirements, acceptance criteria, edge cases)
2. **Plan:** Technical approach (architecture, tech stack, components, testing)
3. **Tasks:** Break into atomic, independently implementable units
4. **Implement:** AI generates from spec, human reviews against criteria
5. **Iterate:** Bugs found → update spec → regenerate → verify

## Project-Specific Recommendations

**Greenfield:** Spec Kit or Kiro, start with constitution, spec-first or spec-anchored

**Brownfield:** Spec-first for new features only, reference existing code, gradual coverage

**Enterprise:** Kiro with formal requirements (EARS), spec-anchored for maintenance

**Prototypes:** Light spec-first, minimal specs, skip constitution, move fast

## Current State (2025)

Maturity: Early adopter phase, experimental to production-ready tools. Most adoption at spec-first level, some spec-anchored, rare spec-as-source. Rapidly evolving toward standardization and better AI understanding.

## Bottom Line

SDD significantly improves over vibe coding for substantial work. Not a silver bullet, but evidence shows reduced technical debt, better collaboration, and predictable AI-assisted development. Recommendation: try on one feature, expand if successful.
