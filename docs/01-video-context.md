# Video Context: Traditional SDLC Is Changing

Source video: [Traditional SDLC Is Changing: Welcome to Vibe Coding & Agentic Engineering](https://www.youtube.com/watch?v=F5IOkeyK9ng) by Krish Naik.

## Important Source Note

This reference uses the video's stated topic and the previous discussion's best-effort synthesis. A complete source transcript was not available during the discussion, so this document captures the recurring, defensible engineering themes rather than claiming a word-for-word summary of the video.

## Main Message

Traditional SDLC is not disappearing. Its stages remain necessary:

```text
Requirements -> Design -> Build -> Test -> Deploy -> Operate -> Improve
```

The change is that AI can now participate in every stage. Work shifts from producing every first draft manually to directing, grounding, verifying, and operating AI-assisted work.

| Traditional activity | AI-native equivalent | Engineer is accountable for |
| --- | --- | --- |
| Gather requirements | Convert intent into a structured specification | Correct problem, scope, acceptance criteria, constraints |
| Design architecture | Explore alternatives, diagrams, risks, and trade-offs | Architecture decision and long-term ownership |
| Write code | Generate small, reviewable implementation slices | Correctness, style, integration, maintainability |
| Test manually | Generate tests, fixtures, edge cases, and failure scenarios | Test strategy and meaningful coverage |
| Review changes | Use AI for first-pass code review and impact analysis | Final review and risk acceptance |
| Deploy and operate | Analyze telemetry and runbooks with AI assistance | Safe releases, incident response, reliability |

## Key Ideas to Carry Forward

1. **Vibe coding is useful, but insufficient alone**

   - Use it for: prototypes, exploration, scaffolding, and small bounded changes.
   - Do not skip: requirements, context, review, tests, security, deployment, and operational ownership.
   - Key lesson: speed without verification increases risk.

2. **Agentic engineering is structured delegation**

   - An agent can: investigate a repository, propose a plan, change a narrow set of files, run tests, and report results.
   - Constrain it with: a specification, repository context, clear non-goals, verification steps, and human approval for material decisions.
   - Key lesson: agents execute bounded work; engineers own the decision and outcome.

3. **Context engineering becomes a core skill**

   - Provide: current code, schemas, contracts, constraints, conventions, examples, and the exact desired outcome.
   - Avoid: vague prompts that require the AI to invent requirements or repository conventions.
   - Key lesson: better context produces more reliable AI-assisted work.

4. **Verification is more valuable, not less**

   - Verify: correctness, security, maintainability, observability, and usefulness.
   - Use: tests, reviews, static checks, telemetry, and production feedback.
   - Key lesson: faster implementation shifts the bottleneck to choosing the right work and proving it is safe.

## Practical Interpretation

Do not ask, "Can AI write this code?" Ask:

1. What business outcome and constraints define success?
2. What context must an AI understand before changing the system?
3. What is the smallest safe slice of work?
4. How will tests and production signals show that it works?
5. Who owns the outcome when the system fails at 2 a.m.?

The engineer remains the owner of the answers.
