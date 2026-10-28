# Deepening

How to deepen a cluster of shallow modules safely, given its dependencies. Assumes the vocabulary in [SKILL.md](SKILL.md): **module**, **interface**, **seam**, **adapter**.

## Dependency categories

When assessing a candidate for deepening, classify its dependencies. The category determines how the deepened module is tested across its seam.

### 1. In-process

Pure computation, in-memory state, no I/O. When responsibilities are cohesive and fit the agreed boundaries, internal implementations can be merged and tested through existing contracts. Pure computation alone does not justify adding an adapter.

### 2. Local-substitutable

Dependencies that have local test stand-ins (PGLite for Postgres, in-memory filesystem). Deepenable if the stand-in exists. The deepened module is tested with the stand-in running in the test suite. The seam is internal; no port at the module's external interface.

### 3. Remote but owned

Your own services across a network boundary (microservices, internal APIs). Reuse an existing client, function, or module interface when it provides the required contract and testability. Introduce a port and adapter only when a concrete isolation, substitution, or testing need cannot be met through that interface.

Choose test doubles or integration tests according to the behavior being checked. A network dependency alone does not require another wrapper or an in-memory implementation.

### 4. True external

Third-party services (Stripe, Twilio, etc.) you don't control. Prefer the project's existing client or the vendor's supported testing facilities. Add a narrow adapter only when an actual contract, isolation, or testing requirement needs it; mocks do not establish the real service's behavior.

## Seam discipline

- **Abstractions need concrete reasons.** Decide whether to introduce a port based on agreed contracts, dependency isolation, and testing needs, not a minimum number of adapters.
- **Internal seams vs external seams.** A deep module can have internal seams (private to its implementation, used by its own tests) as well as the external seam at its interface. Don't expose internal seams through the interface just because tests use them.

## Testing strategy: preserve behavior coverage

- Preserve the behaviors, boundary conditions, and regression cases covered by existing tests. Migrate or remove redundant cases only when refactoring makes the old tests inapplicable and replacement tests cover the same verification goals and have actually passed; explain the changes in the results. Do not weaken, skip, or delete tests merely to obtain a passing result.
- Write new tests at the deepened module's interface. The **interface is the test surface**.
- Tests assert on observable outcomes through the interface, not internal state.
- Tests should survive internal refactors, since they describe behaviour, not implementation. If a test has to change when the implementation changes, it's testing past the interface.
