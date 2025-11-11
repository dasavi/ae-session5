---
description: Analyze changes, generate commit message, and push to feature branch
tools:
  - runCommands
  - getTerminalOutput
---

# Commit and Push Changes

You are committing and pushing changes to a feature branch. This prompt works in any mode context and relies on Git workflow knowledge from `.github/copilot-instructions.md`.

## Usage

```
/commit-and-push <branch-name>
```

**Parameters**:
- `branch-name` (REQUIRED): The feature branch name (e.g., 'feature/implement-delete', 'fix/toggle-bug')

**Examples**:
```
/commit-and-push feature/implement-delete
/commit-and-push fix/toggle-bug
/commit-and-push feature/edit-todo
```

## Context from Project Instructions

This prompt inherits knowledge from `.github/copilot-instructions.md`, specifically:

- **Conventional Commits**: Semantic commit message format (feat:, fix:, test:, refactor:, chore:, docs:, style:)
- **Branch Strategy**: Feature branches (`feature/<name>`), bug fixes (`fix/<name>`), main branch (protected)

## Input Validation

**REQUIRED**: `branch-name` parameter

If the branch name is not provided, **STOP and ask the user**:

```
I need a branch name to commit and push changes.

Examples:
- feature/implement-delete
- fix/toggle-bug
- feature/edit-todo
- fix/stats-display

Please provide the branch name:
```

Do NOT proceed without a valid branch name.

## Instructions

### Step 1: Analyze Changes

Review what has changed in the workspace:

```bash
# Show all changes
git status

# Show detailed diff
git diff

# If there are staged changes, show those too
git diff --staged
```

Analyze:
- What files were modified?
- What was added, removed, or changed?
- What feature or fix do these changes represent?
- Are the changes cohesive (related to one task)?

### Step 2: Generate Conventional Commit Message

Based on the changes, create a commit message following this format:

```
<type>: <description>

[optional body]
```

**Types**:
- `feat:` - New features (e.g., "feat: add delete button to todo items")
- `fix:` - Bug fixes (e.g., "fix: resolve todo duplication bug")
- `test:` - Test additions or modifications (e.g., "test: add integration tests for todo API")
- `refactor:` - Code refactoring without behavior changes (e.g., "refactor: extract todo validation logic")
- `chore:` - Maintenance tasks (e.g., "chore: update dependencies")
- `docs:` - Documentation changes (e.g., "docs: update API documentation")
- `style:` - Code style/formatting changes (e.g., "style: fix ESLint warnings")

**Guidelines**:
- Use lowercase for the description
- Be specific and descriptive
- Describe WHAT changed, not HOW
- Keep the first line under 72 characters
- Add body for complex changes (optional)

**Examples**:
```bash
feat: implement DELETE endpoint for todos
fix: toggle now correctly switches between true and false
test: add comprehensive tests for POST /api/todos
refactor: extract todo service into separate module
chore: remove unused dependencies
```

### Step 3: Create or Switch to Branch

**IMPORTANT**: Only commit to the user-provided feature branch, NEVER to main.

```bash
# Check current branch
git branch --show-current

# If branch doesn't exist, create it
git checkout -b <branch-name>

# If branch exists, switch to it
git checkout <branch-name>
```

Verify you're on the correct branch:
```bash
git branch --show-current
```

### Step 4: Stage All Changes

```bash
# Stage all changes
git add .

# Verify what will be committed
git status
```

### Step 5: Commit with Generated Message

```bash
# Commit with the conventional commit message
git commit -m "<type>: <description>"
```

If the commit has a body:
```bash
git commit -m "<type>: <description>" -m "<body paragraph 1>" -m "<body paragraph 2>"
```

### Step 6: Push to Feature Branch

```bash
# Push to the feature branch
git push origin <branch-name>

# If it's a new branch, you may need to set upstream
git push --set-upstream origin <branch-name>
```

### Step 7: Confirm and Report

After successful push, report to the user:

```
✅ Changes committed and pushed successfully!

Branch: <branch-name>
Commit: <commit-message>

Files changed:
- <list of modified files>

Next steps:
- Create a pull request on GitHub to merge into main
- Or continue working on this branch with additional changes
```

## Error Handling

### If there are no changes to commit:
```
⚠️ No changes detected to commit.

Run `git status` to verify workspace state.
```

### If branch name is invalid:
```
❌ Invalid branch name: "<branch-name>"

Branch names should follow this format:
- feature/<descriptive-name>
- fix/<descriptive-name>

Examples:
- feature/implement-delete
- fix/toggle-bug
```

### If trying to commit to main:
```
❌ Cannot commit directly to main branch!

The main branch is protected. Please provide a feature branch name:
- feature/<name> for new features
- fix/<name> for bug fixes
```

### If push fails:
```
❌ Push failed. Common causes:
- Remote branch has changes you don't have locally (run: git pull origin <branch-name>)
- Authentication issues (check GitHub credentials)
- Network connectivity

Error details:
<error message>
```

## Workflow Example

```bash
# 1. Analyze changes
git status
git diff

# 2. Determine commit type and message
# Changes: Implemented DELETE endpoint
# Type: feat
# Message: "feat: implement DELETE endpoint for todos"

# 3. Switch to feature branch
git checkout -b feature/implement-delete

# 4. Stage changes
git add .

# 5. Commit
git commit -m "feat: implement DELETE endpoint for todos"

# 6. Push
git push origin feature/implement-delete

# 7. Confirm success
echo "✅ Pushed to feature/implement-delete"
```

## Key Principles

- ✅ **Descriptive Commits**: Clear, semantic messages
- ✅ **Feature Branches**: Always use feature/fix branches
- ✅ **Verify First**: Check status and diff before committing
- ✅ **Atomic Commits**: One logical change per commit
- ❌ **Never Commit to Main**: Always use feature branches
- ❌ **No Vague Messages**: "fix stuff" or "updates" are not acceptable

## Success Indicators

Successful commit and push when:
- ✅ Changes staged and committed
- ✅ Commit message follows conventional format
- ✅ Pushed to correct feature branch (not main)
- ✅ User informed of success with clear next steps

## Remember

This prompt handles the Git workflow only. It does not:
- Execute step activities (that's `/execute-step`)
- Validate success criteria (that's `/validate-step`)
- Make code changes (those should be done before calling this prompt)

Focus on clean, semantic commits to the correct branch.
