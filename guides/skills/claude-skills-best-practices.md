# Claude Skills Best Practices

## Overview

Claude Skills are modular capabilities that extend Claude's functionality through organized folders containing instructions, scripts, and resources. Unlike manual commands or static prompts, Skills are **automatically invoked** by Claude when relevant to your task, making them powerful tools for workflow automation and consistent task execution.

**Core concept:** Skills enable Claude to dynamically discover and load specialized capabilities exactly when needed, without cluttering the context window.

## What Are Claude Skills?

### Definition

**Claude Skills** are folders containing:
- **SKILL.md**: Core file with YAML metadata and instructions
- **Optional resources**: Scripts, templates, data files, documentation
- **Optional tools**: Executable code that Claude can run

**Key characteristics:**
- ✅ **Model-invoked**: Claude decides when to use them based on task relevance
- ✅ **Lazy-loaded**: Context loaded only when skill is activated
- ✅ **Reusable**: Same skill works across Claude.ai, Claude Code, and API
- ✅ **Composable**: Multiple skills can work together automatically

### How Skills Differ from Other Features

| Feature | Invocation | Context Loading | Use Case |
|---------|-----------|-----------------|----------|
| **Skills** | Automatic (Claude decides) | Lazy (when needed) | Repeatable procedures, specialized capabilities |
| **Slash Commands** | Manual (/command) | Immediate | Shortcuts, one-off tasks you trigger |
| **Agents/Subagents** | Programmatic | Isolated context | Complex multi-step tasks, delegation |
| **MCP Servers** | Automatic (via tools) | On-demand | External data/system access |
| **CLAUDE.md** | Always loaded | Upfront | Project-wide conventions |

**The key distinction:**
- **Skills**: "Claude, use this approach when you encounter this type of task"
- **Commands**: "I'm telling you to do this specific thing right now"
- **Agents**: "Spin up a specialized helper to work on this independently"

### Skills vs Slash Commands: Detailed Comparison

**Example scenario:** Creating React components

**As a Slash Command:**
```markdown
# .claude/commands/create-component.md

Create a new React component following our standards:
- Functional component with TypeScript
- Styled with Tailwind CSS
- Include PropTypes
- Export as default
```

**Usage:** `/create-component` - You must manually invoke every time

**As a Skill:**
```markdown
# skills/react-component-standards/SKILL.md
---
name: react-component-standards
description: Creating React components following project conventions with TypeScript, Tailwind CSS, and proper structure
---

When creating React components, follow these standards:
- Functional components with TypeScript
- Styled with Tailwind CSS
- Include PropTypes
- Export as default
[... detailed instructions ...]
```

**Usage:** Just ask "Create a UserProfile component" - Claude automatically applies the skill

**When to use each:**
- **Skill**: Repeatable pattern you want applied automatically whenever relevant
- **Command**: Specific action you want to trigger on-demand

## Availability and Requirements

### Platform Support

**Available on:**
- ✅ Claude.ai (Pro, Max, Team, Enterprise users)
- ✅ Claude Code (all users)
- ✅ Claude API (with Agent Skills support)

**Not available on:**
- ❌ Claude Free tier on claude.ai

### Official Skills Repository

**Location:** github.com/anthropics/skills

**Categories of official skills:**
- **Creative**: Algorithmic art, canvas design, music composition
- **Documents**: PDF, Excel, PowerPoint, Word
- **Development**: Webapp testing, MCP server creation
- **Enterprise**: Brand guidelines, communications, analysis

### Community Skills

**Repository:** github.com/travisvn/awesome-claude-skills

Contains curated community-created skills for various workflows.

## Skill Structure

### Basic Anatomy

**Minimum viable skill:**
```
my-skill/
└── SKILL.md
```

**Complete skill structure:**
```
my-skill/
├── SKILL.md              # Required: Core skill definition
├── templates/            # Optional: File templates
│   ├── report.md
│   └── analysis.txt
├── scripts/              # Optional: Executable tools
│   ├── process.py
│   └── validate.sh
├── data/                 # Optional: Reference data
│   └── standards.json
└── docs/                 # Optional: Extended documentation
    └── examples.md
```

### SKILL.md Format

**Required structure:**

```markdown
---
name: skill-name-here
description: What this skill does and when to use it
---

# Skill Instructions

[Core instructions for Claude]

## When to Use This Skill

[Guidance on appropriate use cases]

## Process

[Step-by-step approach]

## Examples

[Concrete examples]
```

