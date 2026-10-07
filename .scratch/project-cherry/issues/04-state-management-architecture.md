# State management and app architecture for a Flutter beginner

Type: research
Status: resolved

## Question

Which state management and app structure suit a solo developer with one prior Flutter app: Riverpod, Bloc/Cubit, Provider, signals, or plain ChangeNotifier/MVVM as in the official Flutter architecture guide? Compare learning curve, boilerplate, testability, fit with a reactive local database, and current official Flutter team recommendations. Include navigation (go_router vs Navigator) briefly.

## Answer

Full findings: [`research/state-management-architecture.md`](https://github.com/Jakeplusplus/project-cherry/blob/research/state-management-architecture/research/state-management-architecture.md) on branch `research/state-management-architecture`.

- **Recommendation (researcher's judgment):** follow the official Flutter architecture guide — views, view models, repositories, optional services — with `ChangeNotifier` + `ListenableBuilder` view models, `provider` for dependency injection, repositories exposing database `Stream`s, and `go_router` for navigation.
- **What is official:** layering, MVVM, repositories, DI and fakes are "Strongly recommend" on docs.flutter.dev. The state library itself is only "Conditional" — personal preference; riverpod, flutter_bloc and signals are named as valid alternatives.
- **Ranked shortlist:** (1) official MVVM with `ChangeNotifier` + `provider` — only option with an end-to-end official case study, no third-party churn, cost is hand-written stream subscribe/dispose and loading/error state; (2) Riverpod 3.x without codegen — best fit for a reactive database, but more concepts and recent/likely breaking majors; (3) Cubit (`flutter_bloc`) — stable, more files, same manual plumbing; (4) signals — least boilerplate, single maintainer, frequent breaking majors.
- **Navigation:** `go_router` (published by flutter.dev, feature-complete, graded "Recommend").
- **What flips ranks 1 and 2:** if persistence lands on a store with query streams (e.g. Drift) and a steeper start is acceptable, Riverpod removes the most plumbing.
- **Caveats:** no primary source compares these on learning curve or boilerplate, so the ranking is judgment on documented facts. Package figures were read through a summarising fetch and only partly cross-checked. Riverpod 4.0 timing and `provider`'s long-term status unverified.
