# Context Window Efficiency Best Practices

## Overview

Context window efficiency is about maximizing the value you get from each token in your LLM's context window. For Claude, this typically means working within a 200,000 token limit (roughly 150,000 words or 600 pages). While this seems large, it fills up quickly with code, conversation history, and memory files.

## Why Context Efficiency Matters

### The Cost Equation

**Every token in context costs you:**
1. **Money:** Input tokens are charged (though less than output)
2. **Latency:** More context = slower processing
3. **Quality:** Excessive context can dilute focus
4. **Capacity:** Limited space for what actually matters

**Example breakdown (typical coding session):**
- **Conversation history:** 30K-50K tokens (grows over session)
- **Files in context:** 20K-100K tokens (depends on codebase)
- **Memory files:** 2K-10K tokens (CLAUDE.md, docs)
- **System prompts:** 5K-15K tokens (invisible but present)
- **Output buffer:** 10K-30K tokens (space for response)
- **Working buffer:** 20K+ tokens (recommended safety margin)

**Total used: 87K-225K tokens**

You can see how a 200K context window fills up quickly!

### The Quality Impact

**Research shows:**
- **Needle-in-haystack:** LLMs struggle to find specific information in very large contexts
- **Recency bias:** Recent information is weighted more heavily than earlier context
- **Dilution effect:** Important info gets lost in noise
- **Attention degradation:** Performance drops with extremely long contexts

**Evidence:**
Studies testing Claude's recall across context lengths show:
- 0-50K tokens: Near-perfect recall (98-99%)
- 50K-100K tokens: Excellent recall (95-98%)
- 100K-150K tokens: Good recall (90-95%)
- 150K-200K tokens: Declining recall (85-90%)

The exact numbers vary by task, but the pattern is clear: **shorter, more focused context is better.**

## Core Principles

### 1. **The Relevance Principle**

**Only include information that's directly relevant to the current task.**

✅ **Good:**
Working on authentication? Load:
- Auth-related source files
- Security documentation
- Auth architecture docs
- Recent auth-related decisions

❌ **Bad:**
Working on authentication? Don't load:
- UI component files
- Database migration history
- Unrelated feature documentation
- Old conversation about UI design

### 2. **The Summarization Principle**

**Summarize when you can, include full text only when you must.**

**Apply to:**
- Long conversation histories → "Summary: We implemented auth with JWT, solved CORS issues, tests pass"
- Previous work → "Last session: Completed payment flow, still need error handling"
- Background context → "This is an e-commerce site using React/Node"
- External docs → "Follow React best practices (see react.dev)"

**Full text needed for:**
- Code being actively modified
- Specific bugs being debugged
- Current task requirements
- Key decision references

### 3. **The Just-In-Time Principle**

**Load information when needed, not preemptively.**

**Pattern:**
1. Start with minimal context (task description + CLAUDE.md)
2. Claude identifies what it needs: "I need to see auth.ts to understand the current implementation"
3. Load only those specific files
4. Continue with focused context

**Anti-pattern:**
1. Load entire codebase
2. Load all documentation
3. Load all conversation history
4. Now you're at 180K tokens before even starting

### 4. **The Forgetting Principle**

**Deliberately forget what's no longer relevant.**

**When to start fresh:**
- Switching to a completely different task
- Previous task is complete
- Context is full of irrelevant history
- You've been in the same conversation for >50 exchanges

**What to preserve:**
- Summary of completed work
- Key decisions made
- Blockers encountered and resolved
- Updated memory files

**User insight:** "I used to try to keep one mega-conversation going. Now I start fresh conversations every few hours and just summarize what's needed. Way better results."

## ✅ DO: Efficiency Strategies

### 1. **Use File References Instead of Including Full Files**

**Pattern:**
Instead of loading entire files, reference them and load snippets as needed.

❌ **Inefficient:**
```
Human: Help me understand the codebase
Claude: [Reads all 50 files, 100K tokens]
```

✅ **Efficient:**
```
Human: Help me understand the authentication flow
Claude: I'll need to see the auth files. Can you share:
- src/auth/authService.ts
- src/middleware/authMiddleware.ts
Human: [Shares those 2 files, 3K tokens]
```

