# ADR-0004: Require Human Approval for High-Risk Changes

## Status

Accepted

## Context

Multi-agent review can catch many problems, but some changes have business, legal, security, or data-loss consequences that should not be delegated fully to AI.

High-risk examples:

- Deleting user data
- Changing permissions
- Changing payment logic
- Breaking database migrations
- Changing production infrastructure
- Exposing user data to AI providers

## Decision

High-risk changes require explicit human approval before implementation or merge.

High-risk areas include:

```text
auth
authorization
billing
database schema breaking changes
data deletion
privacy
production deployment
AI tool execution
secret management
```

## Consequences

Positive:

- Reduces catastrophic mistakes
- Clarifies responsibility
- Prevents AI from silently making risky decisions

Negative:

- Slows down high-risk development
- Requires humans to understand enough context to approve

## Follow-up

- Add high-risk labels to tasks
- Add PR checklist item
- Configure branch protection for high-risk paths
- Require Security Agent review for high-risk tasks
