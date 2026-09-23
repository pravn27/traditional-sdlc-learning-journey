# Level-Based Implementation Roadmap: Beginner to Expert

This roadmap is for learning software engineering by building a real system. It separates learning into four levels:

1. Beginner: build correct local features.
2. Intermediate: build a production-ready modular service.
3. Advanced: build for failure, scale, and operations.
4. Expert: shape architecture, strategy, migrations, and engineering systems.

The pace is flexible. Move forward only when you can explain the concepts, implement the use case without copying a tutorial, and prove the behavior with tests or evidence.

## System to Build: FoodFlow

Build one food-ordering platform throughout the roadmap. Starting a new project for every concept creates shallow knowledge; evolving one project makes design trade-offs visible.

```text
Customer -> FoodFlow API -> PostgreSQL
                         -> Redis cache
                         -> Message queue -> Notification worker
                         -> Payment provider
                         -> Logs, metrics, traces, alerts
```

### Core user roles

- Customer: browse menus, manage cart, place and track orders.
- Restaurant owner: manage restaurant, menu, and incoming orders.
- Support agent: inspect orders and resolve customer issues.
- Administrator: manage access, reports, and audit history.

### Recommended stack

Choose one backend ecosystem and stay with it for the full roadmap.

- Backend: Java/Spring Boot, Node.js/NestJS, Python/FastAPI, or .NET.
- Database: PostgreSQL.
- Cache: Redis.
- Async messaging: Kafka, RabbitMQ, SQS, or a local equivalent.
- API: REST first; add event contracts where asynchronous work is valuable.
- Delivery: Docker, CI pipeline, cloud deployment.
- Observability: structured logs, metrics, traces, dashboards, alerts.

## The Learning Loop for Every Topic

1. **Understand**: write the problem this concept solves in your own words.
2. **Implement**: add the smallest useful version to FoodFlow.
3. **Verify**: add tests or measurements that prove the behavior.
4. **Break**: simulate invalid input, duplicate requests, a timeout, load, or a bad deployment.
5. **Explain**: record the trade-off in a short ADR, README note, or runbook.

Do not learn a topic only by reading it. The implementation and failure drill are the real lesson.

---

## Level 1: Beginner - Build Correct Local Features

### Goal

Learn to create a working API, persist data safely, test it, and use Git with confidence.

### Suggested duration

6 to 10 weeks of consistent practice.

### Modules and implementation tasks

1. **Programming, terminal, and Git fundamentals**

   - Concepts: language syntax, functions, classes/modules, errors, collections, package management, command line, Git commits, branches, and pull requests.
   - Build: create the FoodFlow repository and a health-check endpoint.
   - Verify: clone the repository into a fresh directory, install dependencies, run tests, and start the service from the README.
   - Done when: you can create a feature branch, commit a focused change, resolve a merge conflict, and explain the application entry point.

2. **HTTP and REST API basics**

   - Concepts: request/response, HTTP methods, status codes, headers, JSON, route parameters, query parameters, validation, and error responses.
   - Build: `GET /restaurants`, `GET /restaurants/{id}`, `POST /restaurants`, and `POST /restaurants/{id}/menus`.
   - Verify: test success, missing fields, invalid IDs, and unknown routes using an API client or automated tests.
   - Done when: every endpoint returns predictable status codes and a consistent error contract.

3. **Relational database fundamentals**

   - Concepts: tables, primary and foreign keys, normalization, migrations, joins, indexes, transactions, and SQL injection prevention.
   - Build: `restaurants`, `menus`, `menu_items`, and `customers` tables with versioned migrations.
   - Verify: recreate the database from zero and confirm the service starts with the latest schema.
   - Done when: you can explain why each table exists and how its relationships preserve valid data.

4. **Authentication and basic authorization**

   - Concepts: password hashing, sessions or tokens, authentication versus authorization, roles, and least privilege.
   - Build: customer registration/login and route protection for restaurant owners.
   - Verify: customer requests cannot modify a restaurant they do not own.
   - Done when: you can explain what the token identifies, how it expires, and why authorization is checked server-side.

5. **Testing and debugging**

   - Concepts: unit tests, integration tests, test fixtures, assertions, breakpoints, logs, and debugging workflow.
   - Build: tests for menu-price validation, restaurant ownership, and database persistence.
   - Verify: introduce a deliberate defect and watch a test fail before fixing it.
   - Done when: you can distinguish a unit test from an integration test and choose the right one.

