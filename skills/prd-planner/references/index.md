# PRD Planner References Index

This index helps choose the right reference file for the current planning task.

Use this file when you want a quick map of:
- which reference to open first
- which references are core vs optional
- which references apply to new systems vs existing-system changes
- which references are most relevant for multi-agent handoff and PM orchestration

---

## 1. Core references for most planning tasks

These are the most generally useful references across many projects.

### `discovery-checklist.md`
Use for:
- early requirement intake
- vague ideas
- first-round clarification
- identifying missing scope, user, and dependency information

### `prd-template.md`
Use for:
- main PRD generation
- stable product definition after discovery
- MVP and phased planning output

### `api-spec-template.md`
Use for:
- backend interface definition
- request / response schema planning
- service contract expansion after the main PRD is stable

### `output-quality-rules.md`
Use for:
- strengthening outputs from “structured” to “implementation-usable”
- frontend / backend / impact-analysis detail standards
- improving package quality before handoff

---

## 2. Scenario guidance references

These references are especially useful when a specific planning scenario dominates the task.

### `new-system-guidance.md`
Use for:
- 0-to-1 systems
- unstable object models
- early MVP and system-boundary framing
- avoiding greenfield modeling mistakes

### `existing-system-delta-guidance.md`
Use for:
- current-state / target-state planning
- unchanged-scope definition
- compatibility-aware and migration-aware change planning
- optimization and redesign requests on existing systems

### `internal-platform-eval-guidance.md`
Use for:
- internal platforms
- QA and test systems
- evaluation tools
- black-box vs semi-white-box vs white-box scope clarification
- execution / evaluator / result model clarification

---

## 3. Templates for existing-system changes

Use these when the request is not greenfield and you want change-oriented output structure or sequencing support.

### `change-prd-template.md`
Use for:
- existing feature improvements
- optimization requests
- partial rewrites
- compatibility-aware changes
- migration-sensitive planning

### `doc-package-sequencing-template.md`
Also useful here when deciding how much documentation should be expanded for a change versus keeping the package delta-focused.

---

## 4. References for multi-agent / multi-role handoff

Use these when the user wants to hand work to designer, engineering, QA, or other AI agents.

### `multi-agent-delivery-guidance.md`
Use for:
- deciding when to leave narrative PRD mode and enter role-ready handoff mode
- clarifying Designer / Engineering / QA / PM responsibilities
- packaging a planning effort into execution-ready downstream materials

### `multi-agent-handoff-template.md`
Use for:
- creating role-specific handoff docs
- deciding which briefs and orchestration materials should exist
- packaging a planning effort into downstream execution handoff

### `agent-prompt-round-template.md`
Use for:
- round-1 / round-2 / round-3 prompts
- iterative downstream agent collaboration
- preventing repeated full rewrites across rounds

### `prompt-round-guidance.md`
Use for:
- deciding what each round should do
- distinguishing first-pass output from revision rounds
- preventing reopened decisions and repeated outputs
- PM-led prompt sequencing

### `pm-feedback-template.md`
Use for:
- packaging PM review feedback
- turning first-round outputs into second-round inputs
- separating confirmed items, revision items, and open decisions

### `cross-role-interface-template.md`
Use for:
- designer → engineering handoff
- engineering → QA handoff
- QA → PM / engineering feedback loops
- reducing PM manual translation effort between roles

### `readiness-checklist-template.md`
Use for:
- deciding whether a stage is ready to move forward
- checking design-ready / engineering-ready / QA-ready / handoff-ready / execution-ready status

### `final-doc-review-checklist.md`
Use for:
- final package consistency audit
- cross-document terminology and state-semantics checks
- final low-risk repair pass before handoff
- deciding whether the generated docs are truly ready to send

---

## 5. References for planning process control

### `plan-mode-playbook.md`
Use for:
- structured planning rounds in `/plan`
- organizing checkpoints and decision-focused collaboration
- maintaining process discipline across long planning conversations

### `doc-package-sequencing-template.md`
Use for:
- deciding which documents to create first
- deciding which docs are mandatory vs optional
- sequencing from discovery → PRD → technical docs → handoff → execution materials

---

## 6. References for capability overview and maintenance

### `skill-capability-map.md`
Use for:
- understanding what `prd-planner` currently covers
- quickly explaining the skill’s scope to collaborators
- checking whether a request fits the skill’s strengths

### `index.md`
Use for:
- quickly locating the right reference file
- future maintenance as the reference set grows

---

## 7. Suggested reference paths by scenario

### Scenario A: brand-new product or system
Recommended order:
1. `discovery-checklist.md`
2. `new-system-guidance.md`
3. `prd-template.md`
4. `api-spec-template.md` as needed
5. `doc-package-sequencing-template.md`
6. multi-agent references if the work will be handed off downstream

### Scenario B: existing-system optimization or feature update
Recommended order:
1. `discovery-checklist.md`
2. `existing-system-delta-guidance.md`
3. `change-prd-template.md`
4. `doc-package-sequencing-template.md`
5. `api-spec-template.md` only for impacted interfaces
6. `output-quality-rules.md` when the change package needs more detail
7. handoff references only if the change is large enough to involve multiple roles

### Scenario C: PM preparing downstream AI-agent collaboration
Recommended order:
1. `multi-agent-delivery-guidance.md`
2. `multi-agent-handoff-template.md`
3. `prompt-round-guidance.md`
4. `agent-prompt-round-template.md`
5. `pm-feedback-template.md`
6. `cross-role-interface-template.md`
7. `readiness-checklist-template.md`

### Scenario D: PM deciding whether to keep documenting or move to execution
Recommended order:
1. `doc-package-sequencing-template.md`
2. `readiness-checklist-template.md`
3. `final-doc-review-checklist.md`
4. `skill-capability-map.md`

### Scenario E: internal platform / test / eval tool
Recommended order:
1. `discovery-checklist.md`
2. `internal-platform-eval-guidance.md`
3. `prd-template.md`
4. `doc-package-sequencing-template.md`
5. `output-quality-rules.md`
6. handoff and review references if downstream execution will involve multiple roles

---

## 8. Important scope note

`prd-planner` is a **general planning and handoff skill**, not a domain-specific skill for only internal tools or only test/eval systems.

Some references or guidance sections are more specialized because they capture high-value scenarios that recur often, but they should be treated as:
- scenario-specific accelerators
- optional special guidance
- not the definition of the overall skill scope

The skill still broadly supports:
- 0-to-1 new systems
- existing-system changes and optimizations
- internal tools
- platform workflows
- multi-role / multi-agent delivery preparation
