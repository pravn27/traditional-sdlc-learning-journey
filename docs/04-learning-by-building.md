# Best Technique: Learn by Building a Real-Time System

The strongest technique is project-led learning with deliberate failure practice. A course can introduce vocabulary; a system you design, break, operate, and improve gives you judgment.

## Use One Continuous Project

Build a food-ordering platform. It is realistic enough to cover common production concerns without becoming impossible for one learner.

Start as a modular monolith. Do not begin with microservices. Extract a component only when separate deployment, scaling, team ownership, reliability isolation, or technology requirements justify the added operational complexity.

### Core capabilities

- Customer signup, login, profile, and delivery addresses
- Restaurant and menu discovery
- Cart and checkout
- Order lifecycle: `CREATED`, `PAYMENT_PENDING`, `CONFIRMED`, `PREPARING`, `OUT_FOR_DELIVERY`, `DELIVERED`, `CANCELLED`
- Payment provider integration and webhook processing
- Restaurant order management
- Notifications
- Admin reports and audit history

### Practical stack

Choose one ecosystem you can sustain. Example options:

- Backend: Java/Spring Boot, Node.js/NestJS, Python/FastAPI, or .NET
- Database: PostgreSQL
- Cache: Redis
- Async messaging: Kafka, RabbitMQ, SQS, or an equivalent local setup
- API: REST first; add events only where asynchronous behavior is valuable
- Delivery: Docker, CI pipeline, cloud deployment
- Observability: structured logs, metrics, traces, dashboards, alerts

The stack is secondary. The quality of the decisions and evidence is primary.

## Build in Realistic Phases

1. **Basic application flow**

   - Build: authentication, restaurant/menu CRUD, cart, and simple order creation.
   - Learn: HTTP, API design, database modeling, migrations, validation, and basic testing.

2. **Scale and predictable performance**

   - Build: pagination, filtering, appropriate database indexes, caching, connection pooling, rate limiting, and a baseline load test.
   - Learn: performance measurement before optimization, latency, caching trade-offs, and capacity thinking.

3. **Asynchronous order processing**

   - Build: an `OrderCreated` event, asynchronous notifications, retry policy, error handling, and a dead-letter path.
   - Learn: queues, at-least-once delivery, duplicate events, eventual consistency, and operational recovery.

4. **Payment reliability**

   - Build: a payment adapter or mock provider, idempotency keys, webhook handling, stable state transitions, reconciliation logic, and audit history.
   - Learn: distributed transactions are not magic; use state machines, explicit recovery, and idempotency.

5. **Operability**

   - Build: correlation IDs, structured logs, request metrics, traces, a dashboard, alerts, and a runbook for failed checkout.
   - Learn: a production system is not complete when it responds successfully once. It must be diagnosable when it fails.

6. **Security and delivery**

   - Build: RBAC, input validation, secret handling, audit trails, CI checks, deployment configuration, staged release, and rollback procedure.
   - Learn: secure, repeatable delivery is part of the feature, not work saved for later.

## Weekly Study Cycle

- Day 1: Learn one concept and write a short summary in your own words.
- Days 2-3: Implement it in the project.
- Day 4: Break it with invalid input, concurrency, dependency outage, retry, or malformed event.
- Day 5: Add tests, telemetry, and an ADR or runbook entry.
- Day 6: Request an AI design or code review, then validate every finding yourself.
- Day 7: Explain the decision and trade-off aloud or in writing as if presenting to a staff engineer.

## Concept-to-Example Map

| Concept | Real-time example |
| --- | --- |
| Idempotency | Mobile app retries checkout after a network timeout without creating a second order or charge |
| Transaction boundary | Order and payment state remain valid when two requests arrive at once |
| Event-driven design | `OrderCreated` triggers notification processing without delaying checkout |
| Retry and backoff | Temporary payment-provider errors recover without overwhelming the provider |
| Dead-letter queue | A malformed notification event is retained for investigation instead of disappearing |
| Caching | Restaurant menus load quickly while stale data remains bounded and understandable |
| Observability | A correlation ID traces checkout across API, database, payment, queue, and notification worker |
| RBAC | Restaurant owners can update only their own menus; customers cannot access admin reports |
| Feature flags | A new payment method can be enabled for a limited audience and disabled safely |

## Failure Drills

| Drill | Simulate | Learn |
| --- | --- | --- |
| Duplicate checkout | Client retries after timeout | Idempotency and stable response behavior |
| Payment outage | Provider is slow or unavailable | Timeouts, retry policy, circuit breaking, user messaging |
| Duplicate event | Queue delivers `OrderCreated` twice | Idempotent consumers and deduplication |
| Slow database query | Menu search degrades | Query plans, indexing, caching, latency measurement |
| Bad deployment | New version raises errors | Health checks, rollback, feature flags, alerts |
| Unauthorized request | Customer calls admin endpoint | Authorization, audit logging, least privilege |

The habit to develop is: after implementing a happy path, ask "How does this fail, who notices, and how do we recover?"