### Metadata Fields (YAML Frontmatter)

**Required fields:**

```yaml
---
name: my-skill-name
description: Brief description of what the skill does and when to use it
---
```

**Field requirements:**

**name:**
- Format: lowercase letters, numbers, hyphens only
- Max length: 64 characters
- Examples: `brand-guidelines`, `test-generator`, `api-documentation`

**description:**
- Max length: 1024 characters
- **Critical for skill discovery** - Claude uses this to decide when to invoke
- Should include: what it does + when to use it
- Write in third person (gets injected into system prompt)

**Optional metadata fields:**

```yaml
---
name: advanced-skill
description: A skill with all metadata
version: 1.0.0
author: Your Name
tags: [testing, automation, qa]
dependencies: [other-skill-name]
---
```

## ✅ DO: Best Practices

### 1. **Use Gerund Form for Skill Names**

**Pattern:** verb + -ing

✅ **Good names:**
- `testing-web-applications`
- `generating-api-documentation`
- `analyzing-performance-metrics`
- `creating-react-components`
- `formatting-technical-reports`

❌ **Bad names:**
- `test-app` (imperative, unclear)
- `api-docs` (noun, not action-oriented)
- `performance` (too vague)
- `react` (too broad)

**Why it works:** Gerund form clearly describes the activity or capability, making it obvious when the skill should be invoked.

**Evidence:** Official Anthropic best practices documentation recommends gerund form for clarity.

### 2. **Write Excellent Descriptions**

**The description is the most critical part of your skill.** Claude uses it to decide whether to invoke the skill.

✅ **Good description:**
```yaml
description: Generating comprehensive API documentation from TypeScript code with examples, error cases, and request/response schemas. Use when documenting REST APIs or creating OpenAPI specifications.
```

**Why it's good:**
- States what it does: "Generating comprehensive API documentation"
- Specifies approach: "from TypeScript code with examples..."
- Indicates when to use: "Use when documenting REST APIs..."
- Includes keywords: "API", "documentation", "OpenAPI", "TypeScript"

❌ **Bad description:**
```yaml
description: Makes documentation for APIs
```

**Why it's bad:**
- Too vague
- Doesn't indicate when to use
- Missing important details about approach
- Won't match well against user tasks

**Template for good descriptions:**
```
[Action in gerund form] [specific target] [using/following approach] [key characteristics]. Use when [specific scenarios].
```

**Examples:**
```yaml
# Good
description: Testing web applications using Playwright with comprehensive coverage of user flows, edge cases, and accessibility. Use when creating E2E tests for React or Next.js applications.

# Good
description: Applying brand guidelines consistently across all communications including tone, visual elements, and messaging. Use when creating marketing materials, documentation, or customer-facing content.

# Good
description: Generating TypeScript type definitions from JSON schema with validation logic and documentation comments. Use when integrating external APIs or defining data models.
```

### 3. **Keep SKILL.md Under 500 Lines**

**Guideline:** Core SKILL.md should be under 500 lines for optimal performance.

**Why:**
- Claude reads the entire SKILL.md when skill is invoked
- Large files slow down skill activation
- Harder to maintain and update
- May impact response time

**When you approach 500 lines:**

**Strategy 1: Split into sections**
```markdown
# Main SKILL.md (under 500 lines)
Core instructions and process

## Extended Documentation
For detailed examples, see:
- docs/examples.md
- docs/advanced-usage.md
- docs/troubleshooting.md
```

**Strategy 2: Use progressive disclosure**
```markdown
# SKILL.md
Basic approach and common cases

For complex scenarios, Claude can read:
- templates/complex-report.md
- templates/data-analysis.md
```

**Strategy 3: Extract to separate files**
```
skill/
├── SKILL.md (core, <500 lines)
├── templates/ (referenced as needed)
├── examples/ (referenced as needed)
└── reference/ (referenced as needed)
```

### 4. **Use Progressive Disclosure**

**Core principle:** Structure information so Claude loads details only as needed.

**Pattern:**
```markdown
# SKILL.md

## Overview
High-level approach [50-100 lines]

## Common Cases
Standard procedures [100-200 lines]

## Advanced Scenarios
Complex cases - see docs/advanced.md [reference only]

## Edge Cases
Rare situations - see docs/edge-cases.md [reference only]

## Full Reference
Complete specification in reference/ directory
```

**Why it works:**
- SKILL.md stays concise and readable
- Common cases load immediately
- Advanced details available but not front-loaded
- Scales to unlimited complexity
- **Effectively unbounded context** because Claude only loads what's needed

