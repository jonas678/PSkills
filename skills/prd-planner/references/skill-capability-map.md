# PRD Planner Capability Map

This document summarizes what the `prd-planner` skill currently covers, what kinds of requests it is strongest at, and what outputs it can generate across the full planning-to-handoff workflow.

---

## 1. Skill mission

`prd-planner` is no longer only a PRD drafting helper.

Its broader mission is:
- turn vague product ideas into structured requirements
- stabilize product scope and object model
- expand requirements into role-usable document packages
- support PM-led multi-agent collaboration and iterative refinement
- help transition from planning into execution handoff

Important scope note:
- this is a general planning skill, not a domain-specific skill only for internal platforms or test/eval tools
- specialized guidance for internal platforms, test systems, or multi-agent delivery exists to accelerate recurring scenarios, not to narrow the overall scope

---

## 2. Core capability layers

### Layer A: Requirement discovery and clarification
The skill can:
- intake vague product ideas
- ask focused clarification questions in batches
- summarize checkpoints
- separate confirmed facts, assumptions, open questions, and recommendations
- structure early exploration before writing a PRD

Best for:
- unclear ideas
- greenfield concepts
- early brainstorming with decision options

Related references:
- `discovery-checklist.md`

---

### Layer B: Product definition and PRD generation
The skill can:
- generate a main PRD after enough clarity is reached
- define goals, users, scenarios, scope, assumptions, risks, and dependencies
- structure MVP and later phases
- support product, design, frontend, backend, QA, and PM audiences

Best for:
- main product requirement definition
- MVP definition
- turning clarified requirements into team-usable documentation

Related references:
- `prd-template.md`
- `new-system-guidance.md`

---

### Layer C: Existing-system change and optimization PRDs
The skill can:
- switch into delta-analysis mode
- describe current state, target state, unchanged scope, and migration/compatibility needs
- classify additive vs substitutive vs refactor-driven change
- produce impact-aware change PRDs

Best for:
- optimization of existing apps
- workflow redesigns
- partial feature rewrites
- migration or compatibility-aware changes

Related references:
- `existing-system-delta-guidance.md`
- `change-prd-template.md`

---

### Layer D: Scenario-specific accelerator for internal platforms / test-eval tools
This is a specialized guidance path, not the core definition of the skill. The skill can:
- clarify internal platform framing
- model target system / test suite / run / result / evaluator relationships
- distinguish black-box vs semi-white-box vs white-box evaluation scope
- define result analysis views and execution semantics

Best for when this scenario applies:
- internal QA tools
- developer platforms
- eval systems
- test orchestration products

Related references:
- `internal-platform-eval-guidance.md`

---

### Layer E: Document package expansion
The skill can expand beyond one PRD into a document set such as:
- page requirements
- data model and API appendix
- technical design appendix
- open questions tracker
- acceptance checklist
- review materials
- Jira planning guidance

Best for:
- PMs preparing team-ready deliverables
- projects where multiple disciplines need different views of the same product definition

Related references:
- `doc-package-sequencing-template.md`
- `output-quality-rules.md`

---

### Layer F: Multi-agent handoff planning
The skill can produce or recommend:
- Designer brief
- Engineering brief
- QA brief
- direct-copy prompt templates
- PM orchestration / review playbook

Best for:
- AI-agent-assisted delivery workflows
- multi-role team handoff preparation
- PM-led collaboration after PRD stabilization

Related references:
- `multi-agent-delivery-guidance.md`
- `multi-agent-handoff-template.md`

---

### Layer G: Prompt-round planning
The skill can:
- structure round-1, round-2, and round-3 prompts
- distinguish fresh-output rounds from revision rounds
- help prevent reopened decisions and repeated outputs
- support PM-controlled iterative collaboration

Best for:
- iterative design / engineering / QA collaboration
- projects where prompts must be reused across rounds

Related references:
- `prompt-round-guidance.md`
- `agent-prompt-round-template.md`

---

### Layer H: PM review and feedback packaging
The skill can:
- help PM review outputs role by role
- package round-1 outputs into round-2 input format
- separate confirmed items, revision items, and open decision items
- reduce vague PM feedback and convert it into actionable delta instructions

Best for:
- PM coordination across multiple downstream agents
- structured review cycles
- avoiding “please improve this” style unhelpful follow-up prompts

Related references:
- `pm-feedback-template.md`
- `cross-role-interface-template.md`

---

### Layer I: Cross-role coordination and readiness gating
The skill can:
- define role-to-role interfaces
- sequence document packages
- provide readiness checklists for PRD, design, engineering, QA, handoff, execution, and validation stages
- help determine when to stop expanding docs and move to execution

Best for:
- larger structured planning efforts
- avoiding premature handoff or over-documentation
- cross-functional project orchestration

Related references:
- `readiness-checklist-template.md`
- `doc-package-sequencing-template.md`

---

### Layer J: Final package audit and repair
The skill can:
- review all generated documents as a package
- detect terminology drift, semantic mismatch, and stale downstream prompts
- check whether briefs, prompts, and PM playbooks still reflect the latest confirmed scope
- perform low-risk repairs directly
- surface higher-risk conflicts for explicit confirmation

Best for:
- end-of-cycle cleanup before handoff
- multi-round projects with many generated docs
- PMs asking “is this package really ready?”

Related references:
- `final-doc-review-checklist.md`
- `output-quality-rules.md`

---

## 3. Strongest request types

`prd-planner` is strongest when the user needs one or more of these:
- a new product or system definition
- an MVP breakdown
- an internal tool or platform definition
- a feature improvement or optimization PRD for an existing system
- page requirements and user-flow expansion
- backend/data/API planning support
- team-ready or agent-ready handoff materials
- multi-round PM-led collaboration support

---

## 4. Typical output ladder

A common end-to-end output ladder supported by this skill is:

1. intake summary
2. requirement checkpoint
3. main PRD
4. page requirements
5. data model and API
6. technical design appendix
7. open questions / acceptance / review materials
8. role briefs
9. prompt templates by round
10. PM review and feedback packaging
11. final package audit and repair
12. execution handoff package

Not every project needs every step, but the skill now supports the full ladder.

---

## 5. Recommended default workflow

A strong default workflow is:

1. requirement intake
2. structured clarification
3. summary checkpoint
4. main PRD
5. supporting appendices
6. role-specific handoff docs
7. prompt-round setup
8. PM review cycle
9. final document audit and repair
10. transition into execution or validation

---

## 6. Current limitations to remember

Although the skill is now broad, it still works best when:
- the user is willing to iterate in rounds
- major product ambiguity is addressed before generating downstream execution docs
- the PM or user is willing to confirm checkpoints before later-round handoffs

It is less suitable as a one-shot “generate everything blindly” tool.

---

## 7. Practical summary

The skill is best understood as:

> a planning and handoff system that starts with vague requirements and can continue all the way into structured multi-role / multi-agent execution preparation.
