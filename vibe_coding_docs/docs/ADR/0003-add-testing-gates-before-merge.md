# ADR-0003: Add Testing Gates Before Merge

## Status

Accepted

## Context

Vibe Coding can quickly generate features, but later changes may break previous behavior. Without testing gates, the project may enter a state where each fix creates new bugs.

The project needs a minimum automated safety net before code is merged.

## Decision

Every PR must pass the following gates before merge:

```bash
npm run typecheck
npm run lint
npm run test
npm run build
```

For API or database changes, also run:

```bash
npm run test:integration
```

For UI critical flow changes, also run:

```bash
npm run test:e2e
```

## Consequences

Positive:

- Prevents obvious regressions
- Makes AI-generated changes safer
- Gives Reviewer Agent objective evidence
- Helps humans trust the workflow

Negative:

- Slower than pure Vibe Coding
- Requires maintaining tests
- Some prototype changes may need temporary exceptions

## Exceptions

Prototype-only branches may skip full E2E tests, but must clearly mark the PR as prototype and must not merge into production branch without tests.

## Follow-up

- Add CI workflow
- Add test report template
- Track flaky tests
- Add coverage reporting for domain layer
