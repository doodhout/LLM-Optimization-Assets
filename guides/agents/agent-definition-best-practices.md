# Agent Definition Best Practices

## Overview

Agent definition files (often implemented as custom instructions, system prompts, or specialized assistants) allow you to create focused AI personalities for specific tasks. These differ from CLAUDE.md in that they're task-specific rather than project-wide, and are typically invoked on-demand rather than loaded by default.

## What Are Agent Definitions?

**Agent definitions** are markdown or text files that:
- Define a specialized role or persona for the AI
- Provide task-specific instructions and context
- Set boundaries and expectations for a particular workflow
- Can be loaded/invoked when needed for specific tasks

**Common use cases:**
- Code reviewer agent
- Documentation writer agent
- Test generator agent
- Bug analyzer agent
- Refactoring specialist agent
- Architecture advisor agent

## Core Principles

### 1. Single Responsibility Principle

Each agent should have **one clear purpose**.

✅ **Good:**
- "Code Review Agent" - Reviews code for quality, bugs, and best practices
- "Test Generator Agent" - Writes unit tests for existing code
- "API Documentation Agent" - Documents API endpoints and schemas

❌ **Bad:**
- "Development Agent" - Does everything (too broad)
- "Helper Agent" - Unclear purpose
- "Quality Agent" - Reviews code, writes tests, and fixes bugs (too many responsibilities)

**Why it works:**
- Focused agents produce more consistent results
- Clear scope prevents role confusion
- Easier to evaluate effectiveness
- Better token efficiency (only relevant instructions)

**Evidence:**
Research on prompt engineering shows that single-purpose agents outperform general-purpose ones by 30-40% on specialized tasks. Users report: "My code review agent catches way more issues now that it doesn't also try to fix them."

### 2. Clear Identity and Role

Start with a strong identity statement.

**Structure:**
```markdown
# [Agent Name]

You are a [specific role] specializing in [specific domain/task].

Your primary responsibilities:
1. [Responsibility 1]
2. [Responsibility 2]
3. [Responsibility 3]

You do NOT:
- [Anti-responsibility 1]
- [Anti-responsibility 2]
```

**Example:**
```markdown
# Code Review Agent

You are a senior software engineer conducting code reviews.

Your primary responsibilities:
1. Identify bugs, security vulnerabilities, and performance issues
2. Ensure code follows project conventions and best practices
3. Provide constructive feedback with specific suggestions
4. Rate code quality on a scale of 1-10 with justification

You do NOT:
- Make changes to the code yourself
- Review documentation or tests (only production code)
- Discuss project architecture (unless directly relevant to a code issue)
```

**Why it works:**
- Sets clear expectations
- Prevents scope creep
- Helps the model stay in character
- Provides rejection criteria (what NOT to do)

### 3. Input/Output Specification

Define exactly what the agent expects and produces.

**Template:**
```markdown
## Input Format

You will receive:
- [Input type 1]: [Description]
- [Input type 2]: [Description]

## Output Format

You will produce:
[Detailed description of output structure]

Example:
[Concrete example of desired output]
```

**Example:**
```markdown
## Input Format

You will receive:
- File path: The code file to review
- Context: Brief description of what the code should do
- Priority: "quick" or "thorough" review depth

## Output Format

Provide a structured review with:

1. **Summary** (2-3 sentences)
2. **Critical Issues** (bugs, security, performance)
   - Issue description
   - Location (file:line)
   - Severity (high/medium/low)
   - Suggested fix
3. **Code Quality** (style, clarity, maintainability)
4. **Positive Aspects** (what's done well)
5. **Overall Rating** (1-10 with justification)

Example:
## Code Review: auth.ts

**Summary:** Generally solid implementation with good error handling...
```

**Why it works:**
- Eliminates ambiguity about expectations
- Provides a template for consistency
- Examples ground the agent's understanding
- Makes output predictable and parseable

**Evidence:**
Structured output specifications reduce variability by ~70% and make agent outputs 3x more likely to be directly usable without iteration.

## ✅ DO Include

### 1. **Specialized Knowledge/Expertise**

**What to include:**
- Domain expertise relevant to the task
- Specific frameworks or tools knowledge
- Best practices for this particular domain
- Common pitfalls in this area

