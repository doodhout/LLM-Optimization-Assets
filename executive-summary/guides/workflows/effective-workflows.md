# Executive Summary: Effective Workflows for LLM-Assisted Development

## Core Principle

**Small, iterative changes with verification beat large, monolithic implementations.**
- Large implementations become unwieldy, hard to debug, and degrade in quality
- Developers using small iterations are 3-5x more productive

## The Plan-Implement-Test-Review Cycle

**Each cycle: 15-45 minutes**
1. Plan - Define small, focused task
2. Implement - Write minimal code
3. Test - Verify it works
4. Review - Check quality
5. Commit - Save progress

**Benefits:** Constant verification, manageable chunks, easy debugging, meaningful git history, quick rollback capability

**Good cycle sizes:** Single function, one component with tests, one bug fix, one API endpoint, small refactoring
**Too large:** Entire features, major refactorings, multiple features, anything >1 hour

## Essential Practices

### 1. Start with Planning
- Break tasks into subtasks
- Identify dependencies and risks
- Plan implementation order
- 10-15 minute investment saves hours of rework

### 2. Implement One Component at a Time
- Work on one file/module/component per cycle
- Implement tests immediately
- Verify before moving on
- Commit before next component

### 3. Test After Every Change
- Never accumulate untested code
- Testing continuously is 15x faster for debugging than testing at end
- Fix failures immediately when you know what broke

### 4. Review Before Committing
Verify: Tests pass, code is clean, no debug code, error handling exists, edge cases handled, follows conventions

### 5. Commit Frequently
- After each cycle (every 15-45 minutes)
- Clear commit messages with type and description
- Easy rollback, clear history, safe experimentation

### 6. Branch-Per-Task
- Keep main stable
- Can abandon work if needed
- Enables code review before merge

### 7. Take Breaks
- LLM-assisted development is cognitively intense
- Review quality degrades when tired
- Break after each cycle (5 min)
- Longer break after 4-5 cycles (30+ min)

## Critical Anti-Patterns

### 1. The 200-Line Rule
**Never let Claude write >200 lines without testing.**
- Bugs compound across files
- Hard to debug
- Often faster to restart than fix

### 2. Don't Skip Planning
- No shared understanding leads to wasted effort
- 10-15 minutes planning saves hours

### 3. Don't Accumulate Technical Debt
**Rule: Fix it now if it takes <5 minutes.**
- TODOs never get done
- Debt compounds

### 4. Trust But Verify
- Always test before committing
- LLMs make mistakes

### 5. Don't Use Mega-Conversations
**Start fresh after 50 exchanges or 3-4 hours.**
- Context fills with irrelevant history
- Model confuses features
- Performance degrades

### 6. Don't Skip Code Review
- Review should take 20-30% of implementation time
- Catches bugs before they become technical debt

## Dev Docs System for Complex Tasks

**Problem:** Claude loses track during long tasks, forgets original plan after context loss

**Solution:** Create persistent documentation:
```
dev/active/[task-name]/
  ├── [task-name]-plan.md     # Accepted plan with phases
  ├── [task-name]-context.md  # Key files, decisions, integrations
  └── [task-name]-tasks.md    # Checklist of work items
```

**Workflow:**
1. Create dev docs structure at task start
2. Generate three core documents (plan, context, tasks)
3. Reference docs frequently during implementation
4. Update tasks immediately as completed
5. Update context/decisions as they emerge
6. Update dev docs before context runs low

**Resume after context loss:** Point Claude to dev docs files to continue seamlessly

**Benefits:** Clear plan always available, easy resume, tangents get caught, all decisions documented, smooth handoffs

## PM2 for Backend Debugging

**Problem:** Claude can't see logs while services run, manual copy-paste is tedious

**Solution:** PM2 process manager
- Services run as managed processes with log files
- Claude can read logs autonomously
- Automatic restarts on crashes
- Real-time monitoring

**Commands:** `pm2 logs`, `pm2 restart`, `pm2 status`

**Benefit:** Autonomous debugging without manual log management

## Key Metrics

- 3-5x productivity with small iterations vs large implementations
- 15x faster debugging with continuous testing
- 15-45 minutes per cycle
- 200-line maximum before testing
- 50 exchanges or 3-4 hours maximum per conversation
- 20-30% of time spent on code review
