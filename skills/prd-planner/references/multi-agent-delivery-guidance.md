# Multi-Agent Delivery Guidance

Use this reference when the user is not only asking for a PRD, but also needs downstream execution materials for multiple roles such as Designer, Engineering, QA, or PM.

This guidance helps prevent a common failure mode:
- generating only a narrative PRD
- leaving the PM to manually translate it into role-specific instructions
- failing to define who can start, what each role should read, and what each role should not redefine

---

## 1. When to use this reference

Use this reference when the user asks for:
- materials for designer, frontend, backend, database, QA, or PM
- handoff docs
- “what should I send to the team”
- “can engineering start now”
- prompt templates for downstream roles
- PM review workflow across multiple agents

Do not use this as the only guidance when the product definition itself is still unstable.
First stabilize the core requirement checkpoint and main PRD.

---

## 2. Core principle

When downstream delivery is required, do not stop at only a main PRD.

Expand planning into role-usable materials:
- role-specific briefs
- prompts
- PM orchestration guidance
- readiness or freeze documentation when needed

The goal is not just “documentation exists.”
The goal is:
- each role knows what to read
- each role knows what to produce
- each role knows what not to redefine
- PM can review and run iterative collaboration cleanly

---

## 3. Recognizing the handoff moment

You are likely in handoff mode when:
- the user has already confirmed major scope decisions
- the user asks about design, frontend, backend, QA, or PM next steps
- the user asks how multiple agents should collaborate
- the user asks whether the package is enough for the team
- the user asks for prompt rounds or role prompts

At that point, planning should transition from “define the product” to “package the product for execution.”

---

## 4. Default role model

Unless the user explicitly wants a different split, prefer this top-level model:
- Designer Agent
- Engineering Agent
- QA Agent
- PM as orchestrator and reviewer

Database work can often remain inside Engineering unless the user explicitly wants a separate top-level Database role.

Avoid forcing unnecessary top-level roles if the user’s management model is simpler.

---

## 5. Required handoff package

For a substantial new app or large feature effort, a strong delivery package may include:
- main PRD
- page requirements
- data model / API appendix
- technical design appendix
- open questions tracker
- acceptance checklist
- review materials
- Designer brief
- Engineering brief
- QA brief
- prompt documents
- PM orchestration / review playbook
- readiness / freeze or final review docs when relevant

Not every project needs every item, but once the user wants execution handoff, one PRD alone is usually not enough.

---

## 6. What each role needs to know

### 6.1 Designer
Designer usually needs:
- product framing
- core objects and key relationships
- page inventory and P0 flows
- important states
- interaction constraints
- what still needs PM confirmation

### 6.2 Engineering
Engineering usually needs:
- product boundaries that must not change
- object model and execution semantics
- state semantics
- API / data / tech design inputs
- implementation sequencing
- risks and dependencies
- what remains open versus confirmed

### 6.3 QA
QA usually needs:
- state semantics and failure semantics
- acceptance framing
- high-risk scenarios
- what must be validated early versus later
- key system credibility checks

### 6.4 PM
PM usually needs:
- recommended send order
- role-specific reading order
- review criteria
- second-round packaging method
- readiness and freeze checkpoints

---

## 7. What each brief should contain

### 7.1 Designer brief
Should clarify:
- project framing
- required reading order
- core objects
- P0 pages and flows
- important states and UX constraints
- output format expectations
- what still needs PM confirmation

### 7.2 Engineering brief
Should clarify:
- reading order
- product boundaries that must not be changed
- core execution semantics
- module boundaries
- role split across frontend / backend / database when relevant
- implementation sequence
- risks, dependencies, and open decisions

### 7.3 QA brief
Should clarify:
- reading order
- state semantics and failure semantics
- main risk model
- P0 test dimensions
- what can start now at strategy level
- what depends on later implementation detail

---

## 8. Role boundary rules

Make it explicit that downstream roles should not silently redefine:
- product scope
- core object relationships
- state semantics
- pass / fail semantics
- ownership model
- frozen MVP boundary

If a downstream role believes a confirmed decision is wrong or incomplete, that should be surfaced as:
- conflict
- risk
- recommendation
- confirmation request

not silently rewritten as if it were newly confirmed truth.

---

## 9. PM orchestration guidance

When the user is acting as PM, provide explicit advice on:
- who can start first
- who can start in parallel
- what depth each role should target in round 1
- what PM should review for each role
- how to turn first-round outputs into second-round inputs

A common default:
- Designer and Engineering can usually start round 1 in parallel
- QA can also start in round 1, but should focus on strategy, state semantics, and risk framing rather than final validation detail

---

## 10. Automated Session & Team Execution (Claude Code)

If the user is executing this handoff inside Claude Code, you can automate the entire multi-round process using the **Agent Teams (Swarm)** functionality, rather than just printing prompts.

When the handoff package is ready:
1. Propose setting up a dedicated Team for this feature using **TeamCreate**.
2. **TeamCreate**: Create the team (e.g. `team_name: "feature-delivery-team"`).
3. **Agent tool**: Spawn Designer, Engineering, and QA as named teammates (`team_name`, `name`, `subagent_type: "general-purpose"`). Pass the Round 1 direct-copy prompts as their initialization parameters. Use `run_in_background: true` so the PM isn't blocked.
4. This creates a distinct, stateful background session for each agent. It isolates implementation details away from the PM-planning session while preserving the ability for multi-round interaction.
5. **Multi-round Communication**: When the agents complete their round 1 prompts, they will automatically send idle notifications back to you (the PM). At that point, review their local output documents, generate PM Feedback, and use the **SendMessage** tool to send the Round 2 prompt instructions to each agent specifically by their assigned name.
6. When the delivery package is completely verified, send a `{type: "shutdown_request"}` to all agents and invoke **TeamDelete**.

---

## 11. Common failure modes

Watch for these mistakes:
- generating only a PRD when role handoff is clearly needed
- splitting roles in a way that conflicts with the user’s actual operating model
- giving generic prompts with no reading order or output contract
- starting QA too late
- letting prompts reopen already confirmed product decisions
- leaving PM with no clear review structure

---

## 12. Suggested next references

After using this reference, commonly useful next references are:
- `multi-agent-handoff-template.md`
- `agent-prompt-round-template.md`
- `pm-feedback-template.md`
- `cross-role-interface-template.md`
- `readiness-checklist-template.md`
- `final-doc-review-checklist.md`
