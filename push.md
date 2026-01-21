# Rule: Pushing Git Changes

## Goal

To guide an AI assistant in safely pushing commits to a remote repository with proper validation and user confirmation.

## Process

1. **Gather Context:** Run git commands to understand the current state:
   - `git status` - Verify working tree is clean (no uncommitted changes)
   - `git log -1 --oneline` - Show the commit that will be pushed
   - `git branch --show-current` - Show current branch name
   - `git log @{u}..HEAD --oneline` - Show commits that will be pushed (ahead of remote)

2. **Validate Pre-push Conditions:**
   - Ensure working tree is clean (no uncommitted changes)
   - Check if there are commits to push
   - Identify the remote branch that will receive the push

3. **Present to User:** Use AskUserQuestion to confirm push:
   ```
   Question: Ready to push?
   Options:
   - "Push to origin" - Push commits to origin/<branch>
   - "Set upstream first" - Set upstream branch then push (for first push)
   - "Create PR instead" - Create a pull request instead of pushing
   - "Cancel" - Abort the push
   ```

4. **Execute Push:** Based on user selection:
   - **Standard push**: `git push`
   - **First-time push**: `git push -u origin <branch>`
   - **Create PR**: Use `gh pr create` with appropriate flags

5. **Verify:** Run `git status` after pushing to confirm success.

## Safety Checks

- **NEVER** push with `--force` to main/master
- **WARN** before pushing to main/master directly (recommend PR workflow)
- **VERIFY** the working tree is clean before pushing
- **CHECK** for sensitive files that may have been accidentally committed

## Creating Pull Requests

When user selects "Create PR instead":

1. Run `git diff main...HEAD --stat` to see what will be in the PR
2. Check if there's already a PR for this branch using `gh pr list --head <branch>`
3. Draft PR summary based on commits:
   - Run `git log main..HEAD --oneline` to get commit history
   - Summarize the key changes
   - Generate a test plan checklist
4. Use AskUserQuestion to confirm PR creation:
   ```
   Question: Create PR?
   Options:
   - "Create PR" - Create the PR with the drafted summary
   - "Edit summary" - Modify the PR description before creating
   - "Cancel" - Abort PR creation
   ```
5. Create PR using `gh pr create` with proper formatting

## Example Flow

```
AI: [Checks git status, log, and branch]

You are on branch 'feature/auth' and ahead of origin by 3 commits:
  - feat: add OAuth2 login flow
  - feat: add JWT token management
  - feat: add user session middleware

Working tree is clean. Ready to push to origin/feature/auth.

[Uses AskUserQuestion]
┌─────────────────────────────────────────┐
│ Ready to push?                          │
├─────────────────────────────────────────┤
│ ○ Push to origin                        │
│ ○ Set upstream first                    │
│ ○ Create PR instead                     │
│ ○ Cancel                                │
└─────────────────────────────────────────┘

User: "Create PR instead"

AI: [Drafts PR summary from commits and presents for confirmation]
```

## Main/Master Protection

If attempting to push to main or master branch:
1. **WARN** the user with a clear message
2. **RECOMMEND** creating a PR instead for code review
3. **REQUIRE** explicit confirmation if user insists on direct push
