# ADR-0001: Use Modular Monolith as Initial Architecture

## Status

Accepted

## Context

The project needs to move fast in the early Vibe Coding stage while remaining maintainable after multiple iterations.

A microservice architecture may look scalable, but it introduces extra complexity:

- Multiple deployment units
- Distributed tracing
- Network failure handling
- Cross-service authentication
- Harder local development
- More difficult context management for AI Agent

At this stage, the product requirements are still evolving. The team needs fast iteration and clear module boundaries more than independent service scaling.

## Decision

Use a modular monolith as the initial architecture.

The codebase will be one deployable application, but organized into clearly separated modules:

```text
auth
users
learning-plans
assessments
lessons
ai-tutor
progress
billing
notifications
```

Each module should own its domain logic, application services, infrastructure adapters, tests, and documentation.

## Consequences

Positive:

- Faster development
- Easier local testing
- Easier for AI Agent to understand context
- Lower operational complexity
- Clear migration path to services later

Negative:

- Requires discipline to maintain module boundaries
- Bad imports can create hidden coupling
- Scaling is at application level before service extraction

## Alternatives Considered

### Microservices from day one

Rejected because it creates too much infrastructure and coordination cost for an early product.

### Single unstructured monolith

Rejected because it becomes hard to maintain after many Vibe Coding iterations.

### Serverless-only architecture

Rejected for now because business logic and testing may become fragmented.

## Follow-up

- Add module README files
- Enforce import boundaries
- Add architecture review checklist
- Create ADR if any module is extracted into a service later
