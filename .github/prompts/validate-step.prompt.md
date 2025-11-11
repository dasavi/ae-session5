---
description: Validate that all success criteria for the current step are met
mode: code-reviewer
tools:
  - codebase
  - problems
  - runCommands
  - getTerminalOutput
---

# Validate Step Success Criteria

You are validating that all success criteria for a specific exercise step are met. This prompt automatically switches to **code-reviewer mode** for systematic quality verification.

## Usage

```
/validate-step <step-number>
```

**Parameters**:
- `step-number` (REQUIRED): The step number to validate (e.g., "5-0", "5-1", "5-2")

**Examples**:
```
/validate-step 5-0      # Validate Step 5-0
/validate-step 5-1      # Validate Step 5-1
/validate-step 5-2      # Validate Step 5-2
```

## Context from Project Instructions

This prompt inherits knowledge from `.github/copilot-instructions.md`, specifically:

- **Workflow Utilities**: GitHub CLI commands for issue management
- **Testing Guidelines**: Test execution and validation patterns
- **Code Quality**: Lint checking and error resolution

## Input Validation

**REQUIRED**: `step-number` parameter (e.g., "5-0", "5-1", "5-2")

If the step number is not provided, **STOP and ask the user**:

```
I need a step number to validate.

Examples:
- 5-0 (Step 5-0: Setup)
- 5-1 (Step 5-1: Backend Implementation)
- 5-2 (Step 5-2: Frontend Integration)

Please provide the step number:
```

Do NOT proceed without a valid step number.

## Instructions

### Step 1: Locate the Exercise Issue

```bash
# Find the main exercise issue (has "Exercise:" in title)
gh issue list --state open
```

Look for an issue with "Exercise:" in the title. This is the main exercise issue containing all steps.

### Step 2: Get Issue with All Comments

```bash
# Get the complete issue including comments (steps are in comments)
gh issue view <issue-number> --comments
```

### Step 3: Find the Target Step

Search through the issue content and comments to find:

```
# Step {step-number}:
```

For example, if validating step "5-1", look for:
```
# Step 5-1:
```

Extract the complete step content, including:
- Step title and description
- Activities
- **Success Criteria** section (this is what we validate)
- Notes and tips

### Step 4: Extract Success Criteria

Find the "Success Criteria" or "✅ Success Criteria" section in the step.

This typically looks like:
```
## Success Criteria

- [ ] All backend tests pass
- [ ] No ESLint errors in backend code
- [ ] POST endpoint creates todos with auto-generated IDs
- [ ] DELETE endpoint removes todos correctly
```

Parse each criterion into a checklist.

### Step 5: Validate Each Criterion

For each success criterion, check the current workspace state:

#### Test-Related Criteria

```bash
# Check if tests pass
npm test

# Check specific test file
npm test -- packages/backend/__tests__/app.test.js

# Look for specific test patterns
npm test -- --verbose
```

Validate:
- ✅ All tests pass (exit code 0)
- ✅ No test failures or errors
- ✅ Specific tests mentioned in criteria pass

#### Lint/Quality Criteria

```bash
# Check for lint errors
npm run lint

# Check specific package
npm run lint -- packages/backend/src/

# Use problems tool to see all issues
```

Validate:
- ✅ No ESLint errors
- ✅ No compilation errors
- ✅ Code quality rules satisfied

#### Functionality Criteria

For criteria about specific features (e.g., "POST endpoint creates todos"):

```bash
# Start the application
npm run start

# Or check specific endpoint implementation
```

Validate by:
- Reading the implementation code
- Checking test coverage for that feature
- Verifying the feature works as described

#### Code Structure Criteria

For criteria about code organization:

- Check that files exist in correct locations
- Verify imports and exports are correct
- Confirm patterns are followed consistently

### Step 6: Generate Validation Report

Create a comprehensive report:

