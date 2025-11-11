# GitHub Copilot Instructions for TODO Application

## Project Context

This is a full-stack TODO application with a React frontend and Express backend. Development follows an iterative, feedback-driven approach with emphasis on test-driven development and incremental improvements.

**Current Phase**: Backend stabilization and frontend feature completion

**Tech Stack**:
- Frontend: React with React Testing Library for testing
- Backend: Express.js with Jest and Supertest for testing
- Monorepo structure with separate `packages/frontend` and `packages/backend`

## Documentation References

Consult these files for detailed project information:

- [`docs/project-overview.md`](../docs/project-overview.md) - Architecture, tech stack, and project structure
- [`docs/testing-guidelines.md`](../docs/testing-guidelines.md) - Test patterns and standards
- [`docs/workflow-patterns.md`](../docs/workflow-patterns.md) - Development workflow guidance

## Development Principles

Follow these core principles throughout development:

- **Test-Driven Development**: Follow the Red-Green-Refactor cycle
- **Incremental Changes**: Make small, testable modifications rather than large rewrites
- **Systematic Debugging**: Use test failures as guides to identify and resolve issues
- **Validation Before Commit**: Ensure all tests pass and no lint errors exist before committing

## Testing Scope

This project uses **unit tests and integration tests ONLY**:

- **Backend**: Jest + Supertest for API testing
- **Frontend**: React Testing Library for component unit/integration tests
- **Manual browser testing**: For full UI verification

**DO NOT**:
- Suggest or implement e2e test frameworks (Playwright, Cypress, Selenium)
- Suggest browser automation tools
- Add dependencies for end-to-end testing

**Reason**: This lab focuses on unit and integration tests without the complexity of e2e infrastructure.

### Testing Approach by Context

- **Backend API changes**: Write Jest tests FIRST, then implement (RED-GREEN-REFACTOR)
- **Frontend component features**: Write React Testing Library tests FIRST for component behavior, then implement (RED-GREEN-REFACTOR). Follow with manual browser testing for full UI flows.

**This is true TDD**: Write the test first, watch it fail, then write code to pass the test.

## Workflow Patterns

Follow these development workflows:

### 1. TDD Workflow (Red-Green-Refactor)
1. Write or fix tests
2. Run tests and observe failure (RED)
3. Implement minimal code to pass tests
4. Run tests and observe success (GREEN)
5. Refactor for code quality while keeping tests green
6. Repeat

### 2. Code Quality Workflow
1. Run lint checks (`npm run lint`)
2. Categorize issues by type (syntax, style, unused vars, etc.)
3. Fix issues systematically, category by category
4. Re-validate with lint and tests
5. Commit when clean

### 3. Integration Workflow
1. Identify the issue (bug report, feature request, test failure)
2. Debug to understand root cause
3. Write or update tests to capture expected behavior
4. Implement the fix or feature
5. Verify end-to-end (tests + manual verification if needed)

## Chat Mode Usage

Use specialized chat modes for focused workflows:

- **`tdd-developer` mode**: For test-related work and Red-Green-Refactor cycles. Use when writing tests, implementing features test-first, or debugging test failures.
- **`code-reviewer` mode**: For addressing lint errors, code quality improvements, and ensuring code standards compliance.

Switch modes based on the task at hand to get the most relevant guidance and tool configurations.

## Memory System

The project uses a working memory system to track discoveries and patterns:

- **Persistent Memory**: This file (`.github/copilot-instructions.md`) contains foundational principles and workflows
- **Working Memory**: `.github/memory/` directory contains discoveries and patterns from development sessions
- **During active development**: Take notes in `.github/memory/scratch/working-notes.md` (not committed to git)
- **At end of session**: Summarize key findings into `.github/memory/session-notes.md` (committed)
- **Document recurring patterns**: Add reusable code patterns to `.github/memory/patterns-discovered.md` (committed)
- **AI assistance**: Reference these files when providing context-aware suggestions

See [`.github/memory/README.md`](./memory/README.md) for detailed usage instructions and workflow integration.

## Workflow Utilities

GitHub CLI commands are available for workflow automation:

### Issue Management
- List open issues: `gh issue list --state open`
- Get issue details: `gh issue view <issue-number>`
- Get issue with comments: `gh issue view <issue-number> --comments`

### Exercise Navigation
- The main exercise issue will have "Exercise:" in the title
- Steps are posted as comments on the main issue
- Use these commands when `/execute-step` or `/validate-step` prompts are invoked

## Git Workflow

Follow conventional commit format and branch strategies:

### Conventional Commits
Use semantic commit messages with the following prefixes:
- `feat:` - New features
- `fix:` - Bug fixes
- `chore:` - Maintenance tasks (dependencies, configs)
- `docs:` - Documentation changes
- `test:` - Test additions or modifications
- `refactor:` - Code refactoring without behavior changes
- `style:` - Code style/formatting changes

**Examples**:
```bash
git commit -m "feat: add delete button to todo items"
git commit -m "fix: resolve todo duplication bug"
git commit -m "test: add integration tests for todo API"
```

### Branch Strategy
- **Feature branches**: `feature/<descriptive-name>`
- **Bug fixes**: `fix/<descriptive-name>`
- **Main branch**: `main` (protected, always deployable)

### Commit and Push Workflow
1. Stage all changes: `git add .`
2. Commit with conventional format: `git commit -m "type: description"`
3. Push to the correct branch: `git push origin <branch-name>`
4. Create pull requests for review before merging to `main`
