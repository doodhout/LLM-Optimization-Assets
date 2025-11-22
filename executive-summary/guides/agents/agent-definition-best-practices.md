# Agent Definition Best Practices - Executive Summary

## What Are Agent Definitions?
Specialized AI personalities for specific tasks, invoked on-demand rather than loaded by default. Different from CLAUDE.md (project-wide context).

## Core Principles

### 1. Single Responsibility
One clear purpose per agent. Focused agents produce 30-40% better results on specialized tasks.

### 2. Clear Identity and Role
Define role, primary responsibilities, and explicitly state what the agent does NOT do.

### 3. Input/Output Specification
Specify exact inputs expected and output structure produced. Reduces variability by 70%.

## Critical Inclusions

### Specialized Knowledge
Domain expertise, frameworks, best practices, and common pitfalls relevant to the task.

### Decision-Making Criteria
Prioritization rules, trade-off considerations, and quality thresholds.

### Examples and Counter-Examples
2-3 diverse examples with annotations showing good vs. bad patterns. More effective than 10+ rules without examples.

### Context for Invocation
When to use, when NOT to use, prerequisites, and workflow placement.

### Quality Standards
Minimum requirements, optional enhancements, and self-check questions.

### Constraints and Limitations
Scope boundaries, time/token budgets, escalation criteria, and known limitations.

## Critical Exclusions

- Generic AI assistant instructions (wastes 10-20% of tokens with zero impact)
- Overlapping instructions with CLAUDE.md (creates maintenance burden)
- Step-by-step implementation algorithms (reduces flexibility)
- Too many similar examples (diminishing returns after 2-3)
- Personal preferences unrelated to quality
- Negative or discouraging language

## Optimal Structure

**Target length:** 300-1000 tokens (200-750 words)
- Under 200 tokens: Too vague, inconsistent
- 300-1000 tokens: Sweet spot
- Over 1500 tokens: Diminishing returns
- Over 3000 tokens: Active harm

**Recommended sections:**
1. Role (identity and purpose)
2. Responsibilities
3. Expertise
4. Input/Output
5. Process (high-level, not step-by-step)
6. Quality Standards
7. Examples (2-3 diverse)
8. Constraints
9. Decision Criteria

## Advanced Techniques

### Chain-of-Thought Prompting
Ask agent to think through process before responding. Improves quality 20-30% on complex tasks.

### Role-Playing Enhancement
Rich personas activate more relevant knowledge and create consistent behavior.

### Self-Correction Instructions
Build in quality checks. Reduces errors by 30%.

### Automated Skill Activation with Hooks
Use UserPromptSubmit hooks to auto-detect relevant skills and inject reminders into context. Solves the problem of unused skills.

### Progressive Disclosure
Keep main skill files under 500 lines, use resource files for details. Improves token efficiency 40-60%.

## Validation Checklist

- Single, clear purpose
- Role and responsibilities explicit
- Input/output formats specified
- 2-3 diverse examples provided
- Quality standards defined
- Constraints clear
- No generic AI instructions
- No CLAUDE.md redundancy
- 300-1000 token length
- Professional tone
- Decision criteria included

## Common Pitfalls

1. **"Do Everything" Agent** - Split into multiple specialized agents
2. **Vague Agent** - Be specific about expertise, process, output
3. **Overly Constrained Agent** - Provide principles, not algorithms
4. **Redundant Agent** - Focus only on unique role aspects
5. **Example-Less Agent** - Always include 2-3 concrete examples

## Workflow Integration

Use specialized agents at checkpoints, general assistant for everything else. Create slash commands to trigger agent workflows with proper context.

## Effectiveness Metrics

**Quantitative:**
- Consistency across similar inputs
- Coverage of expected items
- Precision of correct/useful outputs
- Token efficiency

**Qualitative:**
- Stays in role
- Actionable output
- Matches examples
- Usable as-is

**Good signs:** Consistent format, rarely needs clarification, minimal editing, regular use.

**Bad signs:** Variable outputs, goes off-script, heavy editing needed, avoided.

## Key Insights

- One focused agent beats one generalist
- 2-3 diverse examples outperform 10+ similar examples
- Telling Claude what NOT to do is as important as what to do
- If manually reminding Claude about guidelines, build a hook to automate it
- Best agent definition creates output trustworthy enough to use with minimal review

## Automation Principle

Workflow should enforce quality automatically through hooks, not rely on remembering to ask. Build self-correction into the system.
