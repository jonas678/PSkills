# Multi-Agent Handoff Template

Use this template when the PRD is already sufficiently stable and the next step is to hand work off to multiple downstream roles or AI agents such as Designer, Engineering, QA, and PM orchestration.

This template is especially useful for:
- 0-to-1 new applications
- internal tools and platforms
- products with clear designer / engineering / QA handoffs
- PM-led multi-agent collaboration workflows

Do not use this template too early when the product boundary is still highly unclear. First finish requirement clarification and core PRD stabilization.

---

## 1. Usage guidance

Before using this template, make sure the following already exist or are close to stable:
- main PRD
- page-level requirements or user-flow definition
- data model and API appendix if applicable
- technical design notes for backend or execution logic if applicable
- acceptance checklist or at least MVP acceptance criteria

If those are not ready, return to requirement clarification first.

---

## 2. Recommended output package

For a typical multi-agent execution handoff, the recommended document package is:

1. Main PRD
2. Designer Brief
3. Engineering Brief
4. QA Brief
5. Agent Prompt Document
6. PM Review / Orchestration Playbook

Depending on the project, you may also add:
- Jira ticket planning doc
- acceptance / release checklist
- open questions tracker
- change PRD delta appendix for existing systems

---

## 3. Role model recommendation

### Preferred default grouping
Use this grouping unless the user wants a different model:
- Designer Agent
- Engineering Agent
  - Frontend
  - Backend
  - Database
- QA Agent
- PM as orchestrator

### When to split roles further
Split into more documents only when:
- the user explicitly wants separate role outputs
- the database or infrastructure scope is independently owned
- the frontend and backend work are large enough to require separate prompts and separate ownership

Avoid over-splitting too early.

---

## 4. Designer Brief template

### Document purpose
Explain what the designer is expected to produce and how the design output will be used by PM and frontend.

### Suggested structure

#### 4.1 Role positioning
- You are the design owner for this phase.
- Your output will be reviewed by PM before frontend implementation.

#### 4.2 Required reading order
List required docs in order. Example:
1. main PRD
2. page requirements
3. acceptance checklist
4. selected data model / API sections if needed

#### 4.3 Product framing
Clarify:
- product type
- target users
- key jobs to be done
- internal tool vs external product

#### 4.4 Core object model to understand
List the business objects and relationships the designer must understand before drawing screens.

#### 4.5 P0 design scope
List the key pages and flows.

#### 4.6 Required design principles
Examples:
- You **MUST invoke the `/frontend-design` skill** to produce your visual artifacts and code.
- Emphasize bold, non-generic aesthetics per the `/frontend-design` guidelines.
- prioritize clarity over decoration
- prioritize task efficiency and error recovery
- include empty / loading / error / validation / no-data states
- preserve product logic and object relationships

#### 4.7 Required output
Specify what the designer should produce in this round. Example:
- Production-grade HTML/CSS/JS prototypes or React/Vue components (generated via the `/frontend-design` skill).
- information architecture
- key interaction notes
- states coverage
- open decision list for PM

#### 4.8 Explicit “do not decide alone” section
List items that must go back to PM.

---

## 5. Engineering Brief template

### Document purpose
Explain that this brief is for execution planning and implementation alignment, not for re-defining the product.

### Suggested structure

#### 5.1 Role positioning
- You are the engineering owner for implementation planning.
- You may split the work across frontend / backend / database.

#### 5.2 Required reading order
List core docs in order. Example:
1. main PRD
2. data model and API
3. backend / execution technical design
4. task breakdown
5. acceptance checklist
6. open questions

#### 5.3 Confirmed product boundaries
List product semantics that must not be changed.
Examples:
- core object relationships
- run status semantics
- result-generation semantics
- scope boundaries

#### 5.4 Implementation Workflow (SDD & TDD)
Specify the expected engineering workflow, especially if the project uses structured dev skills.
Examples:
- **Phase 1 (Design):** Read the PRD and generate a Software Design Document (SDD) detailing architecture, data models, and API interfaces.
- **Phase 2 (Test & Code):** Follow Test-Driven Development (TDD). Write unit/integration tests based on the SDD and PRD acceptance criteria first, then implement the code to pass the tests.

#### 5.5 Role split
Define responsibilities for:
- Frontend
- Backend
- Database

#### 5.6 Round objective
Specify whether this round is for:
- module breakdown
- implementation sequence
- interface alignment
- dependency identification
- risk identification

#### 5.6 Required output
Examples:
- module ownership
- implementation order
- dependency map
- API / data / execution alignment notes
- risk list
- PM confirmation list

#### 5.7 Explicit “do not change” section
List what engineering should not redefine.

---

## 6. QA Brief template

### Document purpose
Explain that QA should validate product behavior, state semantics, system reliability, and result trustworthiness rather than only happy-path UI interaction.

