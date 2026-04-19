# API Spec Template

Use this template for each important backend interface.

## Endpoint Summary

- Name:
- Method:
- Path:
- Purpose:
- Caller:
- Authentication:
- Authorization:

## Request Parameters

| Field | Type | Required | Description | Example |
|---|---|---:|---|---|

## Response Fields

| Field | Type | Description | Example |
|---|---|---|---|

## Business Rules

- Rule 1:
- Rule 2:
- Rule 3:

## Validation Rules

- Validation 1:
- Validation 2:

## Error Handling

| Code | Meaning | Client handling |
|---|---|---|

## Idempotency / Retry / Concurrency

- Idempotency requirement:
- Retry behavior:
- Concurrency concerns:

## Service Call Flow

1. Receive request
2. Validate auth and permissions
3. Validate request payload
4. Execute business logic
5. Read or write data
6. Call downstream services if needed
7. Emit async event if needed
8. Return response

## Logging / Monitoring / Audit

- Key logs:
- Sensitive fields to mask:
- Metrics:
- Alerts:
- Audit trail requirements:
