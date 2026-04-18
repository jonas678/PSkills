# Phase 3: IMPLEMENT

Goal: make the failing tests pass by spawning specialized agents, each working from the spec with focused context.

## Steps

**3a.** Update status: `phases.implement.status = "in_progress"`

**3b. Apply coordination-patterns** to decide the implementation shape before spawning anything:

| Signal | Pattern | What to do |
|--------|---------|------------|
| Feature is small (< ~5 files, single domain) | **Do it yourself** | No agents — implement directly |
| 2–4 independent domains (frontend + backend + DB) | **Orchestrator-Subagent** | One agent per domain, in parallel if independent |
| Many uniform items (e.g. 8 similar API endpoints) | **Agent Teams** | One agent per endpoint/module, each doing full implement → test |
| Domains share state mid-flight | **Don't delegate** | Implement sequentially yourself, use a scratch file to track state |

Ask these questions before choosing:
1. Can the domains be implemented completely independently? (no shared mutable state mid-work)
2. Is each piece substantial enough to earn the cold-start cost of a subagent?
3. Are there cross-domain dependencies that must be resolved first?

Resolve any cross-domain dependencies by writing shared types/contracts to files before spawning.

**3c. Read the spec** and identify which agents to spawn based on the pattern chosen:

| Agent | When to use |
|-------|------------|
| Frontend | UI components, routing, state, styles |
| Backend | API endpoints, business logic, middleware |
| Database | Schema changes, migrations, queries |
| Full-stack | Feature is narrow; splitting creates more overhead than value |

See `references/agent-roles.md` for brief templates.

**3d. Enter plan mode.** Present the implementation plan:
- Which agents, what each will implement
- Which tests each agent owns
- Any cross-agent dependencies (resolve *before* spawning — agents can't coordinate with each other)

Wait for user approval.

**3e. Exit plan mode.** Resolve cross-agent dependencies upfront by writing shared types, interfaces, or API contracts to files all agents will read.

**3f. Spawn agents** — in parallel if truly independent, sequentially if one depends on another's output. Every agent brief must include:
- Spec file path (agents must read it)
- Test file path(s) they own
- The test run command
- Any shared contract files from 3d

**3g. Run the full test suite** using the commands from `status.json → test_commands`. Run all layers (frontend, backend, E2E) that were confirmed in Phase 1. Must be GREEN.

If tests still fail: read the failure, fix the specific issue, re-run. Don't re-implement from scratch.

**3h. Update status.json:**
```json
{
  "phases": {
    "implement": { "status": "completed", "completed_at": "<ISO timestamp>", "green_verified": true }
  }
}
```

Tell the user: implementation complete, tests GREEN. The hook will go silent — all phases done.