### Suggested structure

#### 6.1 Role positioning
- You are the QA owner for this phase.
- Your output should help define strategy first, then detailed coverage later.

#### 6.2 Required reading order
Example:
1. main PRD
2. page requirements
3. data model and API
4. technical design
5. acceptance checklist
6. open questions

#### 6.3 Core semantics to preserve
List the states, failure categories, and result semantics QA must understand.

#### 6.4 Round objective
Usually round 1 should focus on:
- test strategy
- risk model
- state transition coverage
- abnormal scenario coverage
- acceptance framing

#### 6.5 Required QA output
Examples:
- testing scope
- test dimensions
- P0 test strategy
- state transition cases
- abnormal case list
- regression framing
- issues requiring PM or engineering clarification

#### 6.6 Common QA risks to emphasize
Examples:
- confusing placeholder records with real results
- summary/detail mismatch
- incorrect failure categorization
- result comparison mismatch across targets

---

## 7. Agent Prompt template

This document should give direct-copy prompts to each downstream agent.

### Prompt set recommendation
Prefer organizing prompts by round:
- round 1 prompt
- round 2 prompt
- optional round 3+ prompts if needed

### Round 1 prompt should usually include
- role identity
- required reading list
- confirmed facts
- current objective
- output format
- constraints on changing scope

### Round 2 prompt should usually include
- round-1 output summary
- PM feedback
- confirmed items not to reopen
- current revision target
- what not to repeat
- delta-focused output structure

### Important anti-pattern
Do not provide only one timeless generic prompt when the work is clearly iterative.

---

## 8. PM Review / Orchestration Playbook template

### Document purpose
Help the PM actually operate a multi-agent workflow instead of only reading documents.

### Suggested structure

#### 8.1 Recommended send order
State who should start first and who can run in parallel.

#### 8.2 Round-1 expectations by role
Define the expected depth for:
- Designer
- Engineering
- QA

#### 8.3 PM review criteria by role
Define what PM should review for each role’s first-round output.

#### 8.4 PM review output format
Recommend organizing PM review notes into:
- confirmed items
- revision items
- open decision items

#### 8.5 Second-round packaging instructions
Specify how to turn round-1 results into round-2 prompt inputs.

#### 8.6 Common mistakes
Examples:
- asking for final output too early
- failing to structure PM feedback
- reopening already confirmed decisions
- starting QA too late or too deeply

---

## 9. Recommended sequence for a 0-to-1 project

For a typical new application, a strong default sequence is:

1. stabilize PRD and key appendices
2. create Designer / Engineering / QA briefs
3. create round-1 direct-copy prompts
4. run Designer and Engineering in parallel
5. run QA in parallel with strategy-focused scope
6. PM reviews round-1 outputs
7. package confirmed / revision / open items
8. send round-2 prompts
9. continue toward implementation and validation

---

## 10. Automated Session & Team Execution (Claude Code)

When the user is ready to execute the handoff in Claude Code, do not simply print the prompts for manual copy-pasting.

Instead, offer to set up an **Agent Team (Swarm)** to coordinate the roles automatically:
- Use the **TeamCreate** tool to create a new team (e.g., `team_name: "feature-delivery-team"`, `description: "Delivery of new feature"`).
- Use the **Agent** tool to spawn each role (Designer, Engineering, QA) in parallel blocks. Critically, pass the `team_name` parameter and a distinct `name` (e.g., `name: "qa-agent"`) to join them to the newly created team. Pass the direct-copy prompts as the `prompt` parameter.
- Use `run_in_background: true` so the PM (Main Session) can continue orchestrating.
- **Mult-round Interaction:** Once the agents complete their initial task, they will go "idle" and automatically notify the PM session. The PM can then review their outputs and use the **SendMessage** tool (`to: "qa-agent"`) to give follow-up feedback (Round 2 prompts) without losing their isolated context.
- When all work is done and verified, use **SendMessage** with `message: {"type": "shutdown_request"}` to cleanly terminate the teammates, followed by **TeamDelete**.

---

## 11. Output checklist

Before you consider the handoff package complete, check:
- [ ] main PRD exists
- [ ] designer brief exists
- [ ] engineering brief exists
- [ ] QA brief exists
- [ ] direct-copy prompts exist
- [ ] PM review playbook exists
- [ ] role reading order is explicit
- [ ] confirmed product boundaries are explicit
- [ ] round-1 target depth is controlled
- [ ] round-2 workflow is defined

---

## 12. Common failure modes

Watch for these mistakes:
- generating only the PRD and forgetting execution handoff docs
- over-splitting roles before ownership is clear
- under-specifying required reading order
- allowing role briefs to change confirmed product logic
- missing PM review guidance
- giving generic prompts with no round structure
- assuming QA should only start after implementation
- failing to separate confirmed items from open decision items
