# Plan Mode Playbook

Use this playbook after the initial requirement intake is complete.

## Goal of the `/plan` stage

Use `/plan` mode to turn an early idea into a converged and implementation-ready requirement set before writing the final PRD.

The `/plan` stage should help the user:

- compare solution directions
- close major scope gaps
- identify business and technical assumptions
- define MVP vs later phases
- clarify frontend and backend responsibilities
- prepare a reliable PRD structure

## Entry rule

After the first intake summary in normal chat, continue the next planning round in `/plan` mode by default.

Use normal chat only for the first intake or when plan mode is truly unavailable.

## Recommended `/plan` sequence

### Round 1: Confirm planning frame

In the first `/plan` round, confirm:

- project goal
- target users
- top scenarios
- success criteria
- current known constraints

Good outputs for this round:

- short requirement summary
- open question list
- decision list
- first-cut scope boundary

### Round 2: Explore and compare options

Use this round when the user is not yet sure how the feature or system should work.

Focus on:

- workflow options
- permission model options
- frontend information architecture options
- backend orchestration options
- trade-offs between simpler MVP and fuller design

Good outputs for this round:

- Option A / Option B comparison
- recommended direction
- assumptions that must be confirmed
- risks by option

### Round 3: Close requirement gaps

Use this round to eliminate ambiguity before writing the PRD.

Prioritize closing:

- role permissions
- state transitions
- exception paths
- notification rules
- integrations
- reporting and audit needs
- operational edge cases

Good outputs for this round:

- confirmed scope list
- to-be-confirmed list
- dependency list
- impacted module list

### Round 4: Prepare PRD structure

Before drafting the final PRD, produce a structured checkpoint with:

- confirmed goals
- confirmed roles
- key scenarios
- functional scope
- frontend pages or touchpoints
- backend APIs or services
- change impact summary
- unresolved items

Ask for confirmation when there are still meaningful unknowns.

### Round 5: Draft the final PRD

After enough clarity is reached, generate the PRD.

The PRD should be detailed enough for:

- product review
- design preparation
- frontend estimation
- backend estimation
- QA planning
- project scoping

## Questioning rules inside `/plan`

- Ask only the highest-value unanswered questions.
- Prefer grouped questions by theme.
- Keep each round concise.
- Summarize before asking the next set.
- Convert vague user goals into explicit options when helpful.
- Mark unknowns clearly instead of hiding ambiguity.

## Suggested `/plan` question packs

### Pack A: Product and scope
- What is the business objective?
- Who is the primary user?
- What is the key action the user must complete?
- What is in MVP and what is not?

### Pack B: Frontend flow
- What is the entry point?
- What pages or views are required?
- What is the ideal step-by-step journey?
- What failure or empty states matter most?

### Pack C: Backend design
- What data entities are involved?
- What APIs are required?
- What validations and permissions apply?
- Which services need to be called synchronously or asynchronously?

### Pack D: Delivery and risk
- What dependency could block launch?
- Which part is highest risk?
- What can be postponed to phase 2?
- What requires explicit business confirmation?

## Output checkpoint before PRD

Before writing the final PRD, make sure `/plan` has produced these artifacts:

- confirmed requirement summary
- open question list
- scope boundary
- frontend flow outline
- backend interface outline
- impacted module list
- MVP recommendation

If these artifacts are not ready, keep working in `/plan` before drafting the PRD.
