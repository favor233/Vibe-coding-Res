# ADR-0002: Use AI Provider Interface

## Status

Accepted

## Context

The product depends on AI capabilities, but the exact model provider may change over time.

Risks of coupling directly to one provider:

- Hard to switch models
- Hard to test
- Provider-specific response format leaks into business logic
- Outages become harder to handle
- Cost optimization becomes difficult

## Decision

Create an AI Provider interface and keep provider-specific logic in infrastructure adapters.

Example:

```ts
interface AiProvider {
  generateStructuredResponse<TInput, TOutput>(
    input: TInput,
    schema: Schema<TOutput>
  ): Promise<TOutput>
}
```

Application services should depend on the interface, not a concrete SDK.

## Consequences

Positive:

- Easier to switch providers
- Easier to mock in tests
- Cleaner business logic
- Better error handling
- Can add fallback provider later

Negative:

- Slight upfront abstraction cost
- Interface must not become too generic
- Some provider-specific features may need explicit extension

## Alternatives Considered

### Direct SDK usage everywhere

Rejected because it creates long-term coupling.

### Build a fully generic AI orchestration framework

Rejected because it is over-engineered for the current stage.

## Follow-up

- Define provider error types
- Add timeout handling
- Add structured output validation
- Add prompt injection tests
- Add provider cost logging
