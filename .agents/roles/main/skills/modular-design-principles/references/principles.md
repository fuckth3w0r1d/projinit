# Principles in depth

One section per principle: **definition**, **rules for agents**, **abstract example**. No stack or folder assumptions.

---

## 1 — Well-defined boundaries

**Definition.** Consumers depend on a **small, intentional public surface** (operations, events, types that are part of the contract). Everything else is implementation detail.

**Rules for agents.**

- Prefer extending behavior by adding to the **documented** API rather than importing internals.
- When suggesting refactors, preserve or shrink the public surface; do not widen it “for convenience.”
- Name things so **contract vs internal** is obvious in reviews (e.g. “public operation” vs “internal helper” is a conceptual distinction even without tooling).

**Abstract example.** A “Checkout” context exposes `placeOrder(command)` and `OrderPlaced` events. Other contexts must not reach into Checkout’s internal pricing tables; they subscribe to events or call `placeOrder`, not “update row X.”

---

## 2 — Composability

**Definition.** Modules can be **assembled in different products or deployments** without rewriting their core logic for each combination.

**Rules for agents.**

- Avoid hidden assumptions like “this only runs when module B is present” unless expressed as an **optional integration** or **plugin** contract.
- Configuration and feature flags should not become spaghetti that only one deployment understands.

**Abstract example.** The same “Inventory” module works in a small CLI tool and a large web app because its contract does not assume a specific UI or host—only the composition root changes.

---

## 3 — Independence

**Definition.** Modules do not rely on **hidden shared mutable state** across boundaries. Tests can run a module with **fakes** at its edges.

**Rules for agents.**

- Flag “global singletons” that encode cross-module policy without an explicit contract.
- Prefer **passing dependencies explicitly** or **declared injection** over ambient globals for cross-cutting concerns.

**Abstract example.** Two services in different modules both mutate a process-wide cache keyed by “user id” without coordination → independence is violated; replace with an explicit cache interface owned by one module or a documented shared service.

---

## 4 — Individual scale

**Definition.** **Throughput, storage, batching, and limits** can be tuned per module where needed, without forcing one global setting on everyone.

**Rules for agents.**

- When performance tuning, ask **which bounded context** owns the bottleneck; avoid “fixing” by coupling unrelated code paths.
- Suggest **per-module** quotas, pools, or batch sizes when load profiles differ.

**Abstract example.** “Search” needs a large read replica and aggressive caching; “Billing” needs strict serial writes. Scaling policies are not identical, and neither module forces the other’s settings.

---

## 5 — Explicit communication

**Definition.** All **cross-module** interaction goes through **known contracts**: APIs, messages, events, or versioned schemas—not incidental shared files or implicit side channels.

**Rules for agents.**

- Make inputs, outputs, and errors clear at boundaries, using existing code contracts when sufficient. Specify version compatibility for external or persisted contracts that require it.
- Discourage “just import this DTO from their package” when that DTO is really an **internal** persistence shape.

**Abstract example.** Module A notifies Module B via `OrderPlaced { orderId, placedAt }` on a bus, not by writing into B’s database “because it’s faster.”

---

## 6 — Replaceability

**Definition.** Dependencies on other modules are expressed in terms of **interfaces, protocols, or stable contracts** so implementations can be swapped or mocked.

**Rules for agents.**

- Reuse existing interfaces at boundaries. Introduce a narrow interface when actual dependency isolation, replacement, or testing needs cannot be met by the existing one.
- Refactors that **pin** a module to one technology everywhere should be questioned unless that is a deliberate platform choice.

**Abstract example.** “Notifications” depends on `Notifier` with `send(recipient, body)`; email vs SMS vs push is replaceable behind that port.

---

## 7 — Deployment independence

**Definition.** When independent deployment is an actual requirement, module contracts make process, host, and release assumptions explicit. Modules within a monolith may use direct in-process calls.

**Rules for agents.**

- For independently deployed units, define the required transport and compatibility contract.
- Keep direct calls when they fit the current deployment model; hypothetical distribution does not justify transport abstractions.

**Abstract example.** Two services released separately agree on compatible request and response formats; two modules deployed in one process communicate through their existing functions.

---

## 8 — State isolation

**Definition.** Each module **owns** its authoritative store and naming for its facts. No silent sharing of the same logical data across boundaries without a **clear rule** (who writes, who reads, how consistency is achieved).

**Rules for agents.**

- Treat **reach-through persistence** (reading/writing another module’s store directly) as a **design smell** unless documented as an exceptional, reviewed pattern.
- Require **unambiguous names** for persisted concepts when multiple modules have similar nouns.

**Abstract example.** “Customer” in CRM and “Customer” in Billing are different aggregates with different IDs or explicit mapping—not two modules updating one ambiguous `customers` row.

---

## 9 — Observability

**Definition.** Logs, metrics, traces, and health checks can be **attributed** to a module (and often a use case) so incidents are diagnosable without reading the whole system.

**Rules for agents.**

- When adding diagnostics, include **context** (which operation, which correlation id), not only “error happened.”
- Avoid log lines that **cannot** be filtered by owning team or subsystem.

**Abstract example.** A failed payment shows `billing.capture` span with `orderId` and clear error code; support does not grep unrelated modules’ noise to find root cause.

---

## 10 — Fail independence

**Definition.** Failure handling matches the actual boundary and impact. Remote dependencies or independent resource pools may need timeouts or isolation; local calls need clear error and state semantics.

**Rules for agents.**

- Define failure semantics at module boundaries and timeouts for remote calls. Add retries, bulkheads, or circuit breaking only for a demonstrated requirement or failure risk.
- Async handlers should be **idempotent** or deduplicated where duplicates are possible.

**Abstract example.** When Recommendations is down, Checkout still completes using defaults or a cached tier; the UI degrades instead of blocking purchase.
