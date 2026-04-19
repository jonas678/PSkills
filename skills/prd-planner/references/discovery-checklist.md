# Requirement Discovery Checklist

Use these questions selectively. Do not ask everything at once.

## 1. Business context

- Why do we want to build this now?
- What business problem does it solve?
- What metric or outcome should improve?
- Is this driven by growth, efficiency, compliance, revenue, or user experience?

## 2. Users and roles

- Who will use it?
- Are there multiple roles with different permissions?
- Who creates, reviews, approves, edits, or views the data?
- Which external systems also interact with it?

## 3. Current state and pain points

- What is the current workflow today?
- What are the current inefficiencies or risks?
- Which steps are manual, slow, error-prone, or hard to track?
- Which existing tools or systems are involved?

## 4. Core scenarios

- What are the top 3 to 5 user scenarios?
- What is the trigger for each scenario?
- What does success look like for the user?
- What exceptions or failure cases must be handled?

## 5. Functional scope

- What are the must-have functions for the first version?
- What functions are optional or future scope?
- Which parts are explicitly out of scope?
- Are there approval flows, notifications, exports, audit trails, or reporting needs?

## 6. Frontend workflow

- What screens or pages are needed?
- What is the entry point?
- What is the step-by-step user journey?
- What fields, filters, actions, and status indicators are needed?
- What should happen in loading, empty, error, and success states?

## 7. Backend and data

- What entities or data objects are involved?
- What APIs are needed?
- Which systems provide or consume the data?
- Are there state transitions or lifecycle rules?
- Are there idempotency, retry, async job, or callback requirements?

## 8. Rules and constraints

- What permission rules apply?
- What validation rules apply?
- Are there compliance, privacy, or audit requirements?
- Are there performance or scalability constraints?
- Are there release deadlines or phased rollout constraints?

## 9. Delivery planning

- What belongs in MVP?
- What can wait until phase 2?
- What teams need to coordinate?
- What dependencies could block delivery?
