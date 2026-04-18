# Phase 3: IMPLEMENT

Goal: make the failing tests pass. The team composition was decided and approved in Phase 1 — execute that decision.

## Steps

**3a.** Update status using Read → merge → Write (never bash/jq): set `phases.implement.status = "in_progress"` in the full JSON and write it back.

**3b. Read the team from status.json** (`team` field). The agents and their responsibilities were approved in Phase 1. If the spec scope changed substantially since then, note it and confirm with the user before proceeding.

**3c. Enter plan mode.** Present the implementation plan:
- Which agents will run, what each implements (from `status.json → team`)
- Which test files each agent owns
- Shared contracts the orchestrator will write before spawning (types, API interfaces)
- Sequencing: parallel where independent, sequential where one depends on another

Wait for user approval.

**3d. Exit plan mode.** Write shared types, interfaces, or API contracts to files before spawning. Agents cannot coordinate with each other mid-flight — all shared state must be resolved upfront.

**3e. Spawn implementation agents** — in parallel if truly independent, sequentially if one depends on another's output. Every agent brief must include:
- Spec file path (agents must read it)
- Test file path(s) they own
- Test run command
- Any shared contract files written in 3d

See `references/agent-roles.md` for Frontend, Backend, Database, and Full-Stack brief templates.

**3f. Spawn QA agent as verifier.** After all implementation agents complete:
- Brief: spec path, all test files, all test run commands
- QA runs the full test suite and reports per-test pass/fail with failure details
- QA does NOT fix failures — it reports what's failing and why

If RED after QA verification: read QA's report, make a targeted fix, re-spawn QA to verify again. Do not re-implement from scratch.

**3g. Update status.json** using Read → merge → Write (never bash/jq):
- `phases.implement.status`: `"completed"`
- `phases.implement.completed_at`: current ISO timestamp
- `phases.implement.green_verified`: `true`

Tell the user: implementation complete, tests GREEN. The hook will go silent — all phases done.
