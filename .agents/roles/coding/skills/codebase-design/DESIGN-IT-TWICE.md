# Design It Twice

Compare meaningfully different alternatives when an interface or internal structure involves worthwhile tradeoffs. Use the design vocabulary in [SKILL.md](SKILL.md) alongside the project's domain language.

## Frame the problem

Describe the intended behavior, calling scenarios, existing contracts, and constraints. Identify relevant dependencies and their characteristics, consulting [DEEPENING.md](DEEPENING.md) when useful. Ground the problem in a small code sketch or usage example, and identify unknowns that could affect the comparison.

## Develop alternatives

Choose as many alternatives as the problem warrants, without a fixed count. When a public contract already exists, first compare implementations within that contract. If considering an interface change, explain its effects on callers, compatibility, and migration cost.

Possible areas of focus, depending on actual needs:

- Reduce the interface and rules callers must understand.
- Support existing use cases.
- Simplify the most common calling path.
- Isolate external dependencies that need substitution or testing.

For each alternative, describe its interface, a usage example, the complexity it hides, its handling of dependencies, and its main costs. Alternatives should differ in substance, rather than merely renaming elements or adding hypothetical extensibility requirements.

## Compare and recommend

Compare the capabilities provided by the interface, how well changes and knowledge stay localized, the placement of seams, and implementation and migration costs. Recommend an approach based on concrete calling scenarios, explain the reasoning, and identify conditions that could change the recommendation and assumptions that still need verification.
