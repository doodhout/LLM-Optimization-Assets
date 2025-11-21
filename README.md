# LLM Optimization Assets

**A comprehensive, evidence-based guide to optimizing your use of Claude and other Large Language Models for software development.**

## Overview

This repository contains battle-tested best practices, workflows, and strategies for maximizing productivity with LLM-assisted development. All recommendations are backed by research, community insights, and real-world usage patterns.

## What's Inside

### Core Guides

#### 📄 [CLAUDE.md Best Practices](guides/claude-md/claude-md-best-practices.md)
Everything you need to know about creating effective CLAUDE.md files (also applicable to .clinerules, .cursorrules, and similar files):
- What to include (and why)
- What to avoid (and why)
- Optimal file structure and size
- Evidence-based recommendations
- Token efficiency strategies

**Key insight:** Keep it project-specific (300-500 tokens), not a copy of framework documentation.

#### 🤖 [Agent Definition Best Practices](guides/agents/agent-definition-best-practices.md)
How to create specialized AI agents for specific tasks:
- Single-responsibility principle for agents
- Effective role definitions
- Input/output specifications
- Examples and counter-examples
- Quality standards and constraints

**Key insight:** One focused agent beats one generalist every time.

#### 🧠 [Memory Management](guides/memory/memory-management-best-practices.md)
Strategies for persistent knowledge across sessions:
- Types of memory systems (built-in, manual, MCP)
- When to use each approach
- Memory hygiene practices
- Organization patterns
- Version control for memory

**Key insight:** Memory should be selective, not comprehensive. Store what's unique to your project.

#### ⚡ [Context Window Efficiency](guides/context-efficiency/context-window-efficiency.md)
Maximize value from every token in your context window:
- Core efficiency principles
- Context budgeting strategies
- What to load (and what not to)
- Measuring efficiency
- Context-efficient workflows

**Key insight:** 20K tokens of relevant context beats 150K tokens of mixed content every time.

**Continuation:** [Part 2](guides/context-efficiency/context-window-efficiency-continued.md)

#### 🔧 [Tools and MCP Best Practices](guides/tools-mcp/tools-and-mcp-best-practices.md)
Effective and secure use of tools and Model Context Protocol:
- When to use tools vs MCP
- Security considerations and threats
- Game-changing tools and servers
- Building secure MCP servers
- Performance optimization

**Key insight:** Audit MCP servers before use. Keep data local when possible.

#### 🎯 [Model Selection](guides/model-selection/model-choice-best-practices.md)
Choosing the right model for each task:
- Understanding Opus vs Sonnet vs Haiku
- Decision frameworks
- Cost-benefit analysis
- Model cascading strategies
- Task-specific recommendations

**Key insight:** Most tasks don't need the most powerful model. Match capability to complexity.

#### 🔄 [Effective Workflows](guides/workflows/effective-workflows.md)
Battle-tested development workflows:
- The plan-implement-test-review cycle
- Breaking tasks into manageable chunks
- Continuous testing patterns
- When to start fresh conversations
- Complete workflow examples

**Key insight:** Small, iterative changes with verification beats large implementations every time.

#### ⚠️ [Common Mistakes](guides/advanced-topics/common-mistakes-and-how-to-avoid-them.md)
Learn from others' mistakes:
- The "vibe coding" trap
- Letting AI write too much at once
- Context management failures
- Security oversights
- Technical debt accumulation

**Key insight:** The 200-line rule - never let Claude write more than 200 lines without testing.

## Quick Start

### For Beginners

1. **Start here:** [CLAUDE.md Best Practices](guides/claude-md/claude-md-best-practices.md)
   - Create a basic CLAUDE.md for your project
   - Keep it under 500 tokens
   - Include tech stack, architecture basics, and code style

2. **Then read:** [Effective Workflows](guides/workflows/effective-workflows.md)
   - Learn the core cycle: Plan → Implement → Test → Review
   - Practice with small tasks (15-45 minute cycles)
   - Test after every change

