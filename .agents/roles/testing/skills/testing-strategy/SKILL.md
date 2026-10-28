---
name: testing-strategy
description: Design test strategies and cases from requirements, acceptance criteria, and module contracts, then execute verification and analyze failures. Use for testing features, critical flows, boundary conditions, and regression risks.
---

# Testing Strategy

Choose verification methods based on observable behavior and actual risks, balancing their ability to find defects against execution and maintenance costs.

## Define verification goals

- Derive normal behavior, business rules, failure outcomes, and required invariants from requirements and acceptance criteria.
- Follow module contracts and critical flows to identify risks across boundaries, and select necessary regression coverage from the scope of the change.
- Link cases to existing requirement, module, or flow identifiers so results can be traced to specific verification goals.
- Distinguish established expectations from assumptions that still need clarification. Do not use the current implementation as the source of expected results.

## Choose methods

Use the lowest level of testing that can adequately verify the goal. Behavior across modules and the semantics of real dependencies require corresponding integration evidence.

| Verification goal | Suitable methods |
| --- | --- |
| Business rules, pure computation, state transitions | Unit tests, boundary value analysis, and equivalence partitioning |
| Module interfaces, persistence, interactions with external dependencies | Contract or integration tests, including errors and data consistency |
| Critical user flows | End-to-end tests or reproducible manual procedures |
| Explicit quality requirements | Performance, reliability, security, or accessibility checks under the agreed conditions |

When using test doubles, state which boundary they replace. Do not infer that real databases, networks, or third-party services have the same semantics as their doubles. Coverage metrics provide clues; sufficient testing depends on whether relevant behaviors and risks have been verified.

## Execute and analyze

- Describe each case's initial state, inputs or actions, expected outputs, and state changes. Prepare reproducible data and environments.
- Record actual commands or operations, necessary environment conditions, and results. Distinguish passed, failed, not run, and blocked by the environment.
- Reduce failures to a minimal reproduction where practical, and determine whether the cause lies in the implementation, the expectation, the test itself, or the environment.
- After a fix, verify the failing path and affected behavior again. Avoid repeated verification when no new changes or concerns justify it.
- Summarize covered behaviors, gaps, and evidence for reproducing problems. Present planned verification separately from actual results.