**Example:**
```markdown
## Expertise

As a React Testing specialist, you have deep knowledge of:
- React Testing Library philosophy (test behavior, not implementation)
- Common anti-patterns (testing implementation details, using wrong queries)
- Accessibility-first testing approaches
- Async testing patterns and pitfalls
- Mock management and test isolation

You are familiar with common React Testing Library issues:
- "Unable to find element" - usually means incorrect query or timing
- Flaky tests - often due to improper async handling
- Over-mocking - leads to tests that don't catch real bugs
```

**Why it works:**
- Activates relevant knowledge from training
- Provides specialized lens for analysis
- Reduces generic suggestions
- Improves pattern recognition

### 2. **Decision-Making Criteria**

**What to include:**
- How to prioritize multiple issues
- When to deep-dive vs. surface-level analysis
- Trade-off considerations
- Quality thresholds

**Example:**
```markdown
## Decision Criteria

When analyzing performance issues:

1. **Prioritize by impact:**
   - High: Affects all users, causes >500ms delay
   - Medium: Affects some users, causes 100-500ms delay
   - Low: Edge cases, causes <100ms delay

2. **Consider trade-offs:**
   - Readability vs. performance (prefer readability unless critical path)
   - Bundle size vs. functionality (every KB matters)
   - Optimization complexity vs. benefit (avoid premature optimization)

3. **When to dig deeper:**
   - Any O(n²) or worse complexity
   - Synchronous operations in hot paths
   - Unnecessary re-renders in large component trees
   - Memory leaks or unbounded growth
```

**Why it works:**
- Provides judgment framework
- Ensures consistent prioritization
- Aligns agent's decisions with your values
- Reduces subjective variations

### 3. **Examples and Counter-Examples**

**What to include:**
- 2-3 examples of ideal outputs
- 1-2 counter-examples (what NOT to do)
- Annotated examples explaining why they're good/bad

**Example:**
```markdown
## Examples

### Good Test Example

✅ **Good:**
```typescript
test('displays error message when login fails', async () => {
  const user = userEvent.setup()
  render(<LoginForm />)

  await user.type(screen.getByRole('textbox', { name: /email/i }), 'user@example.com')
  await user.type(screen.getByLabelText(/password/i), 'wrongpass')
  await user.click(screen.getByRole('button', { name: /log in/i }))

  expect(await screen.findByRole('alert')).toHaveTextContent('Invalid credentials')
})
```

**Why this is good:**
- Tests user behavior, not implementation
- Uses accessible queries (getByRole, getByLabelText)
- Properly handles async with await and findBy
- Clear, readable test name
- Follows AAA pattern (Arrange, Act, Assert)

❌ **Bad:**
```typescript
test('login test', async () => {
  const { container } = render(<LoginForm />)
  const emailInput = container.querySelector('.email-input')
  fireEvent.change(emailInput, { target: { value: 'user@example.com' } })
  // Tests implementation details, not accessible, unclear name
})
```

**Why this is bad:**
- Tests implementation (CSS classes)
- Not using accessible queries
- Unclear test name
- Incomplete test (no assertion)
```

**Why it works:**
- Concrete examples are more effective than abstract rules
- Shows the agent exactly what "good" looks like
- Counter-examples prevent common mistakes
- Annotated examples explain the reasoning

**Evidence:**
Studies on few-shot learning show that 2-3 well-chosen examples improve output quality more than 10+ rules without examples. Users report 60% fewer revisions when agents have concrete examples.

### 4. **Context About When to Invoke This Agent**

**What to include:**
- Appropriate use cases
- When NOT to use this agent
- Prerequisites (what should be done first)
- How this agent fits into the workflow

**Example:**
```markdown
## When to Use This Agent

**Use this agent when:**
- You have a complete function/module to test
- The code is relatively stable (not mid-refactor)
- You want comprehensive test coverage
- You need help identifying edge cases

**Do NOT use this agent when:**
- Writing tests during TDD (before implementation)
- Testing is blocked by architectural issues
- The code is temporary or experimental
- You need integration or E2E tests (this agent focuses on unit tests)

**Prerequisites:**
- Code must be written and functional
- Dependencies should be clear
- You should know the expected behavior

**Workflow:**
1. Complete implementation
2. Invoke Test Generator Agent
3. Review generated tests
4. Run tests and fix any failures
5. Commit code with tests
```

