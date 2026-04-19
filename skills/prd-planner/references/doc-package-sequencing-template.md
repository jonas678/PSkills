# Document Package Sequencing Template

Use this template when the user needs more than a single PRD and the planning work must be turned into a sequenced document package.

This template answers questions such as:
- what documents should be produced first
- which documents are mandatory versus optional
- which documents are prerequisites for others
- which documents can be created in parallel
- when to stop writing more docs and start execution handoff

This template is especially useful for:
- 0-to-1 new applications
- internal tools and platforms
- multi-role or multi-agent product delivery
- PM-led planning workflows that move from requirement discovery into implementation preparation

---

## 1. Core principle

Do not generate a large document bundle all at once just because the user asks for “complete docs.”

Instead, sequence the package based on dependency and usefulness.

A good document sequence should:
- stabilize product boundary first
- define user-facing and business-facing requirements next
- then define data / API / execution details
- then define role handoff materials
- then define delivery and validation materials

---

## 2. Default sequencing model

For most projects, a strong default sequence is:

### Stage 1: Discovery and boundary definition
Output:
- requirement intake summary
- decision checkpoint
- open questions list

Goal:
- confirm what is being built
- confirm who it is for
- confirm scope and non-goals

### Stage 2: Core product definition
Output:
- main PRD
- MVP / phase split if needed

Goal:
- define the product clearly enough that downstream documentation is stable

### Stage 3: Functional expansion package
Output as needed:
- page requirements
- user-flow details
- business rules appendix
- acceptance checklist

Goal:
- expand the PRD into role-usable product detail

### Stage 4: Technical expansion package
Output as needed:
- data model and API
- backend technical design
- execution or service flow design
- compatibility / migration notes for existing systems

Goal:
- make the product implementable, not just understandable

### Stage 5: Execution handoff package
Output as needed:
- Designer brief
- Engineering brief
- QA brief
- prompt templates
- PM review playbook

Goal:
- transfer work cleanly into downstream roles or agents

### Stage 6: Delivery and execution package
Output as needed:
- Jira breakdown
- milestone plan
- release / acceptance checklist
- rollout notes

Goal:
- move from planning into execution management

---

## 3. Mandatory vs optional document types

### Usually mandatory for a serious new build
- main PRD
- open questions tracker
- acceptance criteria or acceptance checklist

### Usually strongly recommended
- page requirements or user-flow expansion
- data model and API
- technical design for non-trivial backend systems

### Recommended when multiple roles or agents are involved
- designer brief
- engineering brief
- QA brief
- prompt templates
- PM orchestration / review playbook

### Optional depending on project maturity
- Jira ticket plan
- release checklist
- migration plan
- review deck
- monitoring / rollout appendix

Do not force every project to produce every document.

---

## 4. Dependency map

### Typical dependencies

| Document | Usually depends on | Enables |
|---|---|---|
| Open questions list | initial intake | scope clarification, PRD stabilization |
| Main PRD | discovery checkpoint | all downstream docs |
| Page requirements | main PRD | design, frontend planning, QA coverage |
| Data model and API | main PRD | backend planning, frontend/backend alignment, QA |
| Technical design | main PRD + core object model | backend execution planning |
| Designer brief | main PRD + page requirements | design output |
| Engineering brief | main PRD + technical docs | engineering planning |
| QA brief | main PRD + acceptance + technical semantics | QA strategy |
| Prompt templates | role briefs + document package | multi-agent execution |
| PM review playbook | role briefs + prompt model | multi-round orchestration |
| Jira breakdown | stabilized scope + technical split | execution tracking |

### Rule of thumb
Do not produce a downstream doc if its upstream dependency is still highly unstable.

---

## 5. Parallelization guidance

Some docs can be created in parallel after the right prerequisite is stable.

### Common safe parallel patterns
After the main PRD is stable:
- page requirements and data/API can often proceed in parallel
- designer brief and engineering brief can often proceed in parallel
- QA brief can often start in parallel if product semantics are stable enough

### Common unsafe parallel patterns
Avoid parallelizing too early:
- designer brief before core flows are defined
- technical design before object model is stable
- Jira breakdown before scope is sufficiently split
- QA detailed test cases before states and flows are stable

---

## 6. Sequencing guidance for 0-to-1 new applications

For a typical new application, a strong default order is:

1. discovery summary and open questions
2. main PRD
3. page requirements
4. data model and API
5. backend / execution technical design
6. acceptance checklist
7. designer / engineering / QA briefs
8. prompt templates
9. PM review playbook
10. Jira / milestone execution docs

### Why this order works
- product boundary stabilizes before detailed docs expand
- design and engineering briefs are based on real product definition, not guesses
- prompts are built on top of role briefs instead of replacing them
- Jira planning happens after scope and ownership are visible

---

## 7. Sequencing guidance for existing-system change work

For change PRDs, do not blindly follow the same sequence as greenfield work.

A better default order is:
1. current-state summary
2. delta PRD
3. impacted module and compatibility analysis
4. frontend / backend / data change docs only for the affected areas
5. rollout / migration / QA delta coverage
6. role handoff docs if the change is large enough to require them

### Key rule for existing systems
Only expand the changed surfaces.
Do not regenerate a full greenfield-like package unless the user explicitly wants a full rewrite.

---

## 8. Stage exit criteria

### Stage 1 is done when
- the product goal is clear
- users and roles are clear
- core scenarios are clear
- major scope ambiguity is reduced

### Stage 2 is done when
- the main PRD is coherent
- MVP scope is visible
- core object model is stable enough for downstream docs

### Stage 3 is done when
- user-facing flows and page-level expectations are understandable
- acceptance framing is usable by design / frontend / QA

### Stage 4 is done when
- implementation-relevant technical structure is clear enough
- API / data / execution semantics are stable enough to plan against

### Stage 5 is done when
- each downstream role knows what to read, what to output, and what not to redefine
- prompts and PM coordination materials exist if needed

### Stage 6 is done when
- planning can transition into task execution, review, and validation tracking

---

## 9. Minimal package recommendations by project type

### Small feature inside an existing product
Recommended minimum:
- delta PRD
- impacted page / API notes
- acceptance criteria

### Medium feature or workflow redesign
Recommended minimum:
- main or delta PRD
- page requirements
- data / API appendix
- acceptance checklist

### Large new internal platform or tool
Recommended minimum:
- main PRD
- page requirements
- data model and API
- technical design
- acceptance checklist
- designer / engineering / QA briefs
- prompt templates
- PM review playbook

---

## 10. Stop conditions

The document package is probably sufficient when:
- downstream roles no longer need the PM to repeatedly restate basics
- major object relationships are stable
- major states and boundaries are explicit
- handoff documents explain role expectations clearly
- further documentation would mostly duplicate existing material

At that point, prefer moving into execution rather than continuing to expand docs.

---

## 11. Common failure modes

Avoid these mistakes:
- producing too many docs before the main PRD is stable
- skipping acceptance framing until too late
- generating role handoff docs before product boundaries are clear
- producing Jira breakdown before module ownership is visible
- expanding every document for small changes that only affect one surface
- continuing to write docs when the real bottleneck is now review or execution

---

## 12. Quick sequencing checklist

Before producing the next document, check:
- [ ] is the upstream dependency stable enough?
- [ ] is this document necessary now or only nice to have?
- [ ] will this document unlock a downstream role or decision?
- [ ] can this be safely created in parallel with another doc?
- [ ] are we still in planning, or should we now transition to execution handoff?
