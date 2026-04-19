# Change PRD Template

Use this template when the request is about improving, optimizing, extending, refactoring, or partially redesigning an existing application or feature.

This template is intentionally delta-oriented. Do not rewrite the entire system unless the user explicitly wants a full rewritten PRD.

---

## 1. Document information

| Field | Content |
|---|---|
| Document name | |
| Version | |
| Status | Draft / Review / Final |
| Updated at | |
| Related system | |
| Change type | New feature / Optimization / Refactor / Compatibility / Migration / Reliability |

---

## 2. Change background

Describe:
- what exists today
- what pain point or limitation exists
- why the change is needed now
- who is affected by the current issue

Suggested prompts:
- What is the current user experience or system behavior?
- What problem is repeatedly happening?
- Is the driver user-facing, technical, operational, or compliance-related?

---

## 3. Current-state summary

Describe the current baseline clearly.

Include when relevant:
- current pages or workflows
- current backend behavior
- current APIs
- current data model
- current permissions
- known limitations

Good output format:
- bullet list
- current flow summary
- current-state table

Example table:

| Area | Current behavior | Limitation |
|---|---|---|
| Results page | Shows all records in one list | Hard to filter and compare |
| Run cancellation | Not supported | Users cannot stop wasteful runs |

---

## 4. Problem statement

Define the specific problem this change solves.

Separate:
- user pain
- business or operational pain
- technical pain

Avoid vague wording like “optimize experience” without saying what is wrong.

---

## 5. Goals and non-goals

### Goals
State what should improve after the change.

Examples:
- reduce time to find failed cases
- improve configuration reuse
- support backward-compatible extension of an API
- reduce execution failures caused by invalid mapping configuration

### Non-goals
Explicitly state what is not being changed in this iteration.

This is especially important for existing systems.

Examples:
- no redesign of the entire run data model
- no migration to a new queue system in this phase
- no UI redesign outside the results module

---

## 6. Scope of change

Describe exactly what changes.

Useful structure:
- in-scope modules
- in-scope pages
- in-scope APIs
- in-scope data changes

Then separately describe:

## 7. Unchanged scope

List what intentionally remains unchanged.

This helps prevent over-expansion and protects compatibility.

Examples:
- existing run creation flow remains unchanged
- existing CSV import contract remains unchanged
- current judge output schema remains unchanged

---

## 8. Target-state summary

Describe the target behavior after the change.

Recommended format:
- concise summary paragraph
- then bullet list or table of key changes

Good table format:

| Area | Current | Target |
|---|---|---|
| Run list filtering | Limited filters | Agent, status, tag, and failure category filters |
| Result drill-down | Single raw response view | Request, response, assertion, and judge details |

---

## 9. Functional requirements

List the requirements introduced or changed.

For each requirement, indicate whether it is:
- add
- modify
- remove
- refactor

Recommended table:

| ID | Type | Requirement | Notes |
|---|---|---|---|
| FR-01 | modify | Add failure category filter to result list | Backward compatible |
| FR-02 | add | Support rerun-failed action | Phase-dependent |

---

## 10. Frontend changes

Describe:
- impacted pages
- new components or interactions
- modified validation rules
- changed empty/loading/error states
- unchanged areas

Recommended per page:
- page name
- current behavior
- target behavior
- key interaction changes
- edge cases

---

## 11. Backend changes

Describe:
- impacted endpoints
- new endpoints
- modified request/response contracts
- service logic changes
- concurrency / retry / idempotency implications
- logging / monitoring changes

For each changed endpoint, specify whether it is:
- backward compatible
- backward incompatible
- internal only

---

## 12. Data model changes

Describe:
- schema changes
- new fields
- changed meanings of old fields
- data migration or backfill needs
- whether historical data is affected

Recommended table:

| Object / Table | Action | Change |
|---|---|---|
| case_results | modify | Add root_cause_type |
| test_runs | modify | Clarify partial_failed status semantics |

---

## 13. Compatibility and migration plan

This section is critical for existing systems.

Check whether the change affects:
- old clients
- existing pages
- API consumers
- stored data
- scheduled jobs
- dashboards or reports
- test automation

Document:
- compatibility expectations
- migration steps
- backfill strategy
- transition window
- fallback / rollback plan

---

## 14. Rollout plan

When relevant, include:
- feature flag strategy
- gray release
- staged rollout
- internal pilot first
- production monitoring after release

Example:
- phase 1: internal-only flag on
- phase 2: selected operators
- phase 3: all internal users

---

## 15. Impacted modules and change list

Use an add/modify/remove/refactor table.

| Module | Type | Action | Description |
|---|---|---|---|
| Results frontend | frontend | modify | Add advanced filters and details panel |
| Run service | backend | modify | Support rerun-failed workflow |
| Queue worker | backend | unchanged | No behavior changes |

Include unchanged modules when useful to make boundaries explicit.

---

## 16. Acceptance criteria

Acceptance should be change-specific and testable.

Good examples:
- users can filter results by failure category without affecting existing filters
- old API consumers continue to work without payload changes
- historical result records remain queryable after schema migration
- rerun-failed only includes failed executions from the selected run

---

## 17. Risks and open questions

Distinguish:
- known implementation risks
- compatibility risks
- migration risks
- rollout risks
- unresolved questions

Good format:

| Type | Description | Mitigation / Status |
|---|---|---|
| Compatibility risk | Existing frontend expects old enum semantics | Keep old enum, add translation layer |
| Open question | Whether rerun-failed should clone judge config | To be confirmed |

---

## 18. Optional appendices

Add when helpful:
- before/after flowchart
- current vs target API diff
- page inventory diff
- migration checklist
- Jira ticket breakdown
- phased implementation plan

---

## Delta-first writing checklist

Before finalizing a change PRD, verify:

- Did I describe the current state clearly?
- Did I describe the target state clearly?
- Did I say what is intentionally unchanged?
- Did I identify compatibility constraints?
- Did I identify migration or rollout needs?
- Did I specify impacted modules concretely?
- Did I avoid rewriting the whole product like a greenfield system?
