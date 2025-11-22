# Executive Summary: Claude Skills Best Practices

## What Are Skills

Skills are folders containing SKILL.md files with YAML metadata and instructions. Claude automatically invokes them when relevant to tasks without manual triggers.

**Key characteristics:**
- Model-invoked (Claude decides when to use)
- Lazy-loaded (context loaded only when activated)
- Reusable across Claude.ai, Claude Code, and API
- Composable (multiple skills work together)

## Platform Availability

- Available: Claude.ai (Pro/Max/Team/Enterprise), Claude Code, Claude API
- Not available: Claude Free tier
- Official repository: github.com/anthropics/skills
- Community repository: github.com/travisvn/awesome-claude-skills

## Skills vs Other Features

- **Skills**: Auto-applied for repeatable procedures
- **Slash Commands**: Manually triggered for one-off tasks
- **CLAUDE.md**: Always loaded project-wide conventions
- **Agents**: Isolated context for complex delegation

## SKILL.md Structure

**Minimum required:**
```
my-skill/
└── SKILL.md
```

**Required YAML metadata:**
- `name`: lowercase, hyphens only, max 64 chars
- `description`: max 1024 chars, critical for skill discovery

**Optional metadata:** version, author, tags, dependencies

## Critical Best Practices

### 1. Use Gerund Form for Names
- Pattern: verb + -ing
- Good: `testing-web-applications`, `generating-api-documentation`
- Bad: `test-app`, `api-docs`

### 2. Write Excellent Descriptions
Description determines if Claude invokes the skill.

**Template:** [Action in gerund] [specific target] [using approach] [characteristics]. Use when [scenarios].

**Must include:**
- What it does
- How it approaches tasks
- When to use it
- Relevant keywords

### 3. Keep SKILL.md Under 500 Lines
- Entire SKILL.md loads when skill activates
- Large files slow performance
- Use progressive disclosure for additional content

### 4. Progressive Disclosure
- Core instructions in SKILL.md
- Extended details in separate files
- Claude reads additional files only when needed
- Enables effectively unbounded context

### 5. Create Separate Skills per Workflow
Multiple focused skills compose better than one large skill.
- Each has clear trigger conditions
- Easier to maintain
- Better performance
- Skills coordinate automatically

### 6. Include Concrete Examples
- Minimum 1 example, ideal 2-3
- Show input that triggers skill
- Show expected output/behavior
- Cover edge cases

### 7. Write in Third Person
Description injects into system prompt; use third person for consistency.

### 8. Test Incrementally
- Start with minimum viable skill
- Test discovery and invocation
- Add details iteratively
- Validate output quality at each stage

## Critical Anti-Patterns

### 1. Don't Make Skills Too Broad
If description uses "and" more than twice, skill is too broad.

### 2. Don't Write Vague Descriptions
Must include specifics, triggers, and context for Claude to match tasks to skills.

### 3. Don't Ignore Discovery
- Include common user phrasings
- Add plain language alongside technical terms
- Include synonyms and variations
- Test with different wordings

### 4. Don't Put Everything in SKILL.md
Keep under 500 lines using progressive disclosure pattern.

### 5. Don't Duplicate CLAUDE.md Content
- CLAUDE.md: project-wide conventions
- Skills: specialized procedures
- Skills reference CLAUDE.md, don't repeat it

### 6. Don't Make Skills Context-Unaware
Skills should adapt to project context, not force rigid patterns.

### 7. Don't Skip Testing
**Test checklist:**
- Discovery: invokes when expected, not for unrelated tasks
- Output: meets standards, handles edge cases
- Integration: works with other skills, no conflicts
- Performance: fast activation, under 500 lines

## When to Create Skills

**Create when:**
- Repeatable procedure
- Want consistent application
- Specialized capability
- Will use multiple times

**Don't create when:**
- One-off task (use slash command)
- Project-wide convention (use CLAUDE.md)
- Too simple to need automation
- Still figuring out approach

## Core Principle

Define workflow once → Claude applies automatically → Consistent results forever.
