# Spec Template

Two modes — pick one based on the nature of the work:

- **Mode A: New Feature** — building something that doesn't exist yet
- **Mode B: Enhancement / Update** — modifying something that already exists

Fill every section. If something is genuinely unknown, write "TBD: <reason>" rather than omitting it.

---

## Mode A: New Feature

```markdown
# Spec: <Feature Name>

**Date:** <date>
**Mode:** New Feature
**Status:** Draft | Approved

## 1. Goal & Scope

One paragraph: what problem does this feature solve, and for whom? What is explicitly OUT of scope?

## 2. Components

List the main building blocks. For each:
- **Name**: what it is
- **Responsibility**: what it owns
- **Interface**: how other components interact with it (function signatures, API endpoints, events)

Example:
- **AuthService** — validates JWT tokens. Interface: `validateToken(token: string): User | null`
- **LoginPage** — renders login form, calls AuthService. Interface: emits `login-success` event on success

## 3. Data Models

Define the key data structures. Use pseudocode or TypeScript-style types:

```
type User = {
  id: string
  email: string
  role: "admin" | "user"
  createdAt: Date
}
```

## 4. Key Decisions

For each non-obvious decision, write an ADR-style entry:

**Decision**: <what was decided>
**Why**: <the reason — constraint, tradeoff, preference>
**Alternatives considered**: <what else was considered and rejected>

## 5. Tech Stack

Only list what's relevant to this feature.

- Language: 
- Framework: 
- Testing: 
- Other: 

## 6. Non-Functional Requirements

- **Performance**: e.g., API response < 200ms p95
- **Security**: e.g., all endpoints require authentication
- **Error handling**: e.g., validation errors return 400 with field-level messages

## 7. Open Questions

List anything still unresolved. Resolve before Phase 2 or explicitly accept as assumptions.
```

---

## Mode B: Enhancement / Update

Use this when modifying something that already exists. The key difference is capturing **as-is** vs **to-be** and the **impact** on the surrounding system.

```markdown
# Spec: <Change Name>

**Date:** <date>
**Mode:** Enhancement / Update
**Status:** Draft | Approved

## 1. Change Summary

One paragraph: what is changing, why, and what's the expected outcome? Be concrete — "add pagination to the user list API" is better than "improve performance".

**Type of change:** Bug fix | Performance | New capability | Refactor | Breaking change | Config/data update

## 2. Current State (As-Is)

Describe what exists today — the components, interfaces, or behavior being changed. Be specific enough that the delta in section 3 is unambiguous.

- What files/modules are involved?
- What does the current interface look like? (signatures, API contract, DB schema)
- What is the current behavior that will change?

This section is the baseline. If the current state isn't documented here, reviewers can't evaluate whether the change is safe.

## 3. Target State (To-Be)

Describe what will be true after the change.

- What is the new interface / behavior?
- Show the before/after for anything that changes signature or contract:

```
// Before
function getUsers(): User[]

// After
function getUsers(page: number, pageSize: number): { users: User[], total: number }
```

## 4. Impact Analysis

This is the most important section for changes to existing systems. Think through ripple effects.

**Components affected**: List every file, module, or service that calls or depends on what you're changing.

**Callers that need updating**: For each affected caller, note whether it needs a code change or is backward-compatible.

**Data impact**: Does this change existing data, database schema, or stored state? If yes, describe the migration.

**Breaking vs non-breaking**: Is this change backward-compatible? If breaking, who is affected and what's the migration path?

## 5. Rollback Plan

If this change causes problems in production, how is it undone?

- Can it be feature-flagged?
- Is there a data migration that needs reverting?
- What's the rollback command / procedure?

If rollback is not possible (e.g., destructive migration), say so explicitly and explain why the risk is acceptable.

## 6. Key Decisions

For each non-obvious decision about *how* the change is implemented:

**Decision**: <what was decided>
**Why**: <the reason>
**Alternatives considered**: <what else was considered>

## 7. Non-Functional Requirements

Only include what's relevant to this specific change.

- **Performance impact**: better / worse / neutral — and why
- **Security**: any new attack surface or auth changes?
- **Backward compatibility**: API versioning, deprecation notices needed?

## 8. Open Questions

List anything still unresolved before implementation begins.
```