3. **Understand costs:** [Model Selection](guides/model-selection/model-choice-best-practices.md)
   - Use Haiku for simple tasks
   - Use Sonnet 4.5 for most work
   - Reserve Opus 4 for complex problems

4. **Avoid pitfalls:** [Common Mistakes](guides/advanced-topics/common-mistakes-and-how-to-avoid-them.md)
   - Don't skip planning
   - Don't accept code without understanding it
   - Don't let conversations grow too long

### For Intermediate Users

1. **Optimize context:** [Context Window Efficiency](guides/context-efficiency/context-window-efficiency.md)
   - Implement the relevance principle
   - Use references instead of copying
   - Start fresh conversations for new tasks

2. **Manage memory:** [Memory Management](guides/memory/memory-management-best-practices.md)
   - Set up a memory structure for your project
   - Implement memory hygiene practices
   - Version control your knowledge base

3. **Create agents:** [Agent Definition Best Practices](guides/agents/agent-definition-best-practices.md)
   - Build specialized agents for code review, testing, etc.
   - Use single-responsibility principle
   - Provide examples and quality standards

### For Advanced Users

1. **Master tools:** [Tools and MCP Best Practices](guides/tools-mcp/tools-and-mcp-best-practices.md)
   - Build custom MCP servers for your workflow
   - Implement proper security controls
   - Optimize tool performance

2. **Scale up:** Advanced workflow patterns
   - Multi-model workflows
   - Context partitioning
   - Team collaboration patterns

3. **Contribute:** Share your learnings
   - Document successful patterns
   - Report anti-patterns
   - Help improve these guides

## Key Principles

These principles are consistent across all guides:

### 1. **Understand, Don't Just Use**
Read and understand every line of AI-generated code. You're responsible for quality.

### 2. **Small and Iterative**
Break tasks into 15-45 minute cycles. Test after every change. Commit frequently.

### 3. **Plan Before Implementing**
Spend 10-15 minutes planning. It saves hours of rework.

### 4. **Context is Precious**
Only load what's relevant. Summarize what's not needed. Start fresh for new tasks.

### 5. **Security First**
Audit MCP servers. Never commit secrets. Use least privilege. Keep data local when sensitive.

### 6. **Match Tool to Task**
Use Haiku for simple tasks, Sonnet for most work, Opus for complex problems.

### 7. **Review Everything**
AI makes mistakes. Review, test, and verify all generated code.

### 8. **Document Decisions**
Capture why decisions were made. Future you (and your team) will thank you.

## Evidence and Sources

This guide synthesizes:

- **Official documentation** from Anthropic, OpenAI, and other LLM providers
- **Research papers** on prompt engineering, context management, and AI-assisted development
- **Community insights** from r/ClaudeAI, r/ClaudeCode, and other forums
- **Real-world usage** patterns from developers using LLMs daily
- **Security research** on MCP and AI tooling
- **Industry reports** on AI-assisted development and technical debt

All claims are supported by evidence where possible. References are included in individual guides.

## Repository Structure

```
LLM-Optimization-Assets/
├── README.md                          # You are here
├── guides/
│   ├── claude-md/                     # CLAUDE.md best practices
│   │   └── claude-md-best-practices.md
│   ├── agents/                        # Agent definition patterns
│   │   └── agent-definition-best-practices.md
│   ├── memory/                        # Memory management strategies
│   │   └── memory-management-best-practices.md
│   ├── context-efficiency/            # Context window optimization
│   │   ├── context-window-efficiency.md
│   │   └── context-window-efficiency-continued.md
│   ├── tools-mcp/                     # Tools and MCP guides
│   │   └── tools-and-mcp-best-practices.md
│   ├── model-selection/               # Choosing the right model
│   │   └── model-choice-best-practices.md
│   ├── workflows/                     # Effective workflows
│   │   └── effective-workflows.md
│   └── advanced-topics/               # Advanced guides
│       └── common-mistakes-and-how-to-avoid-them.md
```

## Quick Reference: Essential Do's and Don'ts

