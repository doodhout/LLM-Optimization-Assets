# Model Choice Best Practices - Executive Summary

## Model Landscape (January 2025)

**Claude Sonnet 4.5**: Flagship model, best for production code, complex reasoning, and general development. 200K context, moderate speed.

**Claude Opus 4**: Most capable for extremely complex problems, research, and critical security tasks. Slower, highest cost.

**Claude Haiku**: Fast and economical for simple tasks, prototyping, and high-volume operations. Very fast responses, lowest cost.

## Core Principles

### 1. Match Model to Task
- Simple/well-defined tasks → Haiku
- Most production work → Sonnet 4.5
- Complex reasoning/critical decisions → Opus 4

### 2. Model Cascading Strategy
Start with cheaper models and escalate only if needed. Most tasks don't require the most powerful model.

### 3. Optimize Total Cost
Choose models based on combined time and monetary cost, not just model price. More expensive models that save hours are actually cheaper.

### 4. Speed Matters
- Haiku: 2-5 seconds
- Sonnet 4.5: 5-15 seconds
- Opus 4: 10-30+ seconds

Use faster models for interactive debugging and rapid iteration.

## Critical Best Practices

**DO:**
- Start with the appropriate model for task complexity
- Use Haiku for iteration and experimentation
- Match model to task criticality (critical tasks → Opus/Sonnet)
- Consider context window needs (all Claude models have 200K)
- Factor response speed into workflow decisions
- Calculate ROI: (Time Saved × Hourly Rate) / Model Cost

**DON'T:**
- Always use the most powerful model (wastes money and time)
- Underestimate Haiku (handles 70-80% of routine work)
- Ignore latency in time-sensitive workflows
- Skip human review regardless of model choice

## Model-Specific Strengths

**Sonnet 4.5 - The Workhorse (90% of daily work)**
- Production code, refactoring, debugging, code review, integration work

**Opus 4 - The Specialist (critical tasks only)**
- Novel problems, complex algorithms, architecture decisions, security audits

**Haiku - The Sprinter (rapid work)**
- Prototyping, tests, boilerplate, simple refactoring, documentation

## Multi-Model Workflows

**Draft-Revise-Polish Pattern:**
Haiku (draft) → Sonnet 4.5 (revise) → Opus 4 if needed (polish)
Saves 60-70% cost vs using Opus throughout.

**Parallel Exploration:**
Try multiple approaches with Haiku, select best, refine with Sonnet 4.5.

**Specialist Team:**
- Haiku: Tests and boilerplate
- Sonnet 4.5: Implementation
- Opus 4: Architecture review

## Quick Decision Guide

**Use Haiku:** Straightforward tasks, speed/cost matters, prototyping, high-volume operations

**Use Sonnet 4.5:** Default choice for production work, unsure which model to use, daily development

**Use Opus 4:** Truly complex reasoning, critical/security-sensitive decisions, stuck with Sonnet

## Cost Efficiency

**Relative costs:** Haiku (1x), Sonnet 4.5 (3-5x), Opus 4 (10-15x)

**Key insight:** Users report 70-80% of tasks can be handled by Haiku/Sonnet 4.5, reserving Opus for the 20-30% of truly critical work.

## Golden Rule

Optimize for total cost (your time + model cost), not just model cost. Choose the model that delivers the best outcome in the least total time and money.
