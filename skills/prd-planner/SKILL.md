---
name: prd-planner
description: Guide requirement discovery, product brainstorming, scope clarification, PRD generation, and team-ready handoff package creation for new systems, new features, existing-system changes, and PM-led multi-agent delivery workflows. Use when the user wants to design a new system, define or optimize a feature, clarify vague requirements, expand into page/data/API/technical docs, or prepare designer/engineering/QA handoff materials and prompt rounds.
---

# PRD Planner

Turn vague product requests into structured planning outputs that can progress from discovery to PRD, supporting documents, team handoff, and execution readiness.

## Mission and Scope

`prd-planner` is a general planning and handoff skill.

Use it when the user needs help with one or more of the following:
- clarifying a vague product idea
- defining a new system or workflow
- planning a feature for an existing system
- improving or optimizing an existing experience
- expanding a PRD into page, data, API, or technical documents
- preparing team-ready or agent-ready handoff materials
- organizing PM-led multi-agent collaboration

This skill is **not only a PRD drafting helper**. It can support the full path from:
- discovery
- requirement checkpointing
- PRD generation
- supporting document expansion
- handoff package creation
- prompt-round planning
- readiness assessment
- final document audit before handoff

Some guidance paths are scenario-specific accelerators, such as internal platforms, eval tools, or multi-agent delivery. These are important scenarios, but they do not define the entire scope of the skill.

## Core Operating Rules

- Act like a planning partner first and a document writer second.
- Do not jump straight to a full PRD when the requirements are still vague.
- Ask focused questions in small batches.
- Separate confirmed facts, assumptions, open questions, and recommendations.
- Use checkpoints before large document expansion.
- For existing-system requests, work delta-first: clarify current state, target state, unchanged scope, and compatibility constraints before writing the target plan.
- When the user needs team-ready execution materials, do not stop at a narrative PRD.
- Prefer structured, implementation-usable outputs over generic prose.
- Before declaring a multi-document package complete, run a final package-level audit and repair pass.

## Default Workflow

Follow this default workflow unless the user asks for a different format:

1. **Classify the request**
   - Determine whether the task is mainly about a new system, an existing-system change, an optimization, a redesign, or a handoff package.

2. **Run initial requirement intake**
   - Use normal chat first to understand the goal, users, pain points, expected outcome, and constraints.

3. **Clarify in structured rounds**
   - Ask high-impact questions first.
   - Prefer short rounds with explicit choices when the request is still vague.

4. **Produce a requirement checkpoint**
   - Summarize current understanding before writing a full PRD.
   - Make scope, assumptions, risks, and open questions explicit.

5. **Generate the main PRD**
   - Only after enough clarity is reached.
   - Split into MVP / later phases when useful.

6. **Expand supporting documents if needed**
   - Add page requirements, data / API docs, technical appendices, acceptance checklists, review materials, and related artifacts when the project requires them.

7. **Generate handoff and prompt materials if needed**
   - Add designer / engineering / QA briefs, prompt rounds, and PM review materials when downstream collaboration is part of the task.

8. **Automate team execution (New Session Handoff & Swarm Teams)**
   - If the user asks to start the work or execute the handoff, use the `TeamCreate` and `Agent` tools to spawn the QA, Engineering, and Designer agents into a coordinated Team (Swarm).
   - This creates a fresh, isolated, long-running session for each role.
   - Use `run_in_background: true` when spawning multiple roles in parallel so the main session (PM) remains responsive.
   - Provide each agent with their generated role-specific prompt.
   - **Multi-round Interaction**: Await their idle notifications, review their output files, and use the `SendMessage` tool (by agent name) to provide Round 2/3 feedback while preserving their stateful sub-session context.
   - Cleanly terminate teammates and the team when work is fully completed using `SendMessage` `{type: "shutdown_request"}` and `TeamDelete`.

9. **Assess readiness**
   - Check whether the package is sufficiently stable for design, engineering, QA, PM handoff, or execution.

10. **Run final document audit and repair**
   - Review the generated package for completeness, consistency, stale assumptions, terminology drift, semantic drift, and handoff readiness.
   - Repair low-risk issues directly; escalate high-risk conflicts for confirmation.

## Setup & Configuration

For the **Automated Session & Team Execution** to work, Claude Code requires an experimental flag to be set in your settings.

When a user asks to `setup` this skill or asks how to enable Swarm Teams, you must:
1. Check the user's project `.claude/settings.json` file.
2. Ensure the `env` object contains `"CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"`.
3. If it does not exist, use the `update-config` skill or file edit tools to add it.
4. Let the user know they may need to restart Claude Code if the setting was just added.

## Scenario Router

Use the relevant references below depending on the task.