**Evidence:** "Progressive disclosure is the core design principle that makes Agent Skills flexible and scalable." - Anthropic Engineering Blog

### 5. **Start with Pre-Built Skills**

**Before creating custom skills, explore official ones.**

**Learning path:**
1. Install and use official skills (PDF, Excel, PowerPoint)
2. Study their SKILL.md files
3. Understand structure and patterns
4. Adapt patterns for your needs
5. Create custom skills

**Example - Study the PDF skill:**
```bash
# View official PDF skill
curl https://raw.githubusercontent.com/anthropics/skills/main/document-skills/pdf/SKILL.md
```

**What you'll learn:**
- How to structure complex instructions
- How to handle edge cases
- How to provide examples
- How to organize resources

**Benefits:**
- Learn proven patterns
- Avoid common mistakes
- Understand best practices
- Get inspiration for your skills

### 6. **Create Separate Skills for Different Workflows**

**Principle:** Multiple focused skills compose better than one large skill.

❌ **Bad: One mega-skill**
```
development-workflow/
└── SKILL.md  (2000 lines covering testing, docs, deployment, etc.)
```

✅ **Good: Focused skills**
```
skills/
├── testing-react-components/
│   └── SKILL.md (300 lines, just testing)
├── generating-api-docs/
│   └── SKILL.md (250 lines, just docs)
└── deploying-to-production/
    └── SKILL.md (200 lines, just deployment)
```

**Why separation works:**
- Each skill has clear trigger conditions
- Easier to maintain and update
- Better performance (smaller files)
- Can mix and match skills
- Multiple skills can coordinate automatically

**Real-world example:**

Instead of "software-development" skill, create:
- `writing-unit-tests` - Test generation
- `code-reviewing` - Review process
- `refactoring-legacy-code` - Refactoring patterns
- `documenting-apis` - API documentation

Claude will use the right combination automatically.

### 7. **Include Concrete Examples**

**Examples help Claude understand your intent.**

**Pattern in SKILL.md:**
```markdown
## Examples

### Example 1: Basic Use Case

**Input:**
User asks: "Create a login component"

**Expected Output:**
```tsx
// LoginForm.tsx
import React, { useState } from 'react';

export default function LoginForm() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    // Implementation
  };

  return (
    <form onSubmit={handleSubmit} className="space-y-4">
      {/* Form fields */}
    </form>
  );
}
```

### Example 2: Advanced Use Case
[Another example]
```

**How many examples:**
- Minimum: 1 example (basic use case)
- Ideal: 2-3 examples (basic, intermediate, advanced)
- Maximum: 5 examples (diminishing returns after this)

**What to show:**
- ✅ Input that triggers the skill
- ✅ Expected output/behavior
- ✅ Edge cases or variations
- ❌ Don't show every possible variation (overwhelming)

### 8. **Write in Third Person**

**The description gets injected into the system prompt.**

✅ **Good (third person):**
```yaml
description: Generating React components following TypeScript and Tailwind CSS conventions. Creates functional components with proper typing and accessibility. Use when creating new UI components.
```

❌ **Bad (first/second person):**
```yaml
description: I generate React components for you. You use this when you want to create components. I follow TypeScript conventions.
```

**Why third person:**
- Consistent with how Claude receives instructions
- Prevents point-of-view confusion
- More professional and clear
- Matches system prompt style

**Evidence:** "The description is injected into the system prompt, and inconsistent point-of-view can cause discovery problems." - Anthropic documentation

### 9. **Use the Skill Creator Skill**

**Bootstrap new skills using the official skill-creator skill.**

**How it works:**
1. You describe your workflow to Claude
2. Skill creator asks clarifying questions
3. It generates the folder structure
4. Creates properly formatted SKILL.md
5. Bundles necessary resources

**Example interaction:**
```
You: I need a skill for generating commit messages following conventional commits

Claude (with skill-creator):
- What commit types do you use? (feat, fix, docs, etc.)
- What's your preferred format?
- Any special rules or patterns?
- Should it analyze staged changes?

[After questions]

Claude: I've created commit-message-generator skill:
- SKILL.md with your conventions
- Templates for different commit types
- Examples of good commit messages
```

**Benefits:**
- Faster skill creation
- Follows best practices automatically
- Ensures proper structure
- Handles boilerplate

**How to use:**
```
"Use the skill-creator skill to help me create a new skill for [your workflow]"
```

