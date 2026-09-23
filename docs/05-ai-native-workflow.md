# AI-Native Workflow: Learn and Deliver with Control

AI is most useful as a capable collaborator with constrained scope and explicit verification. It should not be treated as a source of unverified truth or as an owner of production risk.

## Feature Delivery Loop

### 1. Write the specification

Before implementation, define:

- User story and business outcome
- In-scope and out-of-scope behavior
- Acceptance criteria
- API or UI contract
- Data changes and compatibility concerns
- Security and privacy constraints
- Failure scenarios
- Observability requirements
- Rollout and rollback approach

Example requirement: "Customers can place an order exactly once, even when their mobile app retries after a network timeout."

Example acceptance criteria:

- The client supplies an idempotency key.
- Repeated requests with the same key return the original order result.
- A payment timeout does not create a duplicate charge.
- A correlation ID traces the request across dependencies.
- Failed payment processing appears in logs, metrics, and an alert.

### 2. Ground the AI with context

Give AI the relevant repository structure, coding conventions, interfaces, schema, current behavior, desired behavior, constraints, non-goals, and expected output.

Weak prompt:

```text
Build payment processing.
```

Grounded prompt:

```text
Inspect the order module and its existing test conventions. Propose the smallest change that adds idempotent order creation using the existing PostgreSQL repository pattern. Do not change unrelated modules. Return a plan, affected files, assumptions, and risks before writing code.
```

### 3. Ask for a plan before code

Ask the AI to identify impacted modules and contracts, assumptions and unknowns, alternatives where trade-offs matter, tests to add, and operational signals to monitor.

This turns the assistant into an analyst before it becomes an implementer.

### 4. Implement in small slices

1. Add or update the contract.
2. Add a failing test.
3. Implement the smallest behavior that passes.
4. Add edge-case and integration tests.
5. Review the diff for security, concurrency, error handling, and compatibility.
6. Run the application and inspect actual behavior.

Avoid large, unreviewable AI-generated changes.

### 5. Verify independently

Use the right verification layers:

- Unit tests for domain behavior
- Integration tests for database, messaging, and external contracts
- Contract tests for public APIs and webhooks
- Static analysis and dependency/security scanning
- Manual exploratory testing for critical journeys
- Load and resilience testing for high-risk workflows

The standard is not "the AI says it is correct." The standard is evidence that the system satisfies the requirement.

### 6. Operate and learn

Deploy safely, inspect telemetry, and capture what occurred. Convert production findings into stronger specifications, tests, dashboards, runbooks, and future prompts.

## Four Useful AI Roles

| Role | Ask AI to do | Your responsibility |
| --- | --- | --- |
| Teacher | Explain a concept using current code | Confirm through documentation and implementation |
| Architect | Compare options and expose trade-offs | Decide against actual constraints |
| Pair programmer | Draft a narrow implementation or test | Review, run, debug, and own the result |
| Reviewer | Identify edge cases, risks, and missing tests | Validate every finding; retain final judgment |

## Reusable Prompts

### Design review

```text
Challenge this architecture decision. Give the strongest case for keeping a modular monolith and the strongest case for extracting a service. Include operational cost, data ownership, failure isolation, deployment, and team ownership.
```

### Code review

```text
Review this change for duplicate processing, retry safety, race conditions, authorization gaps, backward compatibility, and missing tests. Cite the exact code path for each concern. Do not suggest unrelated refactoring.
```

### Incident investigation

```text
Act as an on-call engineer. Given these logs, metrics, and traces, propose the most likely failure path, evidence for and against it, the next diagnostic query, and the safest immediate mitigation.
```

### Test planning

```text
From this specification and API contract, create a test matrix covering happy path, validation, authorization, timeout, retry, duplicate request, concurrent request, dependency failure, and observability assertions.
```

## Guardrails for AI-Assisted Work

- Do not expose secrets, customer data, proprietary code, or regulated data to tools that are not approved for it.
- Keep changes small and reviewable.
- Ask for citations or code references for claims about the current repository.
- Run tests and inspect the diff yourself.
- Treat generated dependency, security, infrastructure, and database changes as high-risk until reviewed.
- Do not let an agent make irreversible production changes without explicit approval and a rollback path.
