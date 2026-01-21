# Task List Management

Guidelines for managing task lists in markdown files to track progress on completing a PRD

## Task Implementation

### Interactive Task Selection

After completing (or before starting) work, use the **AskUserQuestion** tool to let the user decide what to do next. This provides flexibility for:
- **Non-linear execution**: The user may have already completed some sub-tasks manually
- **Batch work**: The user may want to complete multiple related sub-tasks in one session
- **Review and skip**: The user may want to review a sub-task before deciding

### Standard AskUserQuestion Pattern

Use this structure after each sub-task completion:

```
Question: What would you like to do next?
Options:
- "Continue with next sub-task" (Recommended) - Proceed to the next incomplete sub-task in order
- "Select specific sub-task" - Choose which sub-task to work on next
- "Run tests now" - Run the test suite before continuing
- "Commit and pause" - Stage and commit current changes, then pause
- "Mark parent complete" - Mark the parent task as complete (when all subtasks are done)
```

### Completion Protocol

1. **When you finish a sub-task:**
   - Immediately mark it as completed by changing `[ ]` to `[x]` in the task list file
   - Update the task list file with any new relevant files
   - Use AskUserQuestion to present next steps

2. **If all subtasks under a parent task are now `[x]`:**
   - **First**: Run the full test suite (`pytest`, `npm test`, `bin/rails test`, etc.)
   - **Only if all tests pass**: Stage changes (`git add .`)
   - **Clean up**: Remove any temporary files and temporary code before committing
   - **Commit**: Use a descriptive commit message that:
     - Uses conventional commit format (`feat:`, `fix:`, `refactor:`, etc.)
     - Summarizes what was accomplished in the parent task
     - Lists key changes and additions
     - References the task number and PRD context
     - **Formats the message as a single-line command using `-m` flags**, e.g.:

       ```
       git commit -m "feat: add payment validation logic" -m "- Validates card type and expiry" -m "- Adds unit tests for edge cases" -m "Related to T123 in PRD"
       ```
   - Mark the **parent task** as completed `[x]`

3. **When user selects "Select specific sub-task":**
   - Use AskUserQuestion with `multiSelect: false`
   - Present all incomplete sub-tasks as options
   - Include their descriptions and task numbers for context

## Task List Maintenance

1. **Update the task list as you work:**
   - Mark tasks and subtasks as completed (`[x]`) per the protocol above.
   - Add new tasks as they emerge.

2. **Maintain the "Relevant Files" section:**
   - List every file created or modified.
   - Give each file a one‑line description of its purpose.

# AI Instructions

## When working with task lists, the AI must:

1. **Before starting work:**
   - Read the task list file
   - Identify incomplete sub-tasks
   - Use AskUserQuestion to confirm where to start (or continue)

2. **After implementing a sub-task:**
   - Update the task list file immediately
   - Mark the sub-task `[x]`
   - Use AskUserQuestion to present next-step options

3. **Follow the completion protocol:**
   - Mark each finished **sub‑task** `[x]`.
   - Mark the **parent task** `[x]` once **all** its subtasks are `[x]`.

4. **Add newly discovered tasks** as they emerge.

5. **Keep "Relevant Files" accurate and up to date.**

## 🏗️ ENGINEERING STANDARDS

**Code Quality Requirements:**
1. **No Hardcoded Values**
   - ❌ NEVER hardcode IPs, ports, timeouts
   - ✅ Use configuration files, environment variables, or constants

2. **Externalized Text**
   - ❌ NEVER hardcode user-facing messages in code
   - ✅ Use message catalogs or constants files
   - ✅ Support internationalization (i18n) ready structure

3. **Consistent Response Format**
   - ✅ Always use success/error response wrappers
   - ✅ Standardized error codes and messages
   - ✅ Consistent API response structure

4. **Comprehensive Testing**
   - ✅ Unit tests for ALL business logic (minimum 80% coverage)
   - ✅ Integration tests for API endpoints
   - ✅ Performance tests for critical paths
   - ✅ Security tests for authentication/authorization

5. **Production-Ready Code**
   - ✅ Proper error handling and recovery
   - ✅ Structured logging with correlation IDs
   - ✅ Metrics and monitoring hooks
   - ✅ Graceful degradation patterns
   - ✅ Circuit breakers for external services


**Implementation Checklist:**
- [ ] Remove ALL hardcoded values
- [ ] Create configuration classes/files
- [ ] Implement response wrappers
- [ ] Add comprehensive unit tests
- [ ] Add integration tests
- [ ] Externalize all messages
- [ ] Add proper logging
- [ ] Implement error handling
- [ ] Add performance metrics
