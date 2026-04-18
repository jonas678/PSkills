# Setup: Status File + Project Hook

## Folder structure

All sdd-tdd artifacts for a feature live in one subfolder:

```
docs/specs/<feature-slug>/
├── spec.md          ← design document (written after Phase 1 approval)
├── test-cases.md    ← approved QA test case descriptions (written in Phase 2)
└── status.json      ← flow tracking for this feature
```

Each feature gets its own subfolder. No naming conflicts, no archiving needed — completed flows stay in place and the hook ignores them automatically.

## 1. Check for an existing in-progress flow

Search for any `docs/specs/*/status.json` that has at least one phase not completed:
- Found with incomplete phases → resume from the first incomplete phase; skip completed ones
- Found but all phases completed → start a new feature (create a new subfolder)
- None found → fresh start

## 2. Initialize the feature folder and status.json

Create `docs/specs/<feature-slug>/` and write `docs/specs/<feature-slug>/status.json` using the **Write tool**. Fill in `feature`, `description`, and `started_at` (current ISO timestamp) — never leave them as placeholders.

```json
{
  "feature": "<feature-slug e.g. user-auth>",
  "description": "<one-line description of what is being built>",
  "mode": null,
  "started_at": "<ISO 8601 timestamp e.g. 2026-04-18T10:30:00Z>",
  "spec_file": null,
  "test_cases_file": null,
  "test_files": [],
  "test_commands": {},
  "team": {},
  "coordination_pattern": null,
  "phases": {
    "spec":      { "status": "pending", "completed_at": null },
    "test":      { "status": "pending", "completed_at": null, "red_verified": false },
    "implement": { "status": "pending", "completed_at": null, "green_verified": false }
  }
}
```

**Never update this file with bash or jq.** Always: Read → merge in memory → Write the full JSON back.

## 3. Write the hook script

Write `.claude/hooks/sdd-tdd-check.sh` (pure bash — no Python or jq required):

```bash
#!/bin/bash
# Reads docs/specs/*/status.json using grep/sed only — no runtime dependencies.

extract() { grep "\"$1\"" "$2" | head -1 | sed 's/.*"'"$1"'": *"\([^"]*\)".*/\1/'; }

for status_file in docs/specs/*/status.json; do
    [ -f "$status_file" ] || continue

    feature=$(extract feature "$status_file")
    description=$(extract description "$status_file" | cut -c1-60)

    # Each phase is on one line: "spec": { "status": "...", ... }
    # Use [^_] to avoid matching spec_file, test_commands, test_files etc.
    spec_status=$(grep '"spec"[^_]' "$status_file" | sed 's/.*"status": *"\([^"]*\)".*/\1/')
    test_status=$(grep '"test"[^_c]' "$status_file" | sed 's/.*"status": *"\([^"]*\)".*/\1/')
    impl_status=$(grep '"implement"' "$status_file" | sed 's/.*"status": *"\([^"]*\)".*/\1/')

    pending=""
    [ "$spec_status" != "completed" ]  && pending="${pending}Phase 1 (SPEC), "
    [ "$test_status" != "completed" ]  && pending="${pending}Phase 2 (TEST), "
    [ "$impl_status" != "completed" ]  && pending="${pending}Phase 3 (IMPLEMENT), "
    pending="${pending%, }"

    [ -z "$pending" ] && continue

    echo ""
    echo "[sdd-tdd] Active flow: $feature — $description"
    echo "[sdd-tdd] Pending: $pending"

    red_verified=$(grep '"red_verified"' "$status_file" | sed 's/.*"red_verified": *\(true\|false\).*/\1/')
    if [ "$test_status" = "in_progress" ] && [ "$red_verified" != "true" ]; then
        echo "[sdd-tdd] ⚠ Tests written but RED not verified yet"
    fi
done
```

Then: `chmod +x .claude/hooks/sdd-tdd-check.sh`

## 4. Register in project settings

Merge into `.claude/settings.json` (create if needed; append if keys already exist):

```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  },
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "hooks": [{ "type": "command", "command": "bash .claude/hooks/sdd-tdd-check.sh" }]
      }
    ]
  }
}
```

The `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` flag is required for the Agent Teams coordination pattern. It is set unconditionally — it has no effect when Orchestrator-Subagent is used.

Tell the user: "Phase tracker initialized at `docs/specs/<feature-slug>/status.json`. A project-scoped hook will remind you of incomplete phases — it won't affect other projects."
