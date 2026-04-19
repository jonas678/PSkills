# PRD Template

## 1. Document Information

- Project / Feature Name:
- Version:
- Status:
- Author:
- Date:

## 2. Background and Problem Statement

- Business context:
- Current workflow:
- Existing pain points:
- Why this should be built now:

## 3. Goals and Non-Goals

### Goals
- Goal 1:
- Goal 2:
- Goal 3:

### Non-Goals
- Non-goal 1:
- Non-goal 2:

## 4. Users, Roles, and Permissions

| Role | Description | Key permissions | Restrictions |
|---|---|---|---|

## 5. Core User Scenarios

For each scenario, describe:

- Scenario name
- Trigger
- Preconditions
- Main flow
- Alternate flow
- Exception flow
- Expected result

## 6. Functional Requirements

For each feature, describe:

- Feature name
- Objective
- Target role
- Entry point
- Preconditions
- Detailed behavior
- Validation rules
- Error messages or feedback
- Acceptance criteria

## 7. Frontend Pages and User Journey

For each page or key interaction, describe:

- Page name
- Route or entry point
- User role
- Page goal
- Main sections or components
- Step-by-step interaction flow
- Form fields and validation
- Loading, empty, error, success, and no-permission states
- Edge cases

## 8. Backend API Design

List all major endpoints in a table first.

| Method | Endpoint | Purpose | Caller |
|---|---|---|---|

Then detail each endpoint using the API template.

## 9. Backend Processing Flow and Service Calls

Describe:

- request entry point
- validation sequence
- service orchestration
- data persistence
- event publishing or async jobs
- downstream dependencies
- retry or compensation behavior
- final response behavior

## 10. Data Model and State Transitions

### Core entities
| Entity | Purpose | Key fields |
|---|---|---|

### State transitions
| Entity | From state | To state | Trigger | Rule |
|---|---|---|---|---|

## 11. Impacted Modules and Change List

| Module | Type | Action | Description |
|---|---|---|---|

## 12. Acceptance Criteria

- Functional acceptance:
- Permission acceptance:
- Error handling acceptance:
- Data accuracy acceptance:
- Performance acceptance:

## 13. Risks, Dependencies, and Open Questions

### Risks
- Risk 1:
- Risk 2:

### Dependencies
- Dependency 1:
- Dependency 2:

### Open Questions
- Question 1:
- Question 2:
