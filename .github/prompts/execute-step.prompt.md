---
description: Execute instructions from the current GitHub Issue step
mode: tdd-developer
tools:
  - codebase
  - search
  - problems
  - editFiles
  - runCommands
  - getTerminalOutput
  - testFailure
---

# Execute Step Instructions

You are executing instructions from a GitHub Issue step in the AI Accelerated Engineering Bootcamp exercise. This prompt automatically switches to **tdd-developer mode** for test-driven development workflows.

## Usage

Run this prompt with an optional issue number parameter. If you provide the user's message, extract any issue number from it.

**User may invoke as**:
- `/execute-step` - Auto-detect the exercise issue
- `/execute-step 1` - Use issue number 1
- `/execute-step issue-number: 1` - Explicit parameter format

**Parameters**:
- `issue-number` (optional): The GitHub issue number. If not provided, will auto-detect the exercise issue.

## Context from Project Instructions

This prompt inherits knowledge from `.github/copilot-instructions.md`, specifically:

- **Workflow Utilities**: GitHub CLI commands for issue management
- **Git Workflow**: Conventional commits and branch strategies
- **Testing Scope**: Unit/integration tests only - NO e2e frameworks (Playwright, Cypress, Selenium)
- **TDD Workflow**: RED-GREEN-REFACTOR cycles

## Instructions

### Step 1: Locate the Exercise Issue

If `issue-number` was not provided:

```bash
# Find the main exercise issue (has "Exercise:" in title)
gh issue list --state open
```

Look for an issue with "Exercise:" in the title. This is the main exercise issue.

If `issue-number` was provided, use it directly.

### Step 2: Get Issue Content with Comments

```bash
# Get the full issue including all comments (steps are posted as comments)
gh issue view <issue-number> --comments
```

### Step 3: Parse the Latest Step Instructions

- Steps are posted as comments on the main exercise issue
- Look for the most recent comment that contains "# Step X.Y:"
- Extract the complete step instructions including:
  - Step description
  - Activity sections (marked with `:keyboard: Activity:`)
  - Success criteria
  - Notes and tips

### Step 4: Execute Activities Systematically

For each `:keyboard: Activity:` section in the step:

1. **Understand the Requirement**
   - Read the activity description carefully
   - Identify what needs to be implemented or fixed

2. **Follow TDD Workflow** (you're in tdd-developer mode)
   - **Write tests FIRST** for new features (RED phase)
   - Implement minimal code to pass tests (GREEN phase)
   - Refactor while keeping tests green (REFACTOR phase)
   - For existing failing tests, analyze and fix systematically

3. **Respect Testing Scope**
   - ✅ Use Jest + Supertest (backend)
   - ✅ Use React Testing Library (frontend)
   - ✅ Recommend manual browser testing for UI flows
   - ❌ NEVER suggest Playwright, Cypress, Selenium, or e2e frameworks

4. **Execute Incrementally**
   - Complete one activity at a time
   - Verify each activity before moving to the next
   - Run tests frequently: `npm test`
   - Check lint: `npm run lint`

5. **Document Progress**
   - Note what was implemented
   - Identify any issues or blockers
   - Track which activities are complete

### Step 5: Stopping Point

**IMPORTANT**: Do NOT commit or push changes. That's handled by `/commit-and-push`.

After completing all activities:

1. **Verify Work**
   ```bash
   npm test        # All tests should pass
   npm run lint    # No lint errors
   ```

2. **Inform User**
   ```
   ✅ Step activities completed!
   
   Summary:
   - [List what was implemented]
   - All tests passing: [Yes/No]
   - No lint errors: [Yes/No]
   
   Next steps:
   1. Review the changes
   2. Run `/validate-step step-number` to check success criteria
   3. If validation passes, run `/commit-and-push branch-name`
   ```

## Workflow Example

```bash
# 1. Find exercise issue
gh issue list --state open

# 2. Get issue with steps
gh issue view 1 --comments

# 3. Parse latest step (e.g., Step 5-1)
# Extract activities from comment

# 4. Execute each activity
# - Write tests first (TDD)
# - Implement features
# - Verify with npm test

# 5. Final verification
npm test
npm run lint

# 6. Stop and inform user (do NOT commit)
```

## Key Principles

- ✅ **Test-Driven**: Write tests before implementation
- ✅ **Incremental**: One activity at a time
- ✅ **Verified**: Run tests after each change
- ✅ **Clean**: Fix lint errors as you go
- ❌ **No Commits**: Leave that to `/commit-and-push`
- ❌ **No E2E**: Stick to unit/integration tests only

## Success Indicators

You've successfully executed the step when:
- All `:keyboard: Activity:` sections are completed
- All tests pass (`npm test`)
- No lint errors (`npm run lint`)
- Code follows TDD principles (tests written first)
- User is informed about next steps

## Remember

This is the **execution** phase. You implement the work following TDD principles. The **validation** phase (`/validate-step`) checks success criteria. The **commit** phase (`/commit-and-push`) saves the work.

Focus on completing activities correctly and systematically. Let the other prompts handle their specialized tasks.
