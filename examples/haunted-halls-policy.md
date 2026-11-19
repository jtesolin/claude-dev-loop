# Haunted Halls dev-loop policy example

This file demonstrates how Haunted Halls-specific knowledge can live outside the generic plugin. It is an example for extraction/migration, not yet the authoritative Haunted Halls policy.

## Repositories and ownership

- Frontend: `jtesolin/haunted-halls`
- Engine: `jtesolin/haunted-halls-engine`
- The engine owns authoritative game/domain state and game behavior.
- The frontend owns presentation, user interaction, BFF behavior, and authentication integration.
- A locally correct frontend feature is not complete if the engine cannot produce the state it consumes through normal application execution.

## Cross-repository lifecycle

For material features spanning the repositories, trace the complete supported flow across both repositories. Do not treat test fixtures, manually inserted database rows, or a read API with no normal producer as proof of reachability.

Example historical invariant:

`create campaign -> create/associate character -> campaign exposes character id -> frontend enables Character Sheet -> character API returns character data`

## Canonical durable context

When relevant, use trusted-base engine architecture/status/review-policy documents as canonical project context. If a frontend change depends on engine behavior, inspect enough engine evidence to validate the prerequisite rather than reasoning only from frontend types.

## Validation

Use each repository's current trusted project instructions for exact commands and required CI checks.

## Human/product decisions

Escalate rather than invent:
- undocumented game/product semantics;
- significant architecture/responsibility changes;
- authentication/trust-boundary redesign;
- destructive persistence/data-recovery behavior;
- meaningful schema/migration strategy changes;
- model/tool privilege or world-authority boundary changes;
- substantial cross-repository scope expansion.

## Deployment/release

Project-specific deployment and promotion rules belong here or in the repository's normal operational docs, not in the generic plugin.
