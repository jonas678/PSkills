# New System Guidance

Use this reference when the request is for a brand-new system, product, workflow, or platform and the core object model or scope boundary is not yet stable.

This guidance helps prevent a common failure mode:
- the conversation moves too quickly into writing a PRD
- the visible UI model is mistaken for the real system model
- reusable assets are attached to the wrong parent object
- MVP and future scope become mixed together

---

## 1. When to use this reference

Use this reference when:
- the system does not already exist in a meaningful form
- the user is starting from a rough idea
- the primary user, object model, or product boundary is still unclear
- the request is “from scratch”, “0-to-1”, “design a new tool”, or similar

Do not use this as the primary guidance for:
- existing-system feature changes
- optimization-only requests
- compatibility-driven changes
- migration planning on an existing product

For those cases, prefer:
- `existing-system-delta-guidance.md`
- `change-prd-template.md`

---

## 2. Core principle

For a new system, do not assume the product boundary or object model is already obvious.

Before writing a full PRD, explicitly clarify:
- what the system is
- who primarily uses it
- what the core objects are
- what relationships exist between those objects
- what the MVP is trying to prove or enable
- what is intentionally out of scope for the first phase

The planning goal is not only to describe screens or features. The real goal is to stabilize the system model first.

---

## 3. What must be clarified first

Before expanding into a full PRD, try to establish:

### 3.1 System framing
- What kind of system is this?
- Is it a user-facing product, internal tool, platform capability, workflow layer, or operational system?
- What job does it do?

### 3.2 Primary user and operator
- Who is the primary user?
- Who configures it?
- Who consumes its outputs?
- Is the main user the same as the operator?

### 3.3 Core objects
- What are the main entities in the system?
- Which object is the system centered around?
- Which objects are reusable assets?
- Which objects are execution records or outputs rather than configuration assets?

### 3.4 MVP goal
- What must the MVP prove, enable, or unblock?
- What is the minimum closed loop?
- What is important but still deferrable?

### 3.5 Scope boundary
- What is explicitly in scope?
- What is explicitly not in scope?
- What belongs to a future phase?

---

## 4. Object-model-first questions

When the object model is still fuzzy, prioritize questions like these:

- What is the user really creating, editing, configuring, or analyzing?
- What can be reused across multiple runs, projects, targets, or users?
- What is an execution container versus a reusable asset?
- Which relationships are one-to-many, many-to-many, or snapshots?
- Which records are historical outputs rather than live definitions?

If the user is describing pages before the objects are stable, step back and ask:
- “What are the actual system entities behind these screens?”
- “Which objects should remain independent from each other?”

---

## 5. Boundary-setting questions

Explicitly separate:
- MVP scope
- later-phase scope
- non-goals
- assumptions currently being used to move forward

Useful prompts include:
- “What must be true for version 1 to be considered successful?”
- “What should not be built in the first phase even if it sounds useful?”
- “Which advanced capabilities are future extensions rather than MVP essentials?”

---

## 6. MVP framing guidance

A good MVP for a new system should usually:
- solve one clear workflow or job
- create one complete usable loop
- avoid carrying too many future abstractions too early
- avoid pretending to support multiple deep use cases before one is proven

For MVP framing, prefer:
- one core workflow over many partial workflows
- one stable object model over many loosely defined entities
- one reliable closed loop over a broad but shallow feature list

---

## 7. Common failure modes

Watch for these mistakes:

### 7.1 Confusing the visible UI model with the internal system model
A page list is not the same thing as the object model.

### 7.2 Assuming the ownership hierarchy too early
A reusable asset may not belong under the object where it is first displayed.

### 7.3 Binding reusable assets too tightly
If something should be reused across targets, runs, or workflows, avoid tying it to only one parent entity.

### 7.4 Mixing MVP and future capability
Do not let white-box, automation, analytics, or advanced orchestration capabilities distort the MVP structure unless they are truly core.

### 7.5 Treating recommendations as confirmed requirements
When proposing structure, clearly mark what is:
- confirmed
- assumed
- recommended
- still open

---

## 8. Recommended checkpoint output

Before a full PRD, produce a checkpoint covering:
- project goal
- target users / roles
- core objects
- system boundary
- MVP scope
- non-goals
- assumptions
- open questions
- recommended direction

Do not move into full PRD expansion until this checkpoint is stable enough.

---

## 9. Suggested next references

After using this reference, commonly useful next references are:
- `prd-template.md`
- `doc-package-sequencing-template.md`
- `multi-agent-handoff-template.md` if downstream role handoff will be needed
- `readiness-checklist-template.md` if the user asks whether design or engineering can start
