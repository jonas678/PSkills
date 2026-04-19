# Existing-System Delta Guidance

Use this reference when the request is about changing, optimizing, refactoring, redesigning, or extending an existing product, workflow, page, or backend capability.

This guidance exists to prevent a common failure mode:
- rewriting the whole product as if it were greenfield
- losing compatibility constraints
- failing to define what stays unchanged

---

## 1. When to use this reference

Use this reference when the task is about:
- a new feature added to an existing system
- an optimization of an existing workflow
- a UX improvement
- a backend capability extension
- a partial rewrite
- a migration-sensitive change
- a compatibility-aware refactor
- a performance or reliability improvement that still needs product framing

Do not use this as the primary guidance for:
- a truly new product with no meaningful baseline
- a pure greenfield system definition

For greenfield cases, prefer:
- `new-system-guidance.md`

---

## 2. Core principle

For existing-system work, always plan delta-first.

That means explicitly separating:
- current state
- target state
- unchanged scope
- compatibility or migration behavior

A change PRD is not just “the new thing”.
It is also an explanation of:
- what exists now
- what changes
- what does not change
- how the transition should behave

---

## 3. What to establish before writing (The "System Analyst" Phase)

**CRITICAL RULE for existing codebases:** Do not guess the current implementation.

Before drafting the target plan, you must establish the exact "As-Is" state. If you are operating within Claude Code, **spawn a System Analyst using the `Agent` tool with `subagent_type: "Explore"`** to read the codebase for you.

Brief the Explore Agent to:
1. Search the codebase for the relevant feature (e.g., "Find how our authentication middleware currently works").
2. Identify the exact files, database schemas, and API endpoints involved.
3. Return a concise, high-level summary of the architecture and dependencies, avoiding massive raw code dumps.

Once the Explore Agent returns with facts, you (the PM) can clarify:

### 3.1 Current state (Based on the Analyst's findings)
- What exists today?
- Which page, module, workflow, or API is involved?
- Who uses it now?
- What is the current behavior?

### 3.2 Change motivation
- Why is this change being requested?
- What pain point, inefficiency, defect, or limitation is driving it?
- Is the goal speed, clarity, reliability, flexibility, consistency, or capability expansion?

### 3.3 Change scope
- What exactly is changing?
- What remains unchanged?
- Is the request additive, substitutive, or refactor-driven?

### 3.4 Compatibility
- Must existing behavior remain compatible?
- Is migration required?
- Is there a coexistence period?

---

## 4. Change-type classification

Classify the request as one or more of:

- new feature added to an existing product
- optimization of an existing workflow
- UX / frontend interaction improvement
- backend capability extension
- data model evolution
- refactor without major user-facing change
- migration-driven change
- performance / stability / reliability improvement

This classification should change how the PRD is written.

For example:
- UX optimization needs clear current-vs-target flow comparison
- backend extension needs interface and data impact analysis
- migration-driven work needs rollout and compatibility notes
- reliability work still needs acceptance framing, not only technical language

---

## 5. Delta-first writing rules

For existing-system work, explicitly include:

### 5.1 Current behavior
- What happens now?
- What are the limitations?

### 5.2 Target behavior
- What should happen after the change?

### 5.3 Unchanged behavior
- Which modules, flows, rules, and contracts should remain unchanged?

### 5.4 Compatibility / migration behavior
- What happens to old data, old interfaces, old links, or existing workflows?
- Is there a migration window, fallback mode, or phased rollout?

Do not write the whole product as if nothing existed before unless the user explicitly asks for a full rewritten PRD.

---

## 6. Compatibility and migration checklist

Explicitly check whether the change affects:
- existing pages and user flows
- existing API contracts
- existing database schemas
- historical data
- permissions and roles
- operational or automation workflows
- reporting or analytics definitions
- existing tests and release criteria

If any of these are affected, the PRD should say so directly.

---

## 7. Impact analysis method

Always identify:
- which modules are unchanged
- which modules must be modified
- which modules are newly added
- whether any old behavior should be deprecated
- whether anything should be removed

Prefer using an explicit table:

| Module | Type | Action | Description |
|---|---|---|---|

Where action is one of:
- add
- modify
- remove
- refactor

---

## 8. Optimization-specific analysis

When the request is an optimization, do not stop at “improve the experience.”

Also clarify:
- what is wrong today
- which user pain point is being reduced
- what measurable improvement is expected
- what tradeoff is introduced
- whether the change is UX-only, backend-only, or both

This prevents shallow “improvement PRDs” that do not actually define a better target state.

---

## 9. Rollout and release considerations

For non-trivial changes, consider whether the PRD should mention:
- phased rollout
- feature flag
- data migration
- compatibility window
- rollback or fallback plan
- post-release monitoring

This is especially important when:
- existing users depend on the current behavior
- existing APIs are already consumed
- historical data must remain interpretable

---

## 10. Common failure modes

Watch for these mistakes:

- rewriting the whole product instead of describing the delta
- under-specifying impacted modules
- forgetting to define unchanged scope
- ignoring compatibility constraints
- skipping migration implications
- treating performance or reliability work as if it needs no acceptance criteria
- leaving rollout strategy completely implicit when compatibility matters

---

## 11. Suggested output shape

A strong change-oriented PRD often includes:
- current-state summary
- problem in current design
- target-state summary
- scope of change
- unchanged scope
- compatibility / migration considerations
- impacted modules and interfaces
- rollout and acceptance plan

---

## 12. Suggested next references

After using this reference, commonly useful next references are:
- `change-prd-template.md`
- `api-spec-template.md`
- `doc-package-sequencing-template.md`
- `output-quality-rules.md`
