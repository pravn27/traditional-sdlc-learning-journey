# Beginner Roadmap: Learn, Understand, and Practice

## Learning Philosophy

Use a project-led approach rather than collecting disconnected tutorials. Learn one concept, apply it immediately, deliberately test its limits, and record the decision.

```text
Learn -> Apply -> Break or test -> Observe -> Refactor -> Document the trade-off
```

For every topic, be able to answer:

1. What problem does this solve?
2. What fails when it is absent or misused?
3. What trade-off does it introduce?
4. How do we verify it works?
5. How would we operate it in production?

## Foundation Stages

1. **Engineering fundamentals**

   - Learn: one production-relevant language deeply; Git, HTTP, SQL, JSON, debugging, Linux/terminal fundamentals, dependency management, unit tests, and basic data structures.
   - Outcome: build a small API with authentication, a relational database, migrations, and tests without relying on a tutorial's exact steps.

2. **Web service development**

   - Learn: REST API design, validation, error contracts, authentication, authorization, pagination, filtering, API documentation, and integration testing.
   - Outcome: deliver an API that another engineer can consume safely.

3. **System design foundations**

   - Learn: modular design, database modeling, caching, queues, asynchronous workflows, timeouts, retries, idempotency, and eventual consistency.
   - Outcome: explain why a request should be synchronous or asynchronous and how it behaves when a dependency fails.

4. **Production engineering**

   - Learn: Docker, CI/CD, configuration, secrets, cloud fundamentals, structured logging, metrics, tracing, dashboards, alerting, load testing, and rollback.
   - Outcome: deploy a service, observe it, diagnose a failure, and recover safely.

5. **AI-native delivery**

   - Learn: specification writing, context engineering, prompt design, AI-assisted implementation, AI review, agent supervision, and independent verification.
   - Outcome: use AI to accelerate a feature from requirement to tested, observable implementation without surrendering engineering judgment.

## 30-Day Starter Plan

| Days | Focus | Evidence of learning |
| --- | --- | --- |
| 1-5 | Language, Git, HTTP, SQL, tests | Small API, migration, unit tests, meaningful commits |
| 6-10 | REST, validation, auth, error handling | Login-protected CRUD endpoints and API examples |
| 11-15 | Data modeling, transactions, indexing | Order schema, migration, transaction boundary, query review |
| 16-20 | Docker, CI, integration tests | Containerized service with automated test pipeline |
| 21-25 | Logs, metrics, tracing, failure handling | Correlation IDs, dashboard, timeout/retry exercise |
| 26-30 | AI-native feature delivery | One feature with spec, plan, code, tests, review, and ADR |

## 12-Week Implementation Plan

| Weeks | Focus | Build and prove |
| --- | --- | --- |
| 1-2 | Language, Git, HTTP, SQL, testing | CRUD service with login, migrations, unit tests, and a project README |
| 3-4 | API design, modular architecture, Docker | Restaurant, menu, cart, and order modules with OpenAPI documentation |
| 5-6 | Data integrity, payments, security | Idempotent checkout, RBAC, payment webhook, audit log, integration tests |
| 7-8 | Async processing and distributed systems | Order events, retries, dead-letter handling, notification worker |
| 9-10 | CI/CD, cloud, observability, performance | Pipeline, deployment, logs/metrics/traces, load-test result |
| 11 | System design and leadership communication | Architecture diagrams, ADRs, trade-off document, failure-mode review |
| 12 | AI-native delivery simulation | One feature from specification to production-style release |

## Suggested Learning Resources

Prefer primary documentation for the tools you adopt. Good starting points for AI-native practice include:

- [GitHub Copilot documentation](https://docs.github.com/en/copilot)
- [Claude Code documentation](https://docs.anthropic.com/en/docs/claude-code/overview)
- [OpenAI Agents SDK documentation](https://openai.github.io/openai-agents-python/)
- [OpenAI guide to building agents](https://openai.com/business/guides-and-resources/a-practical-guide-to-building-agents/)

Documentation teaches tool capability. Your project, tests, failures, and written trade-offs create engineering ability.
