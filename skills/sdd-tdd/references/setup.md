# Setup: Status File + Project Hook

## 1. Archive any completed flow

Before creating a new status.json, check if one already exists and all phases are completed:

```bash
# If .claude/sdd-tdd-status.json exists and all phases are "completed":
# read the feature slug from the file, then rename it
mv .claude/sdd-tdd-status.json .claude/sdd-tdd-status.<feature-slug>.json
```

This preserves the history of every completed flow. The hook only watches `sdd-tdd-status.json` (the active flow), so archived files are ignored automatically.

## 2. Initialize status.json

Create `.claude/sdd-tdd-status.json` in the project root:

```json
{
  "feature": "<feature-slug>",
  "description": "<user's feature description>",
  "mode": null,
  "started_at": "<ISO timestamp>",
  "spec_file": null,
  "test_files": [],
  "phases": {
    "spec":      { "status": "pending", "completed_at": null },
    "test":      { "status": "pending", "completed_at": null, "red_verified": false },
    "implement": { "status": "pending", "completed_at": null, "green_verified": false }
  }
}
```

## 2. Write the hook script

Write `.claude/hooks/sdd-tdd-check.sh`:

```bash
#!/bin/bash
STATUS_FILE=".claude/sdd-tdd-status.json"
[ -f "$STATUS_FILE" ] || exit 0

python3 - <<'EOF'
import json, sys

with open(".claude/sdd-tdd-status.json") as f:
    s = json.load(f)

phases = s.get("phases", {})
order = ["spec", "test", "implement"]
labels = {"spec": "Phase 1 (SPEC)", "test": "Phase 2 (TEST)", "implement": "Phase 3 (IMPLEMENT)"}

pending = [labels[p] for p in order if phases.get(p, {}).get("status") != "completed"]
if not pending:
    sys.exit(0)

print(f"\n[sdd-tdd] Active flow: {s.get('feature', '?')} — {s.get('description', '')[:60]}")
print(f"[sdd-tdd] Pending: {', '.join(pending)}")

if phases.get("test", {}).get("status") == "in_progress" and not phases.get("test", {}).get("red_verified"):
    print("[sdd-tdd] ⚠ Tests written but RED not verified yet")
EOF
```

Then: `chmod +x .claude/hooks/sdd-tdd-check.sh`

## 3. Register in project settings

Merge into `.claude/settings.json` (create if needed; append if Stop hooks already exist):

```json
{
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

Tell the user: "Phase tracker initialized at `.claude/sdd-tdd-status.json`. A project-scoped hook will remind you of incomplete phases — it won't affect other projects."
