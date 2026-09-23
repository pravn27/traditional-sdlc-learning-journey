# Checklists, Artifacts, and Progress Signals

## Definition of Done for Every Feature

A feature is complete when it has:

- Clear acceptance criteria
- Tested happy path and important failure paths
- Input validation, authorization, and useful error responses
- Appropriate logs, metrics, and trace or correlation context
- Reviewed data-migration and backward-compatibility impact
- Documented deployment and rollback implications
- A small, understandable change set

## Principal Engineer Decision Checklist

Before committing to a material design, ask:

1. What business outcome and user problem are we solving?
2. What is the simplest design that satisfies current needs?
3. Which assumptions are uncertain, and how will we test them?
4. What can fail, how will users experience it, and how will we detect it?
5. What data consistency and security guarantees are required?
6. What will this cost to build, operate, and evolve?
7. What is the rollout, rollback, and migration path?
8. Can another engineer understand and safely change this in six months?

## Portfolio Artifacts

Your portfolio should reveal reasoning and operational maturity, not only source code.

| Artifact | Show |
| --- | --- |
| `README.md` | Purpose, setup, architecture overview, key decisions |
| `docs/adr/` | Short Architecture Decision Records with alternatives and trade-offs |
| `docs/api/` | API contracts, examples, error behavior, versioning rules |
| `docs/runbooks/` | Investigation and recovery steps for expected incidents |
| `docs/postmortems/` | Blameless analysis of simulated or real failures |
| `docs/diagrams/` | Context, container, sequence, and deployment diagrams |
| `docs/metrics.md` | SLIs, SLOs, dashboards, alerts, and ownership |
| `docs/load-tests/` | Scenarios, baseline results, bottleneck evidence, improvements |

## Minimal ADR Template

```markdown
# ADR: [Decision title]

## Context
What problem and constraints require a decision?

## Options considered
What credible alternatives were evaluated?

## Decision
What is being chosen and why?

## Consequences
What benefits, costs, risks, and follow-up work result?

## Validation
What tests, metrics, or rollout signals will show this was the right choice?
```

## Progress Signals

You are progressing when you can:

- Explain why a solution works, not merely describe the code.
- Predict likely failure modes before they occur.
- Choose between alternatives using explicit trade-offs.
- Turn a vague request into testable, observable acceptance criteria.
- Use AI to accelerate investigation and implementation while independently verifying results.
- Communicate a technical decision clearly to engineers, product partners, and leadership.
- Improve another engineer's effectiveness through documentation, tooling, standards, or mentoring.

## Final Self-Assessment

For any completed feature, review these questions:

1. Could a user, product partner, and on-call engineer each understand its intended behavior?
2. Would a retry, duplicate event, dependency timeout, or malformed input cause correct and observable behavior?
3. Can the feature be deployed gradually and rolled back safely?
4. Does the codebase explain the important decision and its trade-off?
5. Did AI reduce cycle time while you retained enough evidence to trust the result?

When these answers are consistently strong, you are no longer merely using an AI coding tool. You are practicing AI-native engineering.