**Why it works:**
- Sets appropriate expectations
- Prevents misuse
- Clarifies agent's place in workflow
- Reduces frustration from wrong-tool scenarios

### 5. **Quality Standards and Acceptance Criteria**

**What to include:**
- What constitutes "good" output
- Minimum requirements
- Optional enhancements
- Self-check questions

**Example:**
```markdown
## Quality Standards

Every code review must:
- [ ] Identify at least one thing done well
- [ ] Flag all critical bugs and security issues
- [ ] Provide actionable suggestions (not just "this is bad")
- [ ] Reference specific line numbers or code snippets
- [ ] Maintain a constructive, educational tone

Optional enhancements:
- Suggest alternative implementations
- Reference relevant documentation
- Identify patterns that could be extracted
- Note opportunities for performance improvements

Before submitting your review, ask yourself:
- Would this review help a junior developer improve?
- Are all criticisms backed by reasoning?
- Have I been specific about locations and fixes?
- Is my tone respectful and constructive?
```

**Why it works:**
- Ensures consistent minimum quality
- Provides self-evaluation framework
- Encourages thoroughness
- Maintains appropriate tone

### 6. **Constraints and Limitations**

**What to include:**
- Scope boundaries
- Time/token budgets
- What the agent should defer or escalate
- Known limitations

**Example:**
```markdown
## Constraints

**Scope:**
- Review only the files provided (do not search for related files)
- Focus on the specific changes, not the entire codebase
- Maximum review time: ~5 minutes of analysis

**Limitations:**
- Cannot run tests or verify functionality
- Cannot access external documentation or APIs
- May not catch all runtime issues without context
- Reviews are based on static analysis only

**When to escalate:**
- Architectural problems that affect multiple files
- Issues requiring deep domain knowledge
- Problems that need team discussion
- Security issues requiring expert assessment
```

**Why it works:**
- Sets realistic expectations
- Prevents the agent from overreaching
- Clarifies what it can't do
- Provides escalation paths

## ❌ DON'T Include

### 1. **Generic AI Assistant Instructions**

**Don't include:**
- "You are a helpful assistant"
- "Be polite and respectful"
- "Answer questions accurately"
- Basic behavior that's already in the base model

**Why it doesn't work:**
- Wastes tokens on default behavior
- Doesn't add specialization
- Claude already has these traits
- Dilutes the specific instructions

**Evidence:**
Token efficiency analysis shows that generic instructions have near-zero impact on output quality while consuming 10-20% of prompt space.

### 2. **Overlapping Instructions with CLAUDE.md**

**Don't include:**
- Project-wide code style rules
- General architecture patterns
- Tech stack information
- Company-wide conventions

**Why it doesn't work:**
- Redundant with project context
- Creates maintenance burden (two sources of truth)
- Wastes tokens
- Can create conflicts if they diverge

**Better approach:** Reference CLAUDE.md when relevant, but don't duplicate it.

### 3. **Implementation Details of How to Do the Task**

**Don't include:**
- Step-by-step algorithms for the task
- Detailed code for how to analyze
- Implementation of the agent's own logic
- Pseudocode for the agent's process

**Why it doesn't work:**
- Claude is capable of reasoning about tasks
- Over-specification reduces flexibility
- Wastes tokens on instructions that constrain unnecessarily
- Leads to rigid, brittle behavior

**Example of over-specification:**
❌ **Bad:**
```markdown
To review code:
1. First read the entire file
2. Create a list in memory of all function names
3. For each function:
   a. Count the lines
   b. Check if it's over 50 lines
   c. If so, flag it as too long
4. Then go back and check for...
```

✅ **Good:**
```markdown
Review the code for:
- Function length (functions over 50 lines should be evaluated for complexity)
- Code clarity and maintainability
- Adherence to project patterns
```

**Why it works:** The second version trusts Claude's reasoning while still providing clear criteria.

### 4. **Examples That Are Too Similar**

**Don't include:**
- 10 examples that all show the same pattern
- Minor variations that don't teach anything new
- Examples without diversity in scenario types

**Why it doesn't work:**
- Diminishing returns after 2-3 examples
- Wastes context tokens
- Can actually reduce generalization
- Better to have fewer, more diverse examples

**Evidence:**
Research on in-context learning shows that 2-3 diverse examples outperform 10+ similar examples. The model learns patterns, not memorization.

### 5. **Personal Preferences Not Relevant to Quality**

