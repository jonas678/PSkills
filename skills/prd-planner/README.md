# PRD Planner

`prd-planner` is a general planning and handoff skill for turning vague ideas into structured requirements, PRDs, supporting documents, and downstream team or agent handoff packages.

It is designed to help with:
- 0-to-1 new systems
- existing-system feature changes and optimizations
- page / flow / API / technical planning
- PM-led multi-agent collaboration and iterative review cycles

For the canonical behavior and rules, see `SKILL.md`.

---

## 3-minute quickstart

### Use this skill when you want to:
- clarify a vague product idea
- define a new system or feature
- improve an existing workflow or application
- expand a PRD into page/data/API/technical docs
- prepare handoff docs for Designer / Engineering / QA
- organize multi-round PM-led collaboration with AI agents

### Fastest way to start
Ask for the planning task directly, for example:

- “Use `$prd-planner` to help me define a new internal tool from scratch.”
- “Use `$prd-planner` to clarify and write a PRD for an existing feature optimization.”
- “Use `$prd-planner` to turn this product idea into a PRD plus team-ready supporting docs.”
- “Use `$prd-planner` to prepare designer / engineering / QA handoff materials and prompt rounds.”

### What usually happens next
A good default flow is:
1. requirement intake
2. clarification questions in small batches
3. requirement checkpoint summary
4. main PRD
5. supporting doc expansion if needed
6. role handoff docs if needed
7. prompt rounds / PM review support if needed
8. final document audit and low-risk repair before handoff

### Important usage tip
Do not expect the best result from a single “generate everything” request.

This skill works best when:
- you allow clarification rounds
- you confirm checkpoints
- you expand into additional documents only after the core product definition is stable

---

## What this skill covers

High-level capability areas:
- requirement discovery and clarification
- PRD generation
- existing-system delta PRDs
- document package expansion
- multi-agent handoff planning
- prompt-round planning
- PM feedback packaging
- cross-role coordination
- readiness gating
- final package audit and repair

For a more detailed overview, see:
- `references/skill-capability-map.md`

---

## Reference guide

Start with:
- `references/index.md`

That file tells you which reference to use first depending on the scenario.

Common high-value references:
- `references/discovery-checklist.md`
- `references/prd-template.md`
- `references/change-prd-template.md`
- `references/new-system-guidance.md`
- `references/existing-system-delta-guidance.md`
- `references/internal-platform-eval-guidance.md`
- `references/multi-agent-delivery-guidance.md`
- `references/prompt-round-guidance.md`
- `references/multi-agent-handoff-template.md`
- `references/agent-prompt-round-template.md`
- `references/pm-feedback-template.md`
- `references/doc-package-sequencing-template.md`
- `references/readiness-checklist-template.md`
- `references/final-doc-review-checklist.md`
- `references/output-quality-rules.md`

Recommended mental grouping:

### Core planning references
- `references/discovery-checklist.md`
- `references/prd-template.md`
- `references/change-prd-template.md`
- `references/api-spec-template.md`
- `references/plan-mode-playbook.md`

### Scenario guidance references
- `references/new-system-guidance.md`
- `references/existing-system-delta-guidance.md`
- `references/internal-platform-eval-guidance.md`

### Delivery and orchestration references
- `references/multi-agent-delivery-guidance.md`
- `references/multi-agent-handoff-template.md`
- `references/prompt-round-guidance.md`
- `references/agent-prompt-round-template.md`
- `references/pm-feedback-template.md`
- `references/cross-role-interface-template.md`

### Quality and transition references
- `references/doc-package-sequencing-template.md`
- `references/readiness-checklist-template.md`
- `references/final-doc-review-checklist.md`
- `references/output-quality-rules.md`

---

## End-to-end example

If you want to see how the skill can be used across a full planning lifecycle, see:
- `references/end-to-end-example.md`

This example walks through:
- vague idea intake
- clarification
- checkpointing
- PRD generation
- supporting docs
- role handoff docs
- prompt rounds
- PM review and feedback packaging
- readiness gating
- final document audit and repair
- execution transition

---

## Scope note

This is a **general planning skill**, not a skill only for internal platforms or only for test/eval systems.

Some guidance sections are more specialized because they capture recurring high-value scenarios, but they are accelerators, not the definition of the whole skill.