### 10. **Test Skills Incrementally**

**Don't wait until skill is "perfect" - test early and often.**

**Iterative testing approach:**

**Iteration 1: Minimum viable skill**
```yaml
---
name: my-new-skill
description: Basic description
---

# Core instructions
[Minimal instructions]
```

Test: Does Claude invoke it when appropriate?

**Iteration 2: Add details**
```markdown
# Enhanced instructions
[More detailed guidance]

## Common cases
[Handle common scenarios]
```

Test: Does it handle common cases well?

**Iteration 3: Add examples**
```markdown
## Examples
[Concrete examples]
```

Test: Does output quality improve?

**Iteration 4: Add edge cases**
```markdown
## Edge Cases
[Rare situations]
```

Test: Does it handle unusual inputs?

**Iteration 5: Optimize**
- Refine description for better discovery
- Streamline instructions
- Add progressive disclosure if needed

**Benefits:**
- Early feedback on skill effectiveness
- Avoid investing in wrong approach
- Iterative improvement
- Catch discovery issues early

## ❌ DON'T: Anti-Patterns

### 1. **Don't Make Skills Too Broad**

**Problem:** Skills that try to do everything.

❌ **Too broad:**
```yaml
---
name: software-development
description: Everything related to developing software including coding, testing, deployment, documentation, and project management
---
```

**Why it fails:**
- Claude can't tell when to invoke it (too generic)
- Instructions become massive and unfocused
- Conflicts with other skills
- Hard to maintain
- Poor performance

✅ **Better approach:**
Create focused skills:
- `testing-web-applications`
- `generating-api-documentation`
- `deploying-to-aws`
- `code-reviewing-python`

**Rule of thumb:** If your skill description uses "and" more than twice, it's probably too broad.

### 2. **Don't Write Vague Descriptions**

**Problem:** Descriptions that don't help Claude decide when to invoke.

❌ **Bad descriptions:**
```yaml
# Too vague
description: Helps with coding

# Too generic
description: A useful development tool

# Missing context
description: Generates documentation

# No trigger indication
description: Following best practices for React development
```

**Why they fail:**
- Claude can't match task to skill
- May invoke at wrong times
- May never invoke when needed
- Wastes skill creation effort

✅ **Fix:** Add specifics, triggers, and context
```yaml
# Good
description: Generating comprehensive API documentation from OpenAPI/Swagger specifications with code examples and authentication details. Use when documenting REST APIs or creating developer guides.
```

### 3. **Don't Ignore Skill Discovery**

**Problem:** Creating skills that Claude never discovers.

**Common discovery failures:**

**Issue 1: Description doesn't match user language**
```yaml
# Skill description uses:
description: Generating unit tests with Jest framework

# But users say:
"Write tests for this component" ❌ (may not trigger)
"Create test coverage" ❌ (may not trigger)
```

**Fix:** Include common phrasings
```yaml
description: Writing unit tests and creating test coverage for React components using Jest and Testing Library. Use when asked to test components, add tests, or improve test coverage.
```

**Issue 2: Too technical description**
```yaml
# Too technical - user might not say these words
description: Implementing OAuth 2.0 authorization code flow with PKCE extension
```

**Fix:** Include plain language
```yaml
description: Setting up secure user login with Google, GitHub, or other OAuth providers using industry-standard OAuth 2.0. Use when implementing social login, third-party authentication, or single sign-on.
```

**Issue 3: Missing keywords**
```yaml
# Missing obvious keywords
description: Creating components for user interfaces
```

**Fix:** Include synonyms and variations
```yaml
description: Creating React components, UI elements, and interface widgets following TypeScript and Tailwind conventions. Use when building components, creating interfaces, or developing UI features.
```

**Test discovery:**
Ask yourself: "What words would I use when I want this skill activated?"
Include those words in the description.

### 4. **Don't Put Everything in SKILL.md**

**Problem:** 3000-line SKILL.md files that slow everything down.

❌ **Anti-pattern:**
```markdown
# SKILL.md (3000 lines)

## Overview
[100 lines]

## Complete Style Guide
[500 lines of detailed style guide]

## Full API Reference
[1000 lines of API documentation]

## All Examples
[100 examples, 1000 lines]

## Complete Edge Cases
[400 lines of edge cases]
```

**Why it fails:**
- Slow skill activation
- Claude must read entire file
- Hard to maintain
- Poor performance
- Exceeds 500-line guideline

✅ **Fix:** Use progressive disclosure
```markdown
# SKILL.md (400 lines)

## Overview
[50 lines]

## Core Approach
[150 lines]

## Common Cases
[100 lines]

## Examples
[50 lines - basic examples]

## Extended Resources
For detailed information:
- Full style guide: reference/style-guide.md
- API reference: reference/api.md
- All examples: examples/
- Edge cases: docs/edge-cases.md

Claude can read these files when needed.
```

### 5. **Don't Duplicate CLAUDE.md Content**

**Problem:** Repeating project-wide conventions in skills.

❌ **Bad pattern:**
```
# CLAUDE.md
- Use TypeScript
- Use Tailwind CSS
- Export as default
[... project conventions ...]

# skills/component-creator/SKILL.md
- Use TypeScript
- Use Tailwind CSS
- Export as default
[... same conventions repeated ...]
```

**Why it's bad:**
- Duplication maintenance burden
- Wastes tokens
- Can diverge and contradict
- CLAUDE.md already loaded

✅ **Better:**
```
# CLAUDE.md
- Use TypeScript
- Use Tailwind CSS
- Export as default
[... project conventions ...]

# skills/component-creator/SKILL.md
When creating components:
- Follow project TypeScript and Tailwind conventions (per CLAUDE.md)
- Specific to components: include PropTypes, use memo for expensive renders
- Component file structure: [specific guidance]
```

**Rule:** Skills add specialized knowledge. CLAUDE.md covers project-wide basics.

### 6. **Don't Make Skills Context-Unaware**

**Problem:** Skills that don't integrate with existing project context.

❌ **Bad skill:**
```markdown
# testing-skill/SKILL.md

Always create tests using Jest with this exact structure:
[Rigid template]
```

**Problem:** Doesn't adapt to projects using Vitest, or different test structures.

✅ **Better skill:**
```markdown
# testing-skill/SKILL.md

Create tests following the project's testing framework and patterns:
1. Check existing test files for the framework (Jest, Vitest, etc.)
2. Match the established pattern and structure
3. Use project's test utilities and helpers
4. Follow project's assertion style

Core testing principles (framework-agnostic):
- Test user behavior, not implementation
- Cover edge cases
- Use descriptive test names
- Arrange-Act-Assert pattern
```

**Principle:** Skills should adapt to context, not force rigid patterns.

### 7. **Don't Skip Testing**

**Problem:** Creating skills without verifying they work.

**Untested skill risks:**
- Never gets invoked (bad description)
- Invoked at wrong times (bad triggers)
- Produces wrong output (bad instructions)
- Conflicts with other skills
- Performance issues

✅ **Testing checklist:**

**Discovery testing:**
```
□ Does Claude invoke skill when you expect it to?
□ Does Claude NOT invoke it for unrelated tasks?
□ Try 3-5 different phrasings of the task
```

**Output testing:**
```
□ Does skill produce expected output?
□ Does it handle edge cases?
□ Does it follow project conventions?
□ Is output quality consistent?
```

**Integration testing:**
```
□ Does it work with other skills?
□ Does it conflict with CLAUDE.md?
□ Does it work across different projects?
```

**Performance testing:**
```
□ Does skill activation feel fast?
□ Is SKILL.md under 500 lines?
□ Are resources properly structured?
```

## Common Use Cases and Examples

### Use Case 1: Brand Guidelines

**Scenario:** Ensure all content follows company brand standards.

**Skill structure:**
```
brand-guidelines/
├── SKILL.md
├── voice-and-tone.md
├── visual-standards.md
└── examples/
    ├── good-examples.md
    └── bad-examples.md
```

**SKILL.md:**
```markdown
---
name: applying-brand-guidelines
description: Ensuring all communications and content follow company brand standards for voice, tone, messaging, and visual elements. Use when creating marketing materials, documentation, customer-facing content, or any branded communications.
---

# Brand Guidelines

When creating any content, ensure it follows our brand standards:

## Voice and Tone
- See voice-and-tone.md for detailed guidelines
- Professional but approachable
- Clear and concise
- Empowering, not prescriptive

## Key Messaging
- Focus on customer benefits
- Lead with value proposition
- Avoid jargon and technical speak in marketing
- Use active voice

## Visual Elements
(For visual content)
- See visual-standards.md
- Primary colors: [colors]
- Typography: [fonts]
- Logo usage: [guidelines]

## Examples
See examples/good-examples.md for approved content samples
```

**Result:** All content automatically follows brand standards without manual checking.