### Beginner milestone: Restaurant and menu service

| User story | Implement | Prove |
| --- | --- | --- |
| Customer can see restaurants | Restaurant list endpoint with pagination | Empty list, first page, last page, invalid query tests |
| Owner can add a menu item | Protected create/update endpoint | Ownership and validation tests |
| Customer can view a menu | Restaurant-menu endpoint | Correct relationship and missing restaurant test |

### Beginner exit criteria

- A new developer can run the service using the README.
- APIs validate input and use consistent errors.
- Database changes are migrations, not manual changes.
- Important business rules have tests.
- You can explain the request path from HTTP route to database response.

---

## Level 2: Intermediate - Build a Production-Ready Modular Service

### Goal

Learn to organize a growing system, implement a complete checkout workflow, secure it, and deliver it repeatably.

### Suggested duration

8 to 14 weeks after Level 1.

### Modules and implementation tasks

1. **Modular architecture and domain modeling**

   - Concepts: separation of concerns, domain boundaries, layers, dependency direction, domain services, repositories, DTOs, and Architecture Decision Records.
   - Build: separate FoodFlow into `identity`, `restaurant`, `catalog`, `cart`, and `order` modules.
   - Verify: an order module does not reach into restaurant database details directly; it uses a defined contract.
   - Done when: you can draw a module diagram and explain the ownership of each module.

2. **Cart and order state management**

   - Concepts: state machines, invariants, transactions, optimistic locking, validation, and error contracts.
   - Build: cart add/remove/update, checkout, and an order lifecycle: `CREATED`, `PAYMENT_PENDING`, `CONFIRMED`, `PREPARING`, `OUT_FOR_DELIVERY`, `DELIVERED`, `CANCELLED`.
   - Verify: invalid state transitions, such as `DELIVERED` directly from `CREATED`, are rejected.
   - Done when: state transition rules are in one clear place and are tested.

3. **Production API design**

   - Concepts: pagination, filtering, sorting, versioning, backward compatibility, idempotency keys, webhooks, rate limits, and OpenAPI/contract documentation.
   - Build: `POST /orders` with an idempotency key and a documented API contract.
   - Verify: send the same idempotency key twice and receive the original order result without a duplicate order.
   - Done when: another developer can integrate from the API documentation alone.

4. **Security fundamentals in an application**

   - Concepts: RBAC, input validation, OWASP Top 10, secure error handling, secrets, audit logs, and dependency scanning.
   - Build: customer, restaurant owner, support, and administrator roles; audit order status changes.
   - Verify: run authorization tests for every protected endpoint and inspect the audit trail.
   - Done when: you can explain which role may perform each sensitive action and why.

5. **Containers and continuous integration**

   - Concepts: Docker image, environment configuration, dependency services, CI stages, static analysis, test reporting, and build artifacts.
   - Build: containerize FoodFlow and create a pipeline for formatting, unit tests, integration tests, and image build.
   - Verify: a clean clone passes the same checks locally and in CI.
   - Done when: no one needs undocumented local setup steps to contribute.

### Intermediate milestone: Reliable checkout

| User story | Implement | Prove |
| --- | --- | --- |
| Customer can checkout once | Idempotent order creation | Repeat request returns the same order |
| Customer can pay | Payment adapter with mocked provider | Success, rejection, and timeout tests |
| Support can investigate | Order history and audit events | Audit records include actor, time, and action |
| Team can release safely | CI build and Docker image | Pipeline blocks merge when tests fail |

### Intermediate exit criteria

- The service is modular and its ownership boundaries are documented.
- Checkout is transactionally correct and retry-safe.
- Authentication, authorization, validation, and audit logging exist for sensitive workflows.
- CI verifies changes before release.
- You can explain a key architecture choice in an ADR.

---

## Level 3: Advanced - Build for Failure, Scale, and Operations

### Goal

Learn distributed-systems behavior, resilience, performance, observability, and safe cloud delivery.

### Suggested duration

12 to 20 weeks after Level 2.

### Modules and implementation tasks

1. **Asynchronous processing and event-driven design**

   - Concepts: message queues, event contracts, at-least-once delivery, duplicate messages, retries, exponential backoff, dead-letter queues, outbox pattern, eventual consistency, and idempotent consumers.
   - Build: publish `OrderCreated`; send notifications asynchronously; persist an outbox record in the order transaction.
   - Verify: deliver an event twice, stop the notification worker, then recover it from the queue or dead-letter path.
   - Done when: order creation does not wait for notification delivery and duplicate messages do not create duplicate notifications.