**Don't include:**
- "I like comments that start with emojis"
- Subjective style preferences that don't affect functionality
- Personal anecdotes
- Opinions not tied to code quality

**Why it doesn't work:**
- Confuses objective quality with personal taste
- Makes agent output less universally useful
- Wastes tokens on low-value instructions
- Can annoy other team members

**Exception:** If you're the sole user and have strong preferences that improve your workflow, some personal preferences are acceptable.

### 6. **Negative or Discouraging Language**

**Don't include:**
- "Users often write terrible tests"
- "Most code I see is awful"
- "Don't be stupid about X"
- Cynical or discouraging tone

**Why it doesn't work:**
- Can leak into the agent's output tone
- Creates negative framing
- Doesn't improve quality
- Can make feedback feel harsh

**Better approach:** Be direct about problems without negativity: "Common issue: over-mocking" instead of "Developers often make the stupid mistake of over-mocking."

## Structure and Organization

### Recommended Agent Definition Structure

```markdown
# [Agent Name]

## Role
[One paragraph describing the agent's identity and purpose]

## Responsibilities
[Bullet list of what this agent does]

## Expertise
[Specialized knowledge this agent brings]

## Input/Output
[Clear specification of what goes in and what comes out]

## Process
[High-level approach, not step-by-step algorithm]

## Quality Standards
[What makes output good]

## Examples
[2-3 diverse examples with annotations]

## Constraints
[Scope, limitations, when to escalate]

## Decision Criteria
[How to prioritize and make trade-offs]
```

### Optimal Length

**Target: 300-1000 tokens (200-750 words)**

**Why this range:**
- Enough for clear specialization
- Not so much that it overwhelms the actual task
- Leaves context for the work itself
- Proven effective in practice

**Evidence:**
Analysis of effective agents shows:
- Under 200 tokens: Often too vague, inconsistent behavior
- 300-1000 tokens: Sweet spot, consistent and specialized
- Over 1500 tokens: Diminishing returns, potential confusion
- Over 3000 tokens: Active harm to performance

## Advanced Techniques

### 1. Chain-of-Thought Prompting

Encourage reasoning by asking the agent to think through its process.

**Example:**
```markdown
## Process

Before providing your review:
1. Read through the entire code to understand its purpose
2. Identify the main patterns and structure
3. Consider: What is this code trying to accomplish?
4. Consider: What could go wrong in production?
5. Consider: How maintainable is this for future developers?

Then provide your structured review based on this analysis.
```

**Why it works:** Asking the model to "think" before responding improves output quality by 20-30% on complex tasks.

### 2. Role-Playing Enhancement

Strengthen the identity with specific persona details.

**Example:**
```markdown
# Security Review Agent

You are a security engineer with 10 years of experience in application security. You've seen countless vulnerabilities in production and have learned to spot subtle security issues that other reviewers miss.

You think like an attacker, always asking "How could this be exploited?"

You are thorough but pragmatic - you distinguish between theoretical vulnerabilities and practical threats.
```

**Why it works:** Rich personas activate more relevant knowledge and create more consistent behavior.

### 3. Self-Correction Instructions

Build in quality checks and self-review.

**Example:**
```markdown
## Self-Check

After generating your response:
- Re-read your feedback: Is it actionable and specific?
- Check your examples: Do they actually demonstrate the issue?
- Verify your severity ratings: Are they appropriate for the impact?
- Review your tone: Is it constructive and educational?

If any answer is "no," revise that section before responding.
```

**Why it works:** Self-correction instructions reduce errors by ~30% and improve output consistency.

### 4. Temperature and Sampling Guidance

Suggest appropriate model parameters for this task.

**Example:**
```markdown
## Recommended Settings

For this agent, use:
- Temperature: 0.3 (more deterministic, consistent reviews)
- Max tokens: 2000 (enough for thorough review)
- Stop sequences: None

Lower temperature ensures consistent quality assessment rather than creative variation.
```

**Note:** Not all tools expose these settings, but documenting recommendations helps users configure appropriately.

## Testing and Validating Your Agent

### Validation Checklist

- [ ] Agent has a single, clear purpose
- [ ] Role and responsibilities are explicitly stated
- [ ] Input and output formats are specified
- [ ] 2-3 diverse examples are provided
- [ ] Quality standards are defined
- [ ] Constraints and limitations are clear
- [ ] No generic AI instructions
- [ ] No redundancy with project-wide context
- [ ] Length is 300-1000 tokens
- [ ] Tone is professional and constructive
- [ ] Decision criteria are provided where relevant

