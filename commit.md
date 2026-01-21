# Rule: Creating a Git Commit

## Goal

To guide an AI assistant in creating a well-formed, conventional commit message that follows the repository's style and accurately describes the changes being committed.

## Process

1. **Gather Context:** Run git commands in parallel to understand the repository state:
   - `git status` - See all untracked files and working tree state
   - `git diff` - See both staged and unstaged changes
   - `git log -10 --oneline` - See recent commit messages for style reference

2. **Analyze Changes:** Review the output from the git commands:
   - Identify which files have been added, modified, or deleted
   - Understand the nature of the changes from the diff output
   - Match the commit message style to the repository's conventions

3. **Draft Commit Message:** Based on the analysis, draft a commit message that:
   - Uses conventional commit format (`feat:`, `fix:`, `refactor:`, `docs:`, `test:`, `chore:`, etc.)
   - Summarizes the changes in the subject line (50 chars or less preferred)
   - Includes body paragraphs that explain:
     - What was changed and why
     - Key implementation details if relevant
     - Any breaking changes or migration notes
   - References related issues, PRDs, or task numbers if applicable

4. **Present to User:** Show the drafted commit message and ask for approval or modifications using AskUserQuestion:
   ```
   Question: Ready to commit?
   Options:
   - "Commit as written" - Create the commit with the drafted message
   - "Edit message" - Modify the commit message before committing
   - "Cancel" - Abort the commit
   ```

5. **Execute Commit:** If approved, run:
   - `git add .` to stage all changes
   - `git commit` with the approved message using `-m` flags for multi-line messages

6. **Verify:** Run `git status` after committing to confirm success.

## Commit Message Format

Use multi-line `-m` flags for proper formatting:

```bash
git commit -m "feat: add user authentication" \
  -m "- Implement OAuth2 login flow with Google provider" \
  -m "- Add JWT token management and refresh logic" \
  -m "- Create user session middleware for protected routes" \
  -m "Related to PRD-auth-flow"
```

## Conventional Commit Types

| Type | When to Use |
|------|-------------|
| `feat:` | New feature or functionality |
| `fix:` | Bug fix or defect resolution |
| `refactor:` | Code restructuring without behavior change |
| `docs:` | Documentation changes only |
| `test:` | Adding or updating tests |
| `chore:` | Maintenance tasks, dependencies, config |
| `perf:` | Performance improvements |
| `style:` | Code style changes (formatting, naming) |
| `build:` | Build system or dependency changes |
| `ci:` | CI/CD configuration changes |

## Best Practices

1. **Be specific:** "Fix bug" is bad; "Fix null pointer in user service" is good
2. **Focus on why, not what:** The diff shows what changed; explain why
3. **Keep it atomic:** One logical change per commit
4. **Use imperative mood:** "Add feature" not "Added feature" or "Adding feature"
5. **Reference context:** Link to PRDs, task numbers, or issue tickets

## Safety Checks

- **NEVER** commit files that likely contain secrets (`.env`, `credentials.json`, etc.) - warn the user first
- **NEVER** use `--no-verify` to skip hooks unless explicitly requested
- **NEVER** force push to main/master
- **NEVER** amend existing commits unless explicitly requested
- **ALWAYS** create NEW commits

## Example Flow

```
AI: [Analyzes git status, diff, and recent commits]

Based on the changes, here's the drafted commit message:

```
feat: add project label filtering to dashboard

- Add label filter dropdown to teamlead dashboard
- Implement server-side filtering in dashboard service
- Update UI to show selected filters and clear option
- Add unit tests for filter logic

Related to tasks-teamlead-employee-overview-dashboards.md
```

[Uses AskUserQuestion]
┌─────────────────────────────────────────┐
│ Ready to commit?                         │
├─────────────────────────────────────────┤
│ ○ Commit as written                     │
│ ○ Edit message                          │
│ ○ Cancel                                │
└─────────────────────────────────────────┘
```
