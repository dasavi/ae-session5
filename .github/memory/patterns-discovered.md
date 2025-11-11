# Patterns Discovered

## Purpose

This file documents recurring code patterns, architectural decisions, and best practices discovered during development. Each pattern includes context, problem statement, solution, and examples. Use these patterns to maintain consistency and avoid repeating past mistakes.

---

## Pattern Template

```markdown
### Pattern Name

**Context:**
When does this pattern apply? What situation calls for it?

**Problem:**
What problem does this pattern solve?

**Solution:**
How do we solve it in this project?

**Example:**
Code snippet or specific file reference

**Related Files:**
- path/to/file1.js
- path/to/file2.js

**Date Discovered:** YYYY-MM-DD
```

---

## Example Pattern

### Service Initialization with Empty Arrays

**Context:**
When initializing in-memory data stores or service state in backend services, particularly for collection-type data (todos, users, items, etc.).

**Problem:**
Services that initialize with `null` or `undefined` require extensive null checking throughout the codebase and can cause runtime errors when array methods are called.

**Solution:**
Always initialize collection-type state with empty arrays (`[]`) rather than `null` or `undefined`. This allows array methods to work immediately without null checks and provides a clear, predictable initial state.

**Example:**
```javascript
// ❌ BAD: Requires null checks everywhere
let todos = null;

app.get('/api/todos', (req, res) => {
  if (todos === null) {
    return res.json([]);
  }
  res.json(todos);
});

// ✅ GOOD: Works immediately, no null checks needed
let todos = [];

app.get('/api/todos', (req, res) => {
  res.json(todos);
});
```

**Related Files:**
- `packages/backend/src/app.js` - Main application initialization

**Date Discovered:** November 11, 2025

---

## Discovered Patterns

*(Add new patterns below this line, most recent first)*