2. **Payment reliability and distributed transactions**

   - Concepts: timeouts, circuit breakers, saga pattern, compensating actions, reconciliation, webhook signature validation, and stable state transitions.
   - Build: payment initiation, provider webhook handling, reconciliation job, and a failed-payment recovery path.
   - Verify: simulate provider timeout after charging but before responding; reconcile without duplicate charge or duplicate confirmation.
   - Done when: you can explain why a database transaction cannot solve this cross-service consistency problem alone.

3. **Observability and reliability engineering**

   - Concepts: structured logs, correlation IDs, metrics, traces, SLIs, SLOs, error budgets, dashboards, alerts, runbooks, incident response, and postmortems.
   - Build: a correlation ID across API, database, payment, queue, and notification worker; checkout-success and checkout-latency metrics; dashboard and alert.
   - Verify: find the root cause of a simulated checkout failure using telemetry only.
   - Done when: an on-call engineer can detect, diagnose, and mitigate an expected failure using the runbook.

4. **Performance and capacity**

   - Concepts: latency versus throughput, query plans, indexes, Redis caching, cache invalidation, connection pools, load testing, rate limiting, backpressure, graceful degradation, and capacity modeling.
   - Build: cache restaurant menus, add database indexes based on query evidence, and add a rate limit for order creation.
   - Verify: establish a baseline load test, optimize one bottleneck, and compare before-and-after p95 latency and error rate.
   - Done when: performance claims are supported by measurements rather than guesswork.

5. **Cloud delivery and recovery**

   - Concepts: compute, storage, networking, load balancers, autoscaling, secrets management, infrastructure as code, health checks, blue-green/canary release, rollback, backup, and disaster recovery.
   - Build: deploy FoodFlow to one cloud environment with infrastructure configuration, health checks, staged release, and rollback notes.
   - Verify: release a faulty version in a non-production environment and recover through rollback.
   - Done when: you can state the deployment path, key operational risks, and recovery procedure.

### Advanced milestone: Observable, event-driven checkout

| Failure scenario | Expected system behavior | Evidence to collect |
| --- | --- | --- |
| Client retries checkout | Same order result; no duplicate charge | Idempotency record and order trace |
| Payment provider is slow | Timeout, retry policy, clear pending state | Timeout metric, log, and alert |
| Event is duplicated | Consumer safely ignores or deduplicates it | Consumer log and deduplication record |
| Notification worker fails | Order succeeds; work retries later | Queue depth, failed-job record, recovery runbook |
| Menu search becomes slow | System identifies and mitigates bottleneck | Trace, query plan, load-test comparison |

### Advanced exit criteria

- Critical workflows tolerate retries, duplicate events, and dependency failure.
- The application has logs, metrics, traces, dashboards, alerts, and runbooks.
- Performance and capacity decisions are evidence-based.
- Deployment is repeatable, staged, and reversible.
- You can lead a blameless postmortem for a simulated incident.

---

## Level 4: Expert - Shape Systems, Strategy, and Engineering Leverage

### Goal

Operate with principal-engineer judgment: make trade-offs explicit, guide multiple teams, evolve legacy systems safely, and use AI to improve engineering without lowering standards.

### Suggested duration

Ongoing. This is demonstrated through repeated decisions and outcomes, not completed by a course.

### Modules and implementation tasks

1. **Architecture strategy and trade-offs**

   - Concepts: build versus buy, platform thinking, total cost of ownership, roadmap sequencing, architecture fitness functions, risk registers, ADRs, RFCs, and service-boundary decisions.
   - Build: an architecture proposal for FoodFlow's next 12 to 24 months, comparing modular monolith, service extraction, and managed-service alternatives.
   - Verify: reviewers can see constraints, options, decision, non-goals, expected cost, risks, rollout, rollback, and success metrics.
   - Done when: the decision reduces ambiguity for other engineers instead of creating more meetings.

2. **Data and system evolution**

   - Concepts: schema evolution, compatibility, CDC, event sourcing where justified, analytics pipelines, OLTP versus OLAP, data governance, retention, backfills, dual writes, and decommissioning.
   - Build: an incremental reporting-data pipeline from orders to analytics without degrading checkout performance.
   - Verify: backfill correctness, data reconciliation, privacy retention behavior, and rollback plan.
   - Done when: data changes are safe for current users and useful for future analytics.

