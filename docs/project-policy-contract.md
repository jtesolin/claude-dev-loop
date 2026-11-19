# Project policy contract

The `claude-dev-loop` plugin is intentionally project-agnostic. A repository can extend it with an optional:

`.claude/dev-loop-policy.md`

The main implementation session reads this file together with the repository's normal `CLAUDE.md` / `AGENTS.md` instructions. For PR review, the trusted-base version is authoritative; a version changed by the PR is evidence under review.

## Recommended sections

### Architecture and ownership

Document component/repository responsibilities and source-of-truth boundaries.

Examples:
- which service owns durable domain state;
- frontend/BFF versus backend responsibilities;
- authentication/authorization authority;
- which system is allowed to make nondeterministic/model decisions;
- persistence/migration ownership.

### Runtime lifecycle invariants

Document prerequisite producers and important supported flows.

Example:

`create order -> persist order -> publish event -> consumer projects status -> UI reads status`

This lets the generic reviewer verify production/runtime reachability without knowing the project's domain in advance.

### Validation requirements

List required commands or check categories, such as:
- unit/integration tests;
- lint;
- typecheck;
- build;
- migration checks;
- end-to-end/smoke tests.

Do not hard-code ephemeral CI run IDs.

### Documentation/status requirements

Identify durable documents that must be updated when behavior changes.

### Cross-repository dependencies

Name related repositories and what contracts/prerequisites matter. When a change spans repositories, say what evidence is required from each one.

### Human validation

List criteria that are legitimately manual/external-environment checks. Avoid putting automatable engineering requirements here.

### Project-specific escalation rules

Add project-specific decisions the generic Director must not make autonomously, such as:
- destructive migrations;
- security/trust-boundary changes;
- public API compatibility breaks;
- billing/financial semantics;
- regulated-data handling;
- major architecture ownership changes.

## Keep the policy thin

Do not copy the generic dev-loop state machine into project policy. The plugin owns:
- independent review;
- Director dispositions;
- retry state;
- durable PR records;
- external evidence reconciliation;
- READY rules.

The project policy should describe only facts and requirements unique to the project.