```
# Validation Report: Step {step-number}

## Success Criteria Status

✅ Criterion 1: [Description]
   - Status: PASS
   - Evidence: All tests pass (npm test)

❌ Criterion 2: [Description]
   - Status: FAIL
   - Evidence: 3 ESLint errors in app.js
   - Required Actions:
     * Fix no-unused-vars error on line 15
     * Remove console.log on line 42
     * Fix missing key prop on line 67

✅ Criterion 3: [Description]
   - Status: PASS
   - Evidence: DELETE endpoint implemented and tested

⚠️  Criterion 4: [Description]
   - Status: PARTIAL
   - Evidence: Feature implemented but tests are failing
   - Required Actions:
     * Fix test failure in "should handle edge case"

## Overall Status

Status: ❌ NOT READY
Completed: 2/4 criteria (50%)

## Next Steps

To complete this step:

1. Fix ESLint errors in app.js
   - Run: npm run lint
   - Address each error systematically

2. Fix failing test: "should handle edge case"
   - Run: npm test -- --testNamePattern="should handle edge case"
   - Debug and fix the implementation

3. Re-run validation after fixes:
   - Run: /validate-step {step-number}

4. When all criteria pass:
   - Run: /commit-and-push <branch-name>
```

### Step 7: Provide Actionable Guidance

For each failing criterion:

1. **Explain what's missing**
   - Be specific about what doesn't meet the criterion
   - Cite evidence (test output, lint errors, etc.)

2. **Provide concrete next steps**
   - Exact commands to run
   - Specific files to check or fix
   - Reference to relevant tools or prompts

3. **Estimate effort**
   - "Quick fix: 5 minutes"
   - "Moderate work: 15-30 minutes"
   - "Significant implementation needed"

## Validation Categories

### Testing Validation

```bash
# Run all tests
npm test

# Check for test failures
# Exit code 0 = all pass
# Exit code 1 = failures exist
```

Look for:
- Total tests run vs. passed
- Any failing test suites
- Error messages or stack traces

### Lint Validation

```bash
# Run linter
npm run lint

# Check exit code
echo $?
```

Look for:
- Error count (must be 0)
- Warning count (ideally 0, but check criteria)
- Specific rule violations

### Feature Validation

For functionality claims:
- Read implementation code
- Check test coverage
- Verify against requirements in step description

### Integration Validation

Check that components work together:
- Frontend calls backend correctly
- API responses handled properly
- Error states managed

## Success Indicators

Validation is complete when:
- ✅ All success criteria checked systematically
- ✅ Evidence collected for each criterion (pass/fail)
- ✅ Clear status report generated
- ✅ Actionable next steps provided for failures
- ✅ User knows exactly what to do next

## Example Workflow

```bash
# 1. Get step info
gh issue list --state open
gh issue view 1 --comments

# 2. Find "# Step 5-1:" in comments
# Extract success criteria

# 3. Validate each criterion
npm test                # Check tests
npm run lint            # Check lint
# Review code for features

# 4. Generate report
# ✅ 3 criteria pass
# ❌ 1 criterion fails

# 5. Provide guidance
# "Fix ESLint errors, then re-validate"
```

## Key Principles

- ✅ **Systematic**: Check every criterion, not just some
- ✅ **Evidence-Based**: Use actual test/lint output, not assumptions
- ✅ **Actionable**: Tell user exactly what to do next
- ✅ **Clear Status**: Pass/Fail/Partial for each item
- ✅ **Encouraging**: Celebrate what's working, guide on what's not
- ❌ **Not a Fixer**: Report status, don't fix issues (that's for execute-step or code edits)

## Remember

This is the **validation** phase. You check and report, you don't fix. If criteria aren't met:

- Report what's missing
- Explain how to check it
- Guide user to appropriate tools:
  - `/execute-step` for implementing missing features
  - Code-reviewer mode for fixing lint/quality issues
  - Manual debugging for complex issues

Your job is to be a thorough, objective validator that helps users know when they're truly done with a step.