3. **Legacy modernization**

   - Concepts: strangler-fig pattern, incremental migration, parallel run, compatibility layers, feature flags, risk-based refactoring, dependency upgrades, and decommissioning strategy.
   - Build: migrate one legacy-like FoodFlow module behind a compatibility interface and feature flag.
   - Verify: compare old and new behavior during parallel run; define rollback triggers before migration.
   - Done when: the migration improves the system without requiring a risky big-bang rewrite.

4. **Organizational engineering leverage**

   - Concepts: technical mentoring, design-review facilitation, developer experience, engineering standards, internal platforms, inner-source practices, conflict resolution, stakeholder alignment, and executive communication.
   - Build: a reusable delivery capability such as a service template, observability library, secure API standard, or architecture-review process.
   - Verify: another team adopts it and reports reduced setup time, fewer incidents, or better delivery confidence.
   - Done when: your work makes multiple engineers or teams more effective.

5. **AI-native engineering leadership**

   - Concepts: context engineering, agent workflows, quality gates, AI code review, generated test cases, RAG basics, embeddings, vector databases, LLM evaluation, prompt/version management, AI security, privacy, and human-in-the-loop approval.
   - Build: an AI-assisted feature-delivery workflow that requires a specification, grounded context, a plan, narrow changes, tests, review, and an operational check.
   - Verify: compare cycle time and defect findings with and without the workflow; record what must remain human-owned.
   - Done when: AI increases speed and learning without weakening safety, privacy, or accountability.

### Expert milestone: Principal-level system proposal

Create a package containing:

1. A business problem and success metrics.
2. A system context and container diagram.
3. Options with costs, risks, and trade-offs.
4. An ADR or RFC with a recommendation.
5. API, data, security, reliability, and operational implications.
6. A phased migration, rollout, rollback, and decommissioning plan.
7. SLOs, dashboards, alerts, and runbooks.
8. A plan for team ownership, communication, and technical enablement.

### Expert exit criteria

- You see system-wide consequences before they become incidents.
- You help teams make decisions without relying on your title.
- You connect architecture to product outcomes, risk, and cost.
- You guide migrations safely and avoid unnecessary big-bang rewrites.
- You improve the organization's ability to build, operate, and evolve systems.

---

## Concept Progression at a Glance

| Concept | Beginner | Intermediate | Advanced | Expert |
| --- | --- | --- | --- | --- |
| APIs | CRUD and validation | Contracts, idempotency, versioning | Event/webhook resilience | Organization-wide API standards |
| Data | Schema and migrations | Transactions and locking | Outbox, reconciliation, performance | Governance, pipelines, evolution |
| Security | Login and roles | RBAC, audits, OWASP | Threat models and secrets operations | Security strategy and platform guardrails |
| Reliability | Logs and tests | CI and predictable errors | SLOs, alerts, runbooks, recovery | Reliability strategy and cross-team standards |
| Architecture | Modules | Bounded contexts and ADRs | Async workflows and service boundaries | Long-term technical strategy |
| AI | Explain code and draft tests | Grounded feature assistance | Agent workflow and review gates | Evaluation, governance, and adoption strategy |

## Weekly Practice Schedule

1. **Day 1 - Understand**: learn one topic and write a short explanation in your own words.
2. **Days 2-3 - Build**: add one small, complete use case to FoodFlow.
3. **Day 4 - Break**: test an invalid input, concurrency issue, timeout, duplicate event, or dependency failure.
4. **Day 5 - Prove**: add tests, telemetry, or a measurement that verifies the behavior.
5. **Day 6 - Review**: ask AI or a peer to challenge the design; validate all findings yourself.
6. **Day 7 - Explain**: write an ADR, runbook, or trade-off note and share the learning.

## Rules for Moving to the Next Level

- Do not move forward because you watched a course; move forward because you can build and explain the use case.
- Do not add microservices because they are popular; add boundaries only when they solve a demonstrated problem.
- Do not claim performance, reliability, or security without tests, telemetry, or other evidence.
- Do not allow AI-generated code into a critical path without understanding, review, and verification.
- Keep every feature small enough to test, observe, deploy, and roll back safely.

The progression is deliberate: first make a feature work, then make it correct, then make it resilient, and finally make the whole engineering system more capable.
