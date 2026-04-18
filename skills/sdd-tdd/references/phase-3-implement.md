# Phase 3: IMPLEMENT

Goal: make the failing tests pass by spawning specialized agents, each working from the spec with focused context.

## Steps

**3a.** Update status: `phases.implement.status = "in_progress"`

**3b. Read the spec** and decide which agents to spawn:

| Agent | When to use |
|-------|------------|
| Frontend | UI components, routing, state, styles |
| Backend | API endpoints, business logic, middleware |
| Database | Schema changes, migrations, queries |
| Full-stack | Feature is narrow; splitting creates more overhead than value |

One agent is fine for a narrow feature. Only spawn what's actually needed.

See `references/agent-roles.md` for brief templates for each agent type.

**3c. Enter plan mode.** Present the implementation plan:
- Which agents, what each will implement
- Which tests each agent owns
- Any cross-agent dependencies (resolve *before* spawning — agents can't coordinate with each other)

Wait for user approval.

**3d. Exit plan mode.** Resolve cross-agent dependencies upfront by writing shared types, interfaces, or API contracts to files all agents will read.

**3e. Spawn agents** — in parallel if truly independent, sequentially if one depends on another's output. Every agent brief must include:
- Spec file path (agents must read it)
- Test file path(s) they own
- The test run command
- Any shared contract files from 3d

**3f. Run the full test suite.** Must be GREEN.

If tests still fail: read the failure, fix the specific issue, re-run. Don't re-implement from scratch.

**3g. Update status.json:**
```json
{
  "phases": {
    "implement": { "status": "completed", "completed_at": "<ISO timestamp>", "green_verified": true }
  }
}
```

Tell the user: implementation complete, tests GREEN. The hook will go silent — all phases done.