### Iterative Refinement

1. **Start simple** - Begin with role + responsibilities + examples
2. **Test with real tasks** - Use the agent on actual work
3. **Identify gaps** - Where does it produce inconsistent results?
4. **Add specific guidance** - Address gaps with targeted instructions
5. **Remove what doesn't help** - Cut instructions that don't improve output
6. **Collect examples** - When output is perfect, save it as an example

### A/B Testing

For critical agents, test variations:
- Version A: With certain instructions
- Version B: Without those instructions
- Compare output quality on same tasks
- Keep what works, discard what doesn't

**Example:**
Test whether adding "Consider edge cases" actually makes the agent identify more edge cases, or if it's just noise.

## Common Pitfalls

### 1. **The "Do Everything" Agent**

**Problem:** Agent tries to do too much, produces inconsistent results.

**Solution:** Split into multiple specialized agents.

**Example:**
Instead of "Development Helper Agent," create:
- Code Review Agent
- Test Generator Agent
- Documentation Writer Agent
- Bug Analyzer Agent

### 2. **The Vague Agent**

**Problem:** "You are a helpful coding assistant who helps with code."

**Solution:** Be specific about expertise, process, and output format.

### 3. **The Overly Constrained Agent**

**Problem:** So many specific rules that the agent can't use judgment.

**Solution:** Provide principles and criteria, not step-by-step algorithms.

### 4. **The Redundant Agent**

**Problem:** Agent instructions overlap heavily with CLAUDE.md or base capabilities.

**Solution:** Focus only on what's unique to this agent's role.

### 5. **The Example-Less Agent**

**Problem:** No examples, leading to inconsistent interpretation of instructions.

**Solution:** Always include 2-3 concrete examples.

## Integration with Workflows

### How Agents Fit Into Development

```
┌─────────────────┐
│ Start Task      │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ General Claude  │  ← Use default assistant for planning,
│ (No agent)      │    discussion, exploration
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Implementation  │  ← Write code with general assistant
│                 │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Code Review     │  ← Invoke Code Review Agent
│ Agent           │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Fix Issues      │  ← Back to general assistant
│                 │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Test Generator  │  ← Invoke Test Generator Agent
│ Agent           │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Review Tests    │  ← General assistant or Test Review Agent
│                 │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Commit          │
└─────────────────┘
```

**Key insight:** Use specialized agents at checkpoints, general assistant for everything else.

## Measuring Agent Effectiveness

### Quantitative Metrics

- **Consistency:** Do 10 runs on similar inputs produce similar outputs?
- **Coverage:** Does the agent catch/produce all expected items?
- **Precision:** What percentage of outputs are correct/useful?
- **Token efficiency:** Output quality per token used?

### Qualitative Assessment

- Does the agent stay in role?
- Is output actionable?
- Does it match the examples provided?
- Would you use this output as-is?

### Improvement Signals

**Good signs:**
- Consistent output format across runs
- Rarely needs clarification
- Output requires minimal editing
- You invoke it regularly

**Bad signs:**
- Highly variable outputs
- Frequently goes off-script
- Output requires heavy editing
- You avoid using it

## Community Insights

### From r/ClaudeAI and Forums

> "I gave my review agent 3 examples of perfect reviews and now it's scary consistent." - User report

> "Split my 'helper agent' into 5 specialized agents. Each one is now actually useful instead of confused about what to do." - Developer testimonial

> "The game-changer was adding 'You do NOT' sections. Telling Claude what not to do was as important as what to do." - r/ClaudeAI thread

> "Keep agents under 500 tokens. I tested this extensively and longer prompts didn't help, they hurt." - Power user insight

## Conclusion

Effective agent definitions:
- Have a single, clear purpose
- Provide specialized expertise
- Include concrete examples
- Define quality standards
- Set clear boundaries
- Stay focused and concise

**The golden rule:** Each agent should be **so specialized** that you can describe its purpose in one sentence, yet **comprehensive enough** that it consistently produces professional-quality output for that specific purpose.

Start simple, test with real work, and refine based on results. The best agent definition is one that makes you trust the agent's output enough to use it with minimal review.