### Use Case 2: Testing Web Applications

**Scenario:** Consistent E2E testing with Playwright.

**Skill structure:**
```
webapp-testing/
├── SKILL.md
├── templates/
│   ├── basic-test.ts
│   └── authenticated-test.ts
└── helpers/
    └── common-selectors.ts
```

**SKILL.md excerpt:**
```markdown
---
name: testing-web-applications
description: Creating comprehensive E2E tests for web applications using Playwright with focus on user flows, accessibility, and edge cases. Use when writing tests for React, Next.js, or other web applications.
---

# Web Application Testing

## Approach
1. Test user behavior, not implementation details
2. Use accessible selectors (roles, labels)
3. Cover happy path and error cases
4. Test responsive behavior when relevant

## Test Structure
```typescript
import { test, expect } from '@playwright/test';

test.describe('Feature Name', () => {
  test('should do expected behavior', async ({ page }) => {
    // Arrange
    await page.goto('/path');

    // Act
    await page.getByRole('button', { name: 'Submit' }).click();

    // Assert
    await expect(page.getByText('Success')).toBeVisible();
  });
});
```

## Best Practices
- Use getByRole, getByLabel for accessibility
- Wait for explicit conditions, avoid arbitrary timeouts
- Isolate tests (independent execution)
- See templates/ for common patterns
```

### Use Case 3: API Documentation

**Scenario:** Generate consistent API docs from code.

**Skill structure:**
```
api-documentation/
├── SKILL.md
└── templates/
    ├── endpoint-template.md
    └── openapi-template.yaml
```

**SKILL.md excerpt:**
```markdown
---
name: generating-api-documentation
description: Creating comprehensive API documentation from TypeScript/JavaScript code including endpoints, request/response schemas, examples, and error cases. Use when documenting REST APIs or creating OpenAPI specifications.
---

# API Documentation

## What to Include

### For Each Endpoint
1. **HTTP Method and Path**
2. **Description**: What it does
3. **Authentication**: Required auth level
4. **Request**:
   - Path parameters
   - Query parameters
   - Request body schema
5. **Response**:
   - Success response (200, 201, etc.)
   - Error responses (400, 401, 404, 500)
   - Response body schema
6. **Example Request**
7. **Example Response**

## Format
Use OpenAPI 3.0 specification format

See templates/endpoint-template.md for structure
```

### Use Case 4: Code Review

**Scenario:** Consistent code review process.

**SKILL.md excerpt:**
```markdown
---
name: reviewing-code-changes
description: Conducting thorough code reviews focusing on correctness, security, performance, and maintainability. Use when reviewing pull requests, code changes, or conducting code audits.
---

# Code Review Process

## Review Checklist

### Correctness
- [ ] Does code solve the stated problem?
- [ ] Are edge cases handled?
- [ ] Is error handling appropriate?
- [ ] Are there any obvious bugs?

### Security
- [ ] No SQL injection vulnerabilities
- [ ] No XSS vulnerabilities
- [ ] Authentication/authorization correct
- [ ] Sensitive data properly handled

### Performance
- [ ] No unnecessary loops or operations
- [ ] Database queries optimized
- [ ] Caching used appropriately
- [ ] No memory leaks

### Maintainability
- [ ] Code is readable and clear
- [ ] Proper naming conventions
- [ ] Comments where needed
- [ ] Tests included and passing

## Feedback Format
- Be specific and constructive
- Reference line numbers
- Suggest improvements
- Note what's done well
```

## Integration Patterns

### Skills + CLAUDE.md

**Best practice:** Use them together, not redundantly.

**Pattern:**
```
CLAUDE.md:
- Project-wide conventions (tech stack, style, patterns)
- Always loaded

Skills:
- Specialized procedures (testing approach, documentation format)
- Loaded when relevant
```

**Example:**
```markdown
# CLAUDE.md
Tech Stack:
- TypeScript
- React 18
- Tailwind CSS
- Vitest for testing

# skills/component-creation/SKILL.md
When creating components:
- Follow project's TypeScript and Tailwind standards (per CLAUDE.md)
- Component-specific patterns:
  * Use memo() for expensive renders
  * Include PropTypes
  * Export default
```

### Skills + MCP

**Skills can reference MCP tools.**

**Pattern:**
```markdown
# skills/database-query/SKILL.md

When analyzing database performance:
1. Use the database MCP tool to query metrics
2. Analyze slow queries
3. Suggest optimizations
4. Generate migration if needed

The MCP database tool provides:
- Query execution
- Schema inspection
- Performance metrics
```

