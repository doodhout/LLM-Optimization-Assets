# Tools and MCP Best Practices - Executive Summary

## Core Concepts

**Tools:** Predefined functions Claude invokes (file operations, bash commands, web search, code execution).

**MCP (Model Context Protocol):** Standardized protocol for connecting Claude to external data sources via server-based architecture. More flexible than built-in tools.

## Critical Best Practices

### 1. Tool Selection
- Use file tools for code access, grep for codebase search, bash for tests, MCP for complex integrations
- Choose MCP over tools for: stateful operations, authentication-required APIs, complex business logic
- Avoid building MCP servers for simple one-off API calls

### 2. MCP Server Design
- Single responsibility principle: one focused purpose per server
- Implement pagination and result limits (prevent context overflow)
- Return structured JSON, not formatted text
- Provide detailed tool descriptions with use cases and examples

### 3. Security Essentials
- **Audit first:** Review source code, dependencies, permissions before use
- **Least privilege:** Grant minimum necessary permissions
- **Sandbox:** Run MCP servers in isolated environments (Docker, virtual environments)
- **Secrets:** Use environment variables, never hardcode in config files
- **Monitor:** Log all tool usage with parameters, timestamps, and user info

### 4. Performance Optimization
- Minimize round trips: batch operations when possible
- Implement multi-level caching (in-memory + persistent)
- Return only necessary data fields
- Handle rate limits proactively with retry logic

### 5. Error Handling
- Return detailed errors with: error code, clear message, debugging details, remediation suggestions
- Enable Claude to understand and communicate issues to users

## Critical Anti-Patterns

**DON'T:**
- Grant excessive filesystem or network permissions
- Trust MCP servers blindly - review code before installation
- Store secrets in MCP configuration files
- Return massive datasets without pagination
- Ignore API rate limits and quotas
- Build complex MCP servers for simple tasks

## Security Threat Model

**Key risks:** Data exfiltration, unauthorized access, code execution, DoS, supply chain attacks

**Red flags in MCP servers:** Unknown network requests, file access outside project directory, arbitrary command execution, closed-source code, excessive environment variables

**Mitigation:** Keep data local when possible, use Docker with `--network=none`, implement audit logging, apply OS-level sandboxing

## Essential Tools and Servers

**Top 3 Built-in Tools:**
1. File read/write - Direct codebase interaction
2. Bash execution - Run tests and verify changes
3. Web search - Access current information

**Top 3 MCP Servers:**
1. @modelcontextprotocol/server-github - GitHub API integration
2. @modelcontextprotocol/server-memory - Persistent memory across sessions
3. @modelcontextprotocol/server-postgres - Database access

## Common Usage Patterns

1. **Read-Analyze-Modify:** Read code → analyze → write changes
2. **Search-Focus-Act:** Search codebase → read relevant files → perform task
3. **Test-Fix-Verify:** Run tests → fix failures → verify with tests
4. **Research-Implement-Validate:** Web search → implement → test

## Productivity Enhancements

- **Voice-to-text** (SuperWhisper): Reduce typing fatigue for long sessions
- **Automation tools** (BetterTouchTool): Custom hotkeys and clipboard transformations for IDE-to-CLI workflows
- **Utility scripts:** Automate test data generation, auth testing, database operations
- **CLI tools:** PM2, jq, httpie, fd, ripgrep for enhanced workflows

## Success Metrics

**Tool call success rate:** >95% good, <85% poor
**Efficiency:** High task completion with minimal tool calls
**Error recovery:** >90% errors handled gracefully

## Golden Rule

Tools and MCP should enhance capabilities while maintaining security and performance. Start with built-in tools, add official MCP servers as needed, build custom MCP only for clear recurring needs. If a tool adds complexity or slows workflow, reconsider its necessity.
