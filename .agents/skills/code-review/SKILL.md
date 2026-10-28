---
name: code-review
description: Review code changes for requirements alignment, architectural contracts, correctness, security, and regression risks. Use for reviewing or reassessing a PR, diff, specified implementation, or design.
---

# Code Review

Start from the intent and actual impact of a change, and ground review judgments in code and verifiable evidence.

## Establish scope

Use the task's PR, diff, file paths, requirements, and architecture documents to identify what to review. Understand the intended behavior and relevant contracts before inspecting the implementation and its callers. Identify missing information when the available context does not establish the scope.

Start from `docs/README.md` to locate project documentation. When reviewing formal documentation, use the shared [documentation-governance](../documentation-governance/SKILL.md) criteria and verify claims against the implementation.

## Review method

1. Trace key calls and data flows, checking preconditions, state changes, external effects, and failure paths.
2. Compare the implementation with requirements, acceptance criteria, and contracts. When documentation and code disagree, determine whether the cause is an implementation defect, outdated documentation, or a design tradeoff that needs reconsideration.
3. Check whether relevant tests cover actual behavior and regression risks. Distinguish deductions from code, existing verification results, and verification performed during this review.
4. State the conditions under which a suspected problem occurs. Verify its impact through the call chain or a feasible check, and mark findings with insufficient evidence as unresolved.

Select areas of focus according to the change:

| Dimension | Questions to examine |
| --- | --- |
| Requirements and architecture | Does the change deliver the intended behavior and respect module responsibilities, dependency direction, state ownership, and public contracts? |
| Correctness | Boundary inputs, state transitions, error propagation, concurrency, transactions, and resource lifecycles |
| Security | Relevant input and authorization boundaries, including injection, unauthorized access, and sensitive data exposure |
| Performance | Query amplification, unbounded growth, complexity in hot paths, or resource leaks at the expected scale |
| Maintainability | Coupling, duplication, or complexity that increases the cost of change, and missing verification of critical behavior |

## Form conclusions

- For each finding, provide the code location, triggering conditions, cause, impact, and suggested direction. Assess severity from the impact and conditions of occurrence.
- Distinguish confirmed defects, unresolved questions, and optional improvements. Do not present personal style preferences as defects.
- Lead with findings that affect delivery. State plainly when no clear issues were found.
- Describe the actual review scope, evidence sources, and remaining uncertainty. Do not present a partial review as comprehensive assurance.