### Skills + Slash Commands

**When to use each:**

**Use Skill when:**
- Want automatic application
- Repeatable procedure
- Should apply whenever relevant

**Use Slash Command when:**
- Want explicit control
- One-off task
- Manual trigger needed

**Example:**

**Skill:** `applying-security-review` - automatically applied during code review
**Command:** `/deploy-to-prod` - manually trigger deployment

### Skills + Agents

**Skills can be used by agents/subagents.**

**Pattern:**
Agent invokes → Agent can use skills → Skills enhance agent capabilities

**Example:**
```
Main Claude: "Let me delegate this to the code review agent"
  → Spawns code review agent
  → Agent has access to reviewing-code-changes skill
  → Agent uses skill to conduct review
  → Returns findings to main Claude
```

## Measuring Skill Effectiveness

### Quantitative Metrics

**Invocation Rate:**
```
Are skills being invoked when expected?

Track:
- Tasks where skill should apply
- Times skill was invoked
- Invocation rate = Invocations / Expected uses

Target: >80% invocation rate
```

**Output Quality:**
```
Does skill-assisted output meet standards?

Track:
- Output quality rating (1-5)
- Need for revisions
- Time to acceptable output

Target: Quality >4.0, minimal revisions
```

**Time Savings:**
```
Does skill save time?

Compare:
- Time with skill
- Time without skill

Target: 30%+ time savings
```

### Qualitative Indicators

**Good signs:**
- ✅ Claude invokes skill without being told
- ✅ Output consistently follows procedures
- ✅ Fewer corrections needed
- ✅ Team members adopt the skill
- ✅ Skill works across different projects

**Bad signs:**
- ❌ Must manually tell Claude to use skill
- ❌ Skill invoked at wrong times
- ❌ Output doesn't match expectations
- ❌ Skill conflicts with other features
- ❌ Team avoids using it

### A/B Testing

**Test skill effectiveness:**

**Scenario A (with skill):**
```
10 tasks requiring the procedure
→ Invoke skill automatically
→ Measure: quality, time, consistency
```

**Scenario B (without skill):**
```
10 similar tasks
→ Standard prompting
→ Measure: quality, time, consistency
```

**Compare:**
- Quality difference
- Time difference
- Consistency improvement

**Iterate:** Refine skill based on results

## Advanced Techniques

### 1. Skill Composition

**Multiple skills working together.**

**Example:**
```
Task: "Create a new feature with tests and documentation"

Claude automatically uses:
1. component-creation skill (creates the component)
2. testing-components skill (creates tests)
3. api-documentation skill (documents the feature)

All three coordinate without manual orchestration
```

**Why it works:** Skills have clear, non-overlapping scopes. Claude knows when to use each.

### 2. Conditional Skill Logic

**Skills that adapt to context.**

**Pattern in SKILL.md:**
```markdown
# Adaptive skill

## Check Project Context
1. If project uses Jest → use Jest patterns
2. If project uses Vitest → use Vitest patterns
3. If no testing framework → suggest installation

## Adapt to Code Style
1. Check existing files for style
2. Match indentation, quotes, semicolons
3. Follow established patterns
```

**Why it works:** Skill is flexible, not rigid. Adapts to each project.

### 3. Skill Templates

**Skills that generate from templates.**

**Structure:**
```
skill-with-templates/
├── SKILL.md
└── templates/
    ├── basic.tsx
    ├── with-state.tsx
    └── with-api.tsx
```

**SKILL.md:**
```markdown
When creating components:
1. Assess complexity
2. Choose appropriate template:
   - Simple display → templates/basic.tsx
   - Needs state → templates/with-state.tsx
   - Fetches data → templates/with-api.tsx
3. Customize template for specific needs
```

**Result:** Consistent starting point, customized for each use.

### 4. Multi-File Skills

**Skills spanning multiple files.**

**Structure:**
```
comprehensive-testing/
├── SKILL.md (orchestration)
├── unit-testing.md
├── integration-testing.md
├── e2e-testing.md
└── test-data/
    └── fixtures.json
```

**SKILL.md:**
```markdown
# Comprehensive Testing

## Determine Test Level
Based on what's being tested:
- Pure functions, utilities → unit-testing.md
- Component integration → integration-testing.md
- User flows → e2e-testing.md

## Process
1. Identify appropriate test level
2. Load relevant testing guide
3. Use fixtures from test-data/
4. Generate tests following that guide
```

