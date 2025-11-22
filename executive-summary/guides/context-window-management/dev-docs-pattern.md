# Executive Summary: Dev Docs Pattern for Complex Tasks

## The Problem
Claude loses track during long tasks due to context accumulation, auto-compaction, and inherent context window limitations. Original plans get buried, causing drift and wasted time re-explaining requirements after context resets.

## The Solution: Three-File System
Create persistent documentation in `dev/active/[task-name]/`:

**1. [task-name]-plan.md**
- High-level strategy document that rarely changes
- Contains executive summary, implementation phases, risks, success metrics, and timeline
- Provides stable direction without requiring frequent updates

**2. [task-name]-context.md**
- Living state document updated frequently
- Includes session progress, completed work, current status, immediate next steps, blockers
- Documents key files, architectural decisions with reasoning, technical constraints
- Contains quick resume instructions for seamless continuation

**3. [task-name]-tasks.md**
- Granular action checklist with status indicators
- Each task includes acceptance criteria and dependencies
- Mark tasks complete immediately as work progresses

## Workflow
1. Enter planning mode for thorough analysis
2. Review plan completely before implementation
3. Create dev docs structure and generate three core documents
4. Reference docs throughout implementation
5. Update tasks and context immediately as work progresses
6. Before context runs low, update all docs with current state and next steps

## Continuing After Context Loss
Simply instruct Claude to read the three dev doc files and pick up where work left off. All necessary context persists across sessions.

## Critical Insight: Context Windows Are the Hidden Bottleneck
Context window limits are the single most disruptive factor in LLM-assisted development. Studies show developers lose 30-60 minutes per session when context resets without proper management. "Context rot" occurs as token count increases, reducing accurate recall even within the window. The "lost-in-the-middle effect" causes LLMs to undervalue important context between the beginning and end of prompts.

## Benefits
- Clear plan always available
- Easy resume after breaks or compaction
- All decisions documented with reasoning
- Smooth handoffs between conversations
- Tangents caught early by comparing to plan
- Eliminates hours wasted re-explaining requirements

## When to Use Dev Docs
**Use for:** Tasks over 2 hours, work spanning multiple sessions, complex features, large refactoring, multi-day projects

**Skip for:** Simple bug fixes under 30 minutes, single-file changes, quick updates, exploratory work without clear scope

**The 2-hour rule:** If a task might exceed 2 hours or span sessions, create dev docs. Time invested upfront saves hours later.
