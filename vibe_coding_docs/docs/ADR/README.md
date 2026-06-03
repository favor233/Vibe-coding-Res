# ADR

ADR means Architecture Decision Record.

Use this directory to record important technical and architecture decisions.

## When to create an ADR

Create an ADR when the project makes decisions such as:

- Choosing architecture style
- Adding a database
- Adding vector search
- Choosing AI provider strategy
- Introducing queue system
- Changing auth model
- Changing deployment strategy
- Splitting service
- Adding major dependency

## File naming

```text
0001-use-modular-monolith.md
0002-use-provider-interface-for-ai-models.md
0003-add-vector-database-for-retrieval.md
```

## ADR status

Allowed statuses:

```text
Proposed
Accepted
Rejected
Deprecated
Superseded
```

## ADR template

```md
# ADR-000X: Title

## Status

Proposed / Accepted / Rejected / Deprecated / Superseded

## Context

What problem are we solving?

## Decision

What did we decide?

## Consequences

What happens because of this decision?

## Alternatives Considered

What other options were considered?

## Follow-up

What should be done next?
```