**Why it works:** Progressive disclosure. Load only relevant testing approach.

## Troubleshooting

### Problem: Skill Never Invoked

**Diagnosis:**
- Description doesn't match user language
- Too vague or generic
- Missing keywords

**Fix:**
1. Review description
2. Add common phrasings users might say
3. Include synonyms and variations
4. Test with different wordings

**Before:**
```yaml
description: Generating tests with Jest
```

**After:**
```yaml
description: Writing unit tests and test coverage using Jest and Testing Library for React components. Use when asked to test components, add tests, create test coverage, or write test cases.
```

### Problem: Skill Invoked at Wrong Times

**Diagnosis:**
- Description too broad
- Missing "when to use" guidance
- Conflicts with other skills

**Fix:**
1. Narrow description scope
2. Add explicit trigger conditions
3. Add "Use when..." guidance
4. Review for conflicts

### Problem: Poor Output Quality

**Diagnosis:**
- Instructions unclear
- Missing examples
- Conflicting guidance

**Fix:**
1. Add concrete examples
2. Clarify step-by-step process
3. Remove contradictions
4. Test and iterate

### Problem: Slow Performance

**Diagnosis:**
- SKILL.md too large
- Not using progressive disclosure
- Loading unnecessary resources

**Fix:**
1. Reduce SKILL.md to <500 lines
2. Move details to separate files
3. Use progressive disclosure
4. Reference don't include

## Community Resources

### Official Resources

**Documentation:** docs.claude.com/en/docs/agents-and-tools/agent-skills
**Repository:** github.com/anthropics/skills
**Blog:** anthropic.com/news/skills

### Community Resources

**Awesome Claude Skills:** github.com/travisvn/awesome-claude-skills
**Tutorials:** Various community tutorials on Medium, blogs

### Getting Help

**Official support:** support.claude.com
**Community:** Reddit r/ClaudeAI discussions on skills

## Future of Skills

### Current State (2025)

- Available on Pro/Max/Team/Enterprise tiers
- Growing library of official skills
- Active community creation
- Integration with Claude Code, API

### Near-Term Evolution

**Expected developments:**
- More official skills (data analysis, design, etc.)
- Better skill discovery mechanisms
- Skill marketplace/sharing platform
- Enhanced composition capabilities
- Performance improvements

### Long-Term Vision

**Potential future:**
- Natural language skill creation
- Automatic skill suggestion
- Cross-team skill sharing
- Enterprise skill governance
- AI-assisted skill optimization

## Conclusion

Claude Skills represent a paradigm shift in AI interaction:

**From:** Repeating instructions every time
**To:** Define once, apply automatically forever

### Key Takeaways

1. ✅ **Skills are automatic** - Claude decides when to use them
2. ✅ **Description is critical** - Determines discovery and invocation
3. ✅ **Keep focused** - One skill, one purpose
4. ✅ **Use gerund naming** - Clear action-oriented names
5. ✅ **Progressive disclosure** - Core in SKILL.md, details in other files
6. ✅ **Test thoroughly** - Verify discovery, output, integration
7. ✅ **Start with pre-built** - Learn from official skills
8. ✅ **Compose skills** - Multiple focused skills work together
9. ❌ **Don't make broad** - Specific beats general
10. ❌ **Don't duplicate** - Skills add to CLAUDE.md, not replace

### When to Create Skills

**Create a skill when:**
- You've figured out how you want a task done
- You want that approach applied consistently
- The procedure is repeatable
- It's specialized enough to warrant separation
- You'll use it multiple times

**Don't create a skill when:**
- It's a one-off task (use slash command)
- It's project-wide convention (use CLAUDE.md)
- It's too simple to need automation
- You're still figuring out the approach

### The Bottom Line

Skills make Claude **consistently excellent** at specialized tasks without requiring you to repeat instructions.

**The magic:** Define your workflow once → Claude applies it automatically forever → Consistent, high-quality results.

Skills are one of the most powerful features for customizing Claude to your specific workflows and standards. Used well, they dramatically improve consistency and reduce the need for repetitive prompting.

---

**Further Reading:**
- Official Skills Docs: docs.claude.com/en/docs/agents-and-tools/agent-skills
- Skills Repository: github.com/anthropics/skills
- Best Practices: docs.claude.com/en/docs/agents-and-tools/agent-skills/best-practices
- Community Skills: github.com/travisvn/awesome-claude-skills