**Token savings: 97K tokens**

### 2. **Implement Strategic Chunking**

**Break large tasks into context-efficient chunks.**

**Example: Large refactoring**

❌ **Inefficient approach:**
"Refactor the entire authentication system" → Loads all auth files (50K tokens) → Makes changes → Context full

✅ **Efficient approach:**
1. "Review authentication architecture" → Load only arch docs (2K tokens)
2. "Refactor AuthService.ts" → Load only that file + tests (5K tokens)
3. "Refactor AuthMiddleware.ts" → Load only that file + tests (4K tokens)
4. "Update auth integration tests" → Load test files (6K tokens)

**Total tokens: 17K vs 50K+, with more focused attention**

### 3. **Use Diff-Based Updates**

**For modifications, show only what changed.**

❌ **Inefficient:**
```typescript
// Here's the entire 500-line file again with one function modified
[500 lines of code]
```

✅ **Efficient:**
```typescript
// In auth.ts, replace the validateToken function (lines 45-60):
- async validateToken(token: string): Promise<boolean> {
+ async validateToken(token: string): Promise<ValidationResult> {
    // ... changes here
  }
```

**Token savings: 90% fewer tokens for incremental changes**

### 4. **Leverage Code Summarization**

**Use comments and docstrings to convey information without full code.**

**Example:**
Instead of loading all implementation:
```typescript
// UserService.ts
// - getUser(id): Fetches user from DB with caching (Redis)
// - createUser(data): Creates user with validation and email verification
// - updateUser(id, data): Updates user, invalidates cache
// - deleteUser(id): Soft delete, maintains referential integrity
//
// Uses UserRepository for data access
// Implements retry logic for DB failures
// All methods are fully tested (see userService.test.ts)
```

**Then load full code only for the method you're modifying.**

**Token savings: 80-90% for background context**

### 5. **Implement Conversation Pruning**

**Strategically reset or prune conversation history.**

**Pattern 1 - Summary Reset:**
After completing a subtask:
```
Human: "Let me summarize our progress:
- Implemented JWT authentication ✓
- Solved CORS issues ✓
- Added refresh token logic ✓
- All tests passing ✓

Next task: Implement password reset flow. Let's start fresh with this summary as context."
```

**Pattern 2 - Selective Retention:**
```
Human: "We can forget the discussion about authentication (task complete).
Keep in mind:
- We're using PostgreSQL (from earlier)
- Error handling pattern is to throw AppError subclasses
- All APIs require rate limiting

Now, let's work on the password reset flow..."
```

**Pattern 3 - New Session with Handoff:**
At end of session, create handoff doc:
```markdown
# Session Handoff: 2024-01-15

## Completed
- JWT authentication implemented
- Tests passing

## Decisions
- Using jsonwebtoken library
- Tokens expire in 1 hour
- Refresh tokens in httpOnly cookies

## Next Steps
- Password reset flow
- Email verification

## Context Files
- src/auth/authService.ts (recently modified)
- src/middleware/authMiddleware.ts (recently modified)
```

Start new session: "Continue from this handoff document"

**Token savings: Massive. Fresh session with 500-token handoff vs 50K token conversation history**

### 6. **Use Schema Over Full Data**

**When discussing data structures, use TypeScript types instead of example data.**

❌ **Inefficient:**
```typescript
// Here are 50 example user objects:
const users = [
  { id: 1, email: "user1@example.com", name: "John", age: 30, ... },
  { id: 2, email: "user2@example.com", name: "Jane", age: 25, ... },
  // ... 48 more
]
```

✅ **Efficient:**
```typescript
// User type:
interface User {
  id: number;
  email: string;
  name: string;
  age: number;
  // ... other fields
}
```

**Token savings: 95%+ for large datasets**

### 7. **Implement Lazy Loading Patterns**

**Let Claude request what it needs rather than pre-loading everything.**

**Example workflow:**
```
Human: Help me fix the bug where users can't log in

Claude: To help debug this, I'll need to see:
1. The login endpoint implementation
2. Any recent changes to authentication
3. Error logs if available

Could you share those?