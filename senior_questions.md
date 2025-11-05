## Senior-level Interview Questions — Flutter / Dart

This file contains only seniority-level technical interview questions. No answers are included here — use these prompts during interviews to evaluate architecture, design, and deep technical knowledge for senior Flutter/Dart engineers.

## Table of Contents

- [Mixin & Inheritance](#mixin--inheritance) (Q1-5)
- [Spread Operator & Collection Handling](#spread-operator--collection-handling) (Q6-8)
- [Operator Overloading & API Design](#operator-overloading--api-design) (Q9-11)
- [Mixins vs Extensions vs Composition](#mixins-vs-extensions-vs-composition--architectural-decisions) (Q12-14)
- [Incremental Development, Hot Reload & Build](#incremental-development-hot-reload--build-strategy) (Q15-17)
- [Polymorphism, Generics & Variance](#polymorphism-generics--variance) (Q18-20)
- [`identical`, Equality & Hashing](#identical-equality--hashing) (Q21-22)
- [Advanced Algorithmic / Factorial](#advanced-algorithmic--factorial--big-integer-handling) (Q23-25)
- [System Design & Real-world Trade-offs](#system-design--real-world-trade-offs) (Q26-28)
- [Behavioral / Process-focused](#behavioral--process-focused-technical-probes) (Q29-30)
- [StatefulWidget Lifecycle & State Management](#statefulwidget-lifecycle--state-management) (Q31-34)
- [Async Event Loop: Future, Microtask, Then](#async-event-loop-future-microtask-then) (Q35-38)

---

### Mixin & Inheritance

1. Describe a production scenario where you would prefer a mixin over inheritance and explain the trade-offs in terms of lifecycle, testability, and binary size.
2. How can `mixin on` constraints and `mixin class` declarations be used to enforce safe usage patterns in a large codebase? Give a concrete example.
3. Explain a situation where inheritance violated the Liskov Substitution Principle in a Flutter codebase you worked on. How did you refactor it and why?
4. When migrating a deep inheritance hierarchy to composition/mixins, what compatibility and API-stability concerns must you consider for downstream packages?
5. Discuss pitfalls and mitigation strategies when mixins carry mutable state shared across different classes that use them.

### Spread Operator & Collection Handling

6. In a heavily memory-constrained Flutter app, what performance pitfalls arise from liberal use of the spread operator in widget builders, and how would you avoid them?
7. When merging multiple maps from different sources (user, remote, defaults) using spread, how would you ensure deterministic conflict resolution and avoid silent data loss?
8. Explain how const and canonicalization interact with spread in Dart, and when using `const` can change runtime behavior unexpectedly.

### Operator Overloading & API Design

9. Provide a design for a strongly-typed physical units library in Dart (e.g., meters, seconds) that uses operator overloading safely. What compile-time and runtime checks would you implement to avoid unit-mismatch bugs?
10. Discuss when operator overloading improves or reduces API clarity. Give two examples where operator overloading made code clearer and two where it introduced subtle bugs.
11. If you override `==` on a widely-used model type, what steps do you take to ensure backward compatibility and correct behavior with `Set` and `Map`? Describe migration strategies.

### Mixins vs Extensions vs Composition — Architectural Decisions

12. Given a cross-cutting concern like caching or debounce that multiple controllers need, compare and justify three implementations: mixin, extension method wrapping a private helper, and composition with a separate service.
13. When is it appropriate to expose behavior via extension methods rather than changing class hierarchies? What are the testing and discoverability trade-offs?
14. How would you design a plugin API that allows third-party packages to add behavior to core classes safely, without exposing internal state or breaking encapsulation?

### Incremental Development, Hot Reload & Build Strategy

15. Explain the internals of Flutter's hot reload/hot restart. What changes are reliably applied by hot reload and which require a restart or full rebuild? Give examples from native plugins and static initialization.
16. In a large monorepo with multiple Flutter apps, outline an incremental CI strategy that minimizes build time while ensuring deterministic release artifacts.
17. Describe how you'd implement a staged rollout (feature flagging) for a UI feature in Flutter, including telemetry, rollback, and A/B testing considerations.

### Polymorphism, Generics & Variance

18. Discuss covariance and contravariance in Dart generics. Provide a concrete example showing how incorrect variance assumptions can cause runtime type errors in Flutter widgets or services.
19. Design a generic repository/cache abstraction for different model types that balances type-safety, performance, and testability. Show the API surface and justify variance choices.
20. Explain how you would reason about and test polymorphic behavior in a complex widget tree that accepts plugin-provided child widgets of unknown concrete types.

### `identical`, Equality & Hashing

21. Describe specific cases where `identical(a,b)` returns true but `a == b` is overridden with different semantics, and why relying on canonicalization is risky.
22. You find inconsistent behavior in a production `Map` lookup after changing a model's `==` implementation. Explain how to triage and fix it in a backward-compatible way.

### Advanced Algorithmic / Factorial & Big Integer Handling

23. You must compute factorials for values up to 100k in a server-side Dart service. Discuss algorithms, BigInt considerations, memory/performance trade-offs, and parallelization options (isolates).
24. Explain the binary-splitting (divide-and-conquer) approach for large factorial computation and why it outperforms naive iterative multiplication for very large n.
25. How would you design an API and testing strategy for a numerical library that exposes factorial, modular factorial, and factorial-streaming computations while preventing DoS from very large inputs?

### System Design & Real-world Trade-offs

26. Design a modular Flutter architecture that allows replacing UI state-management strategies (Provider, Bloc, Riverpod) without touching feature widgets. What patterns enable this pluggability and how do you preserve testability?
27. Explain the trade-offs of shipping many small Dart isolates for CPU-bound workloads in a Flutter app vs handling them on a remote server. Discuss UX, battery, and security implications.
28. For a cross-platform app that must interop with existing native code, describe strategies to incrementally replace native implementations with Dart equivalents while keeping parity and minimizing regressions.

### Behavioral / Process-focused Technical Probes

29. Describe a time you introduced a language-level or architectural change (mixins, generics, or operator overloads) across a large codebase. How did you manage migration, code review, and rollback?
30. When mentoring mid-level engineers, what concrete exercises or review checklist items do you use to teach safe use of mixins, operator overloading, and BigInt-heavy algorithms?

### StatefulWidget Lifecycle & State Management

31. Explain the complete lifecycle of a StatefulWidget from construction to disposal. In what scenarios might `didUpdateWidget` be called, and what are common pitfalls when overriding it?
32. You find a memory leak in a production app traced to a StatefulWidget not disposing controllers. Design a systematic approach (static analysis, lint rules, code review checklist) to prevent this class of bug.
33. Discuss when you'd use `State.setState` vs `InheritedWidget` vs `ValueNotifier` for propagating state changes. Include performance and rebuild scope trade-offs.
34. How would you debug and fix a scenario where a widget's `build` method is called unexpectedly often, causing jank? What tools and techniques apply?

### Async Event Loop: Future, Microtask, Then

35. Explain the difference between the microtask queue and the event queue in Dart's event loop. Give a concrete example where scheduling a microtask vs a Future changes program behavior.
36. You have a performance-sensitive async workflow that must minimize latency. When would you use `scheduleMicrotask` vs `Future` vs `Future.sync`, and what are the trade-offs?
37. Describe a bug caused by incorrect ordering of `Future.then` vs `await`, and how you diagnosed and fixed it. Include discussion of error propagation and stack traces.
38. Design an API for a plugin that exposes async methods with controllable backpressure and cancellation. How do you handle microtasks, StreamControllers, and error zones?

---

File purpose: senior-level interview prompts only (no answers). Use these as live interview prompts or as the basis for take-home tasks.