### New system / 0-to-1
Use:
- `references/new-system-guidance.md`

Use this path when:
- the user is defining a brand-new system
- the core object model is not yet stable
- the system boundary still needs to be shaped
- MVP vs future scope is not yet clear

### Existing-system change / optimization
Use:
- `references/existing-system-delta-guidance.md`
- `references/change-prd-template.md`

Use this path when:
- the user is changing an existing feature
- the request is an optimization, refactor, redesign, or migration-aware change
- compatibility and unchanged scope matter

### Multi-agent / multi-role delivery handoff
Use:
- `references/multi-agent-delivery-guidance.md`
- `references/multi-agent-handoff-template.md`

Use this path when:
- the user needs materials for designer, engineering, QA, PM, or other downstream roles
- the user asks what to send to the team
- the user asks whether engineering can start now
- the work must leave planning and enter coordinated execution

### Prompt rounds and PM-led iteration
Use:
- `references/prompt-round-guidance.md`
- `references/agent-prompt-round-template.md`
- `references/pm-feedback-template.md`

Use this path when:
- the user wants role-specific prompts
- downstream work will happen in multiple rounds
- PM review needs to be packaged into second-round inputs

### Internal platform / eval / test system scenario
Use:
- `references/internal-platform-eval-guidance.md`

Use this path when:
- the product is an internal platform, QA tool, eval system, or test orchestration product
- execution semantics, evaluator logic, result models, and black-box vs white-box scope need clarification

### Final package review
Use:
- `references/final-doc-review-checklist.md`

Use this path when:
- the user asks whether the docs are enough
- the package is about to be handed to design, engineering, QA, or PM
- multiple documents were generated over several rounds and may have drifted

## Output Expectations

Unless the user asks for a different format:
- keep outputs structured
- mark assumptions explicitly
- mark recommendations explicitly
- make open questions visible
- distinguish user-facing flow, business rules, and technical behavior
- prefer outputs that are usable by product, design, frontend, backend, QA, and PM when relevant

When the project requires team-ready output, expand beyond a single PRD into a document package such as:
- PRD main document
- page requirements
- data model and API appendix
- technical design appendix
- open questions tracker
- acceptance checklist
- review materials
- role briefs
- prompt materials
- PM review / orchestration materials

When expanding outputs to implementation-usable detail, use:
- `references/prd-template.md`
- `references/change-prd-template.md`
- `references/api-spec-template.md`
- `references/output-quality-rules.md`

## Final Audit Rule

Before declaring a multi-document package complete:
- check completeness against the intended package
- check terminology consistency
- check state and status semantics consistency
- check scope boundary consistency
- check whether prompts and handoff docs still reflect the latest confirmed decisions
- check whether the package is actually ready for the next role to start

Repair low-risk inconsistencies directly.

If the inconsistency would change:
- product scope
- execution semantics
- ownership
- acceptance meaning
- role boundaries

then surface it as a confirmation item instead of silently rewriting it.

Use:
- `references/final-doc-review-checklist.md`

## Writing Rules

- Be specific and implementation-oriented.
- Prefer tables and bullet points over vague paragraphs.
- Mark uncertain items as `To be confirmed`.
- Mark assumptions as `Assumption`.
- Mark your recommended solution as `Recommendation`.
- Do not present recommendations as confirmed requirements.
- If the user asks for brainstorming only, stop before the full PRD and provide a clear next-step summary.
- After the initial intake summary, continue the next collaboration round in `/plan` mode when available; otherwise continue with explicitly structured checkpoints in chat.
- If terminology is ambiguous, reflect the ambiguity back and resolve it before finalizing the structure.

## Resource Guide

Use the bundled references when useful.

### Core planning
- `references/discovery-checklist.md`
- `references/prd-template.md`
- `references/change-prd-template.md`
- `references/api-spec-template.md`
- `references/plan-mode-playbook.md`

### Scenario-specific guidance
- `references/new-system-guidance.md`
- `references/existing-system-delta-guidance.md`
- `references/internal-platform-eval-guidance.md`

### Delivery and orchestration
- `references/multi-agent-delivery-guidance.md`
- `references/multi-agent-handoff-template.md`
- `references/prompt-round-guidance.md`
- `references/agent-prompt-round-template.md`
- `references/pm-feedback-template.md`
- `references/cross-role-interface-template.md`

### Readiness and final review
- `references/doc-package-sequencing-template.md`
- `references/readiness-checklist-template.md`
- `references/final-doc-review-checklist.md`

### Output quality, overview, and examples
- `references/output-quality-rules.md`
- `references/skill-capability-map.md`
- `references/index.md`
- `references/end-to-end-example.md`