### ✅ DO:
- ✅ Read and understand all generated code
- ✅ Test after every change
- ✅ Break tasks into 15-45 minute cycles
- ✅ Plan before implementing
- ✅ Start fresh conversations for new tasks
- ✅ Review all AI-generated code
- ✅ Keep CLAUDE.md under 500 tokens
- ✅ Use appropriate model for task complexity
- ✅ Audit MCP servers before using
- ✅ Document significant decisions

### ❌ DON'T:
- ❌ Let AI write more than 200 lines without testing
- ❌ Skip the planning phase
- ❌ Accept code without understanding it
- ❌ Work in one mega-conversation for days
- ❌ Load entire codebases into context
- ❌ Store secrets in config files
- ❌ Use MCP servers without auditing
- ❌ Always use the most powerful model
- ❌ Accumulate technical debt
- ❌ Treat AI as a replacement for thinking

## The Golden Rules

### The 200-Line Rule
> **"Never let Claude write more than 200 lines without running and testing the code."**

This single rule prevents the majority of problems. Break large tasks into smaller ones.

### The Context Budget Rule
> **"If Claude doesn't need it for the current task, don't include it."**

Every token should earn its place by being directly relevant.

### The Security Rule
> **"Audit before trusting. Least privilege always."**

For MCP servers and tools, security first.

## Recommended Reading Order

### Path 1: Fast Start (2-3 hours)
1. [CLAUDE.md Best Practices](guides/claude-md/claude-md-best-practices.md) (30 min)
2. [Effective Workflows](guides/workflows/effective-workflows.md) (45 min)
3. [Model Selection](guides/model-selection/model-choice-best-practices.md) (30 min)
4. [Common Mistakes](guides/advanced-topics/common-mistakes-and-how-to-avoid-them.md) (45 min)

**Outcome:** Solid foundation, ready to work efficiently.

### Path 2: Comprehensive (6-8 hours)
Read all guides in order listed in repository structure.

**Outcome:** Deep understanding, ready to optimize complex workflows.

### Path 3: Problem-Solving (as needed)
Jump directly to the guide relevant to your current challenge.

**Outcome:** Targeted solutions to specific issues.

## FAQ

### Q: Which guide should I read first?
**A:** Start with [CLAUDE.md Best Practices](guides/claude-md/claude-md-best-practices.md) and [Effective Workflows](guides/workflows/effective-workflows.md). These provide the foundation.

### Q: Do these practices apply to other LLMs besides Claude?
**A:** Most principles apply broadly (workflows, context management, security). Some specifics are Claude-focused (CLAUDE.md, model selection), but analogous features exist in other LLMs.

### Q: How often should I review these guides?
**A:** Review when:
- Starting a new project
- Facing performance issues
- Your workflow feels inefficient
- LLM behavior seems suboptimal
- Every few months to catch updates

### Q: Can I use these in commercial projects?
**A:** Yes. These are best practices and patterns, not code. Apply them however helps your project.

### Q: What if I disagree with a recommendation?
**A:** That's fine! These are guidelines based on evidence and consensus, not rules. Adapt to your context. If you find a better approach with evidence, please share it.

## Contributing

Found a mistake? Have a better approach? Want to share your experience?

**Improvements welcome:**
- Open issues for discussions
- Submit pull requests for corrections
- Share your success stories and lessons learned

**Guidelines for contributions:**
- Back claims with evidence (research, data, community consensus)
- Be specific and actionable
- Include examples where helpful
- Maintain the practical, evidence-based tone

## Final Thoughts

**AI-assisted development is powerful but requires discipline.**

The developers who succeed are those who:
- Understand what the AI generates
- Work in small, verified iterations
- Plan before implementing
- Review critically
- Maintain quality standards
- Never stop learning

**Use these guides as your foundation. Adapt to your context. Share what you learn.**

**Happy coding with Claude! 🚀**

---

**Last Updated:** January 2025
**Version:** 1.0
**Status:** Living document (check for updates)
