# Working Memory System

## Purpose

This directory implements a **working memory system** for tracking patterns, decisions, and lessons learned during development. It helps both human developers and AI assistants maintain context across sessions and apply accumulated knowledge to future work.

## Memory Types

### Persistent Memory
- **Location**: `.github/copilot-instructions.md`
- **Content**: Foundational principles, workflows, and project standards
- **Update Frequency**: Rarely changes; represents core project guidelines
- **Git Status**: Always committed

### Working Memory
- **Location**: `.github/memory/`
- **Content**: Development discoveries, patterns, and session-specific learnings
- **Update Frequency**: Updated during and after each development session
- **Git Status**: Mixed (see Directory Structure below)

## Directory Structure

```
.github/memory/
├── README.md                    # This file - explains the memory system
├── session-notes.md             # Historical summaries of completed sessions (COMMITTED)
├── patterns-discovered.md       # Accumulated code patterns and best practices (COMMITTED)
└── scratch/
    ├── .gitignore              # Ignores all scratch files
    └── working-notes.md        # Active session notes (NOT COMMITTED)
```

### File Purposes

#### `session-notes.md` (Historical Record)
- **Purpose**: Document completed development sessions for future reference
- **When to Update**: At the END of each session, after work is completed
- **Content**: Summary of what was accomplished, key findings, and decisions made
- **Git Status**: **COMMITTED** - This is your project's development history
- **Audience**: Future you, team members, and AI assistants reviewing past work

#### `patterns-discovered.md` (Knowledge Base)
- **Purpose**: Document recurring patterns, solutions, and architectural decisions
- **When to Update**: When you discover a reusable pattern or solve a recurring problem
- **Content**: Structured pattern documentation with examples
- **Git Status**: **COMMITTED** - This is your project's accumulated wisdom
- **Audience**: Developers and AI assistants needing to understand project conventions

#### `scratch/working-notes.md` (Active Session)
- **Purpose**: Quick notes, experiments, and thoughts during active development
- **When to Update**: Throughout your CURRENT development session
- **Content**: Current task, approach, findings, blockers, next steps
- **Git Status**: **NOT COMMITTED** - This is ephemeral working memory
- **Audience**: You and AI assistants during the active session only

## Workflow Integration

### TDD Workflow (Red-Green-Refactor)

**During Development:**
1. Note failing tests in `scratch/working-notes.md` → Current Task
2. Document your implementation approach → Approach section
3. Record test results and why tests failed/passed → Key Findings
4. Note any pattern discoveries → Mark for later addition to `patterns-discovered.md`

**End of Session:**
1. Summarize the TDD cycle outcomes in `session-notes.md`
2. Extract any reusable patterns to `patterns-discovered.md`
3. Leave `scratch/working-notes.md` for next session or clear it

### Code Quality Workflow (Linting)

**During Development:**
1. Document lint error categories in `scratch/working-notes.md` → Current Task
2. Note systematic fixes applied → Approach and Decisions Made
3. Record any patterns in lint violations → Key Findings

**End of Session:**
1. If you discovered a common lint issue pattern, add to `patterns-discovered.md`
2. Summarize linting improvements in `session-notes.md`

### Integration & Debugging Workflow

**During Development:**
1. Document the bug or issue in `scratch/working-notes.md` → Current Task
2. Track debugging steps and hypotheses → Approach and Key Findings
3. Note root cause when found → Decisions Made
4. Record any blockers → Blockers section

**End of Session:**
1. Document the bug pattern and solution in `patterns-discovered.md`
2. Summarize debugging session in `session-notes.md`
3. Include prevention strategies for similar bugs

## How AI Uses This System

### Context Gathering
When you ask an AI assistant for help, it can:
1. Read `.github/copilot-instructions.md` for foundational guidelines
2. Check `patterns-discovered.md` for project-specific patterns
3. Review `session-notes.md` for recent context and decisions
4. Read `scratch/working-notes.md` for your current task and findings

### Pattern Application
AI assistants use accumulated patterns to:
- Suggest solutions consistent with established patterns
- Avoid proposing solutions that have already been tried and rejected
- Apply project-specific conventions automatically
- Warn about anti-patterns discovered in previous sessions

### Continuity Across Sessions
Even in a new chat session, AI can:
- Understand project history from `session-notes.md`
- Apply learned patterns from `patterns-discovered.md`
- Avoid repeating past mistakes documented in the memory system

## Best Practices

### DO ✅
- **Update `scratch/working-notes.md` frequently** during active work
- **Summarize to `session-notes.md`** at end of each meaningful session
- **Document patterns** when you solve something you might encounter again
- **Be specific** with examples and file references in pattern documentation
- **Date your entries** in session-notes.md for chronological tracking
- **Keep scratch/ ephemeral** - it's okay to clear it between sessions

### DON'T ❌
- **Don't commit scratch/** - These are temporary notes, not project history
- **Don't duplicate** - Keep foundational info in copilot-instructions.md
- **Don't be vague** - "Fixed a bug" is less useful than "Fixed toggle bug in app.js line 45"
- **Don't skip summarization** - Active notes lose value if not preserved properly
- **Don't over-document** - Not every small change needs a pattern entry

## Example Workflow: Full Session

```
1. START SESSION
   - Open scratch/working-notes.md
   - Fill in "Current Task" section

2. DURING DEVELOPMENT (30-90 minutes)
   - Note findings in "Key Findings"
   - Track decisions in "Decisions Made"
   - Update "Next Steps" as you progress
   - Document blockers as they arise

3. END SESSION
   - Review scratch/working-notes.md
   - Extract key points → Add to session-notes.md
   - Identify any patterns → Add to patterns-discovered.md
   - Either clear scratch/working-notes.md or leave for next session
   - Commit session-notes.md and patterns-discovered.md

4. AI ASSISTANCE NEXT SESSION
   - AI reads session-notes.md: "Last session fixed toggle bug"
   - AI reads patterns-discovered.md: "Services initialize with empty arrays"
   - AI applies learned patterns to new suggestions
```

## Quick Reference

| File | Purpose | Updated When | Committed |
|------|---------|--------------|-----------|
| `copilot-instructions.md` | Core principles | Rarely | Yes |
| `session-notes.md` | Historical summaries | End of session | Yes |
| `patterns-discovered.md` | Code patterns | When pattern found | Yes |
| `scratch/working-notes.md` | Active session | During session | No |

## Getting Started

1. **First Session**: Start with `scratch/working-notes.md` - just take notes freely
2. **End of First Session**: Summarize to `session-notes.md` - see what feels important
3. **Pattern Discovery**: When you solve something tricky, add to `patterns-discovered.md`
4. **Subsequent Sessions**: Review past session notes before starting new work
5. **Ask AI for Help**: Reference these files when asking questions

---

**Remember**: This system is a tool for you and AI to build shared context over time. It's most valuable when used consistently, but don't let it slow you down. Quick notes now → Better context later.
