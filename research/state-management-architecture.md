# State management and app architecture for Project Cherry

Research for ticket `04-state-management-architecture`. Sources checked 2026-10-07.

Conventions in this file: **[Fact]** means the cited page says it. **[Judgment]** means it is my inference for this project and no source states it. Package numbers (versions, dates, likes) were read from pub.dev on 2026-10-07 and will drift.

## Answer

**Bottom line [Judgment]:** build Cherry on the structure in the official Flutter architecture guide (views, view models, repositories, optional services), and implement the view models with the tools the guide itself uses: `ChangeNotifier` + `ListenableBuilder`, `provider` for dependency injection, repositories that expose `Stream`s from the local database, and `go_router` for navigation. Riverpod is the one alternative worth a serious look; it fits a reactive local database better but costs more to learn and has more API churn.

The layering matters more than the library. Every option below can sit on top of the same repositories, so the state library can be swapped later without touching the data layer.

Ranked shortlist:

| Rank | Option | Why it lands here |
|---|---|---|
| 1 | **Official MVVM: `ChangeNotifier` view models + `provider` (DI) + repository `Stream`s** | The only option the Flutter docs teach end to end, with a full case-study app. No third-party API to break. Cost: you hand-write stream subscriptions, disposal, and loading/error state. |
| 2 | **Riverpod 3 (without code generation)** | Best fit with a reactive database: a `StreamProvider` turns a database `watch()` stream into loading/data/error state with no subscription code. Cost: more concepts, a breaking 3.0 in Sept 2025, and the maintainer has flagged a possible 4.0. |
| 3 | **Cubit (`flutter_bloc`)** | Most stable API and strong testing docs. Cost: more files per feature, and stream subscriptions inside a cubit are still manual, so it buys little over option 1 for this app. |
| 4 | **signals** | Least boilerplate, but smallest community, single maintainer, and three breaking majors. Named by the Flutter docs as a valid alternative, not taught by them. |
| n/a | **Provider on its own** | Not a separate choice: it is the DI half of option 1. Still a Flutter Favorite, low activity. |

**What flips rank 1 and 2 [Judgment]:** if the persistence ticket (03) picks a store with first-class query streams (for example Drift) and the developer is willing to learn one library's concepts up front, Riverpod removes the most hand-written plumbing. If the priority is "follow one official tutorial and never fight a library upgrade", stay with rank 1.

**Navigation [Judgment, backed by the facts below]:** use `go_router`. The Flutter docs recommend it, the case study wires view models inside its route builders, and it is published by the Flutter team.

**Biggest caveat:** no primary source compares these options for learning curve or boilerplate. The Flutter team explicitly declines to pick a state-management package ("ultimately the decision comes down to personal preference"). The ranking is my judgment built on documented facts, not a documented recommendation.

## What the Flutter team officially recommends (late 2026)

Source for this whole section unless noted: https://docs.flutter.dev/app-architecture/recommendations (page last updated 2026-05-05).

The page grades each recommendation. **[Fact]** "Strongly recommend" is defined as "You should always implement this recommendation if you're starting to build a new application"; "Recommend" as "This practice will likely improve your app"; "Conditional" as "This practice can improve your app in certain circumstances."

| Recommendation | Strength | Relevance to Cherry |
|---|---|---|
| Clearly defined data and UI layers | Strongly recommend | Adopt |
| Repository pattern in the data layer | Strongly recommend | Adopt; repositories wrap the local database |
| ViewModels and Views in the UI layer (MVVM) | Strongly recommend | Adopt |
| `ChangeNotifier`s and `Listenable`s for widget updates | **Conditional** | The state-library question; see below |
| Do not put logic in widgets | Strongly recommend | Adopt |
| Domain layer (use-cases) | Conditional | Skip for v1; the page says it "adds unnecessary overhead in most apps" |
| Unidirectional data flow | Strongly recommend | Adopt |
| `Command`s for user-interaction events | Recommend | Optional helper |
| Immutable data models | Strongly recommend | Adopt |
| `freezed` or `built_value` for models | Recommend | Optional; the page warns these "can add significant build time" |
| Separate API and domain models | Conditional | Not relevant without a network API |
| Dependency injection | Strongly recommend | "We recommend you use the provider package" |
| `go_router` for navigation | Recommend | See Navigation |
| Abstract repository classes | Strongly recommend | Adopt; enables fakes in tests |
| Test components separately and together; make fakes | Strongly recommend | Adopt |

Key points on state management:

- **[Fact]** On the `ChangeNotifier` row the page says: "There are many options to handle state-management, and ultimately the decision comes down to personal preference." https://docs.flutter.dev/app-architecture/recommendations
- **[Fact]** The state-management options page lists built-in approaches (`setState`, `ValueNotifier`/`InheritedNotifier`, `InheritedWidget`/`InheritedModel`) and, for packages, points to the pub.dev `#state-management` topic rather than naming any. It says the best choice "often depends on the app's complexity, your team's preferences, and the specific problems you need to solve." Page last updated 2026-07-31. https://docs.flutter.dev/data-and-backend/state-mgmt/options
- **[Fact]** The case study states its app "leans heavily on view models and `ChangeNotifier`, but it could've easily been written with streams, or with other libraries such as riverpod, flutter_bloc, and signals." https://docs.flutter.dev/app-architecture/case-study
- **[Fact]** The DI page says "teams at Google recommend using `package:provider` to implement dependency injection." https://docs.flutter.dev/app-architecture/case-study/dependency-injection
- **[Fact]** The older "Simple app state management" page still says of `provider`: "If you are new to Flutter and you don't have a strong reason to choose another approach (Redux, Rx, hooks, etc.), this is probably the approach you should start with." Page last updated 2026-05-29. https://docs.flutter.dev/data-and-backend/state-mgmt/simple
- **[Fact]** The architecture guide calls its advice "guidelines, not steadfast rules." https://docs.flutter.dev/app-architecture/guide

**[Judgment]** Read together: the layering (views, view models, repositories, DI, fakes) is strongly recommended; the specific state library is left open; `ChangeNotifier` + `provider` is the documented default path and the only one with an official worked example.

### The official structure in brief

Source: https://docs.flutter.dev/app-architecture/guide (last updated 2026-05-05).

- **[Fact]** Two layers. UI layer = views (widgets, no business logic) + view models ("most of the logic in your Flutter application lives in view models"). Data layer = repositories ("the source of truth for your model data") + services (wrap platform APIs, files, endpoints; expose `Future`/`Stream`).
- **[Fact]** "Views and view models should have a one-to-one relationship." "Repositories should never be aware of each other."
- **[Fact]** The guide gives an example of a repository exposing a `Stream` (a `UserProfileRepository` exposing `Stream<UserProfile?>`).
- **[Fact]** The domain layer is optional, for logic that merges several repositories, is very complex, or is reused across view models.

**[Judgment]** Mapping to Cherry: `TaskRepository`, `TimerSessionRepository`, `RetroRepository` (or similar; naming belongs to ticket 07) wrap the database and expose streams and write methods. One view model per screen. A use-case layer is not needed for v1, with one possible exception: the suggestion picker and the estimate-rollup rules are pure logic that may be reused, and can be plain Dart classes without committing to a full domain layer.

## Comparison

Versions and activity, all read 2026-10-07:

| Package | Latest | Last published | Publisher | Source |
|---|---|---|---|---|
| `provider` | 6.1.5+1 | ~13 months ago | dash-overflow.net; Flutter Favorite | https://pub.dev/packages/provider |
| `flutter_riverpod` | 3.4.3 | 2026-09-04 | dash-overflow.net | https://pub.dev/packages/flutter_riverpod , https://pub.dev/packages/flutter_riverpod/changelog |
| `flutter_bloc` | 9.1.1 | ~17 months ago | bloclibrary.dev; Flutter Favorite | https://pub.dev/packages/flutter_bloc |
| `bloc` (core) | 9.2.1 | ~4 months ago | bloclibrary.dev | https://pub.dev/packages/bloc |
| `signals` | 7.1.0 | ~4 months ago | rodydavis.com | https://pub.dev/packages/signals |
| `go_router` | 18.0.2 | ~9 days ago | flutter.dev | https://pub.dev/packages/go_router |

Side by side. The Facts columns are cited in the per-option sections; "Learning curve" and "Boilerplate" are **[Judgment]** throughout, because no primary source measures them.

| | Official MVVM (ChangeNotifier + provider) | Riverpod 3 | Cubit / Bloc | signals |
|---|---|---|---|---|
| Official Flutter docs coverage | Full guide + case-study app | Named as alternative | Named as alternative | Named as alternative |
| Learning curve [Judgment] | Lowest: SDK classes plus one small package | Medium-high: providers, `ref`, `AsyncValue`, auto-dispose, families | Medium: cubit, state classes, `BlocBuilder`/`BlocListener` | Low for basics; fewer learning resources |
| Boilerplate [Judgment] | Medium: manual subscribe/dispose, manual loading/error | Lowest for stream-backed screens | Highest: state classes per feature | Low |
| Reactive DB fit | Manual `listen` + `notifyListeners` | `StreamProvider` consumes a stream directly | Manual subscription inside the cubit | `streamSignal` exists |
| Testability | Plain Dart classes with fake repositories | Overrides on `ProviderContainer.test()` / `ProviderScope` | `bloc_test` package | Not verified |
| API stability | SDK; `provider` barely changes | Breaking 3.0 (2025-09); 4.0 flagged as possible | Stable on 9.x | Breaking majors 5, 6, 7 |
| Code generation | None needed | Optional | None | None |

### Option 1: official MVVM with ChangeNotifier + provider

- **[Fact]** View models extend `ChangeNotifier`, receive repositories through their constructor, and call `notifyListeners()`; views take the view model as a constructor argument and rebuild through `ListenableBuilder`. https://docs.flutter.dev/app-architecture/case-study/ui-layer
- **[Fact]** Repositories and services are registered at the top of the tree with `MultiProvider`, and repositories are handed to view models in the router configuration. https://docs.flutter.dev/app-architecture/case-study/dependency-injection
- **[Fact]** To consume a repository `Stream`, the docs' offline-first recipe has the view model call `.listen(...)`, store the value, and call `notifyListeners()`. https://docs.flutter.dev/app-architecture/design-patterns/offline-first
- **[Fact]** The case study hand-writes a `Command` class to track running/error/completed state for actions, and suggests the `command_it` package to avoid writing your own. https://docs.flutter.dev/app-architecture/case-study/ui-layer
- **[Fact]** The Flutter getting-started pathway teaches "`ChangeNotifier` to update app state" and "`ListenableBuilder` to update app UI" as its state lessons. https://docs.flutter.dev/get-started/fwe/state-management
- **[Fact]** `provider` is a Flutter Favorite at 6.1.5+1, published about 13 months ago. The repo's recent commits are sparse: 2026-09-29, 2026-03-10, then 2025-08-19. https://pub.dev/packages/provider , https://github.com/rrousselGit/provider/commits/master
- **[Fact]** The `provider` README has no testing section. https://github.com/rrousselGit/provider (README)
- **[Judgment]** Testability is good regardless, because view models are plain Dart classes with constructor-injected repositories, which is exactly the "make fakes" approach the recommendations page asks for.
- **[Judgment]** Weak points for Cherry: every screen that watches the database repeats the same subscribe / cancel-in-`dispose` / track-loading / track-error code, and a forgotten `cancel()` is a leak a beginner can miss. A small shared base class or helper removes most of it. `provider` being quiet is low risk: it is small, stable, and the Flutter docs depend on it.

### Option 2: Riverpod 3

- **[Fact]** Riverpod describes itself as "a reactive caching and data-binding framework" that handles "errors/loading states by default". https://pub.dev/packages/flutter_riverpod
- **[Fact]** Provider types form a matrix: `Provider`, `FutureProvider`, `StreamProvider` (read-only) and `NotifierProvider`, `AsyncNotifierProvider`, `StreamNotifierProvider` (modifiable through methods). https://riverpod.dev/docs/concepts2/providers
- **[Fact]** Drift's stream docs name Riverpod's `StreamProvider` alongside `StreamBuilder` as a way to consume its query streams. https://drift.simonbinder.eu/dart_api/streams/
- **[Fact]** Testing: `ProviderContainer.test()` for unit tests, `ProviderScope` overrides for widget tests, every provider is overridable by default, and "It is generally discouraged to mock Notifiers" (mock their dependencies instead). https://riverpod.dev/docs/how_to/testing
- **[Fact]** Code generation is optional: "When using Riverpod, code generation is completely optional." The docs advise using it "Only if you already use code-generation for other things", note it is "still fairly slow", and say it was the recommended path until Dart macros were cancelled. https://riverpod.dev/docs/concepts/about_code_generation
- **[Fact]** 3.0.0 stable shipped 2025-09-10 with many breaking changes: `Ref` subclasses removed, `StateProvider` and `StateNotifierProvider` moved to a legacy import, `StreamProvider` pauses when unused, notifiers recreated on rebuild. https://pub.dev/packages/flutter_riverpod/changelog
- **[Fact]** `ChangeNotifierProvider` is also among the legacy providers described as "not recommended anymore". https://riverpod.dev/docs/whats_new
- **[Fact]** Offline persistence and mutations are experimental: "usable, but the API may change in breaking ways without a major version bump." https://riverpod.dev/docs/whats_new
- **[Fact]** The changelog (entry 3.0.0-dev.12, 2025-04-30) says 3.0 "is a transition version" and "It is quite possible that a 4.0.0 will be released relatively soon in the future." As of 2026-10-07 the latest is still 3.4.3, with releases through 2026 (3.3.2 in June, 3.4.0 in July, 3.4.3 in September). https://pub.dev/packages/riverpod/changelog
- **[Fact]** Riverpod's claimed advantages over `provider`: providers are plain Dart objects rather than widgets, multiple `ref.watch` calls replace `Consumer2`/`Consumer3`, `autoDispose`, and `family` for parameters. https://riverpod.dev/docs/from_provider/provider_vs_riverpod
- **[Judgment]** Fit for Cherry is strong: "list of tasks", "steps of task X" (a family keyed by task id), and "average actual minutes per category and bucket" are each one stream provider, and the UI gets loading/error/data for free. This is the least hand-written code of any option for a database-driven app.
- **[Judgment]** Risks for a beginner: two parallel syntaxes (codegen and non-codegen) in docs and tutorials, a large amount of pre-3.0 material online that no longer matches the API, and a maintainer-flagged possibility of another major version. Mitigation if chosen: no code generation, avoid everything under `experimental/` and `legacy`, and keep repositories free of Riverpod imports so the data layer survives a migration. Riverpod's experimental "offline persistence" is not a substitute for the app database and should not be used as one.
- **[Judgment]** Choosing Riverpod means departing from the official case study in DI and view-model wiring (Riverpod replaces `provider` and `ChangeNotifier`), while keeping its layering. The official tutorial code will no longer be copy-adaptable.

### Option 3: Cubit / Bloc

- **[Fact]** The bloc docs advise: "start with `Cubit` and you can later refactor or scale-up to a `Bloc` as needed." Cubit's stated advantage is simplicity; Bloc's is traceability (knowing what event triggered a change) and event transformers such as debounce. https://bloclibrary.dev/bloc-concepts/
- **[Fact]** `flutter_bloc` supplies `BlocBuilder`, `BlocListener`, `BlocProvider`, `BlocConsumer`, and is a Flutter Favorite. https://pub.dev/packages/flutter_bloc
- **[Fact]** Testing uses the `bloc_test` package's `blocTest` (build / act / expect); the docs state "Bloc was designed to be extremely easy to test." https://bloclibrary.dev/testing/
- **[Fact]** `flutter_bloc` 9.1.1 was published about 17 months ago; core `bloc` 9.2.1 about 4 months ago. https://pub.dev/packages/flutter_bloc , https://pub.dev/packages/bloc
- **[Judgment]** The slow release cadence reads as maturity rather than neglect, given the core package is still being published, but I did not verify issue-tracker activity.
- **[Judgment]** For Cherry, a Cubit is close to a `ChangeNotifier` view model with an immutable state object and an enforced `emit`. That is more disciplined, and the explicit state classes suit the timer (idle / running / paused / overrun). But a cubit that watches a database stream still subscribes and cancels by hand, so it adds a dependency and files per feature without removing the main plumbing cost of option 1. Full event-driven `Bloc` is more ceremony than a solo app with no complex event streams needs.

### Option 4: signals

- **[Fact]** `signals` is "based on Preact Signals" with automatic dependency tracking, lazy computed values, and widgets such as `SignalBuilder` for targeted rebuilds. https://pub.dev/packages/signals
- **[Fact]** It has `streamSignal` for streams, a DevTools extension, and a lint package. https://dartsignals.dev/
- **[Fact]** Version 7.1.0, published about 4 months ago, by a single verified publisher (rodydavis.com), with 712 likes against 2,910 for `flutter_riverpod` and about 8,000 for `flutter_bloc`. https://pub.dev/packages/signals , https://pub.dev/packages/flutter_riverpod , https://pub.dev/packages/flutter_bloc
- **[Fact]** Majors 5, 6 and 7 each carried breaking changes; 7.0 changed constructor parameters and the `SignalBuilder` signature, 6.0 switched the core implementation to `preact_signals`. https://pub.dev/packages/signals/changelog
- **[Judgment]** Attractive ergonomics, but the weakest on the map's stated criteria (simplicity and documentation quality for a beginner): the smallest body of tutorials and answered questions, and it gives no guidance on structure, so the developer would have to design the architecture conventions alone.

### Fit with a reactive local database

This depends on ticket 03, which is still open. The following assumes a store with query streams.

- **[Fact]** Drift exposes a `watch()` counterpart for each query method, returning a stream; "All drift streams will emit an up-to-date result after listening to them"; queries are rescheduled "Whenever an insert, an update, or a deletion is made through drift APIs". https://drift.simonbinder.eu/dart_api/streams/
- **[Fact]** Caveats from the same page: changes made outside Drift's APIs do not trigger updates, streams can emit more often than strictly necessary, and watched queries should cover "relatively few rows and not be too computationally expensive". https://drift.simonbinder.eu/dart_api/streams/
- **[Fact]** Drift's FAQ recommends a single database instance shared through `provider`, or a DI container such as `get_it`. https://drift.simonbinder.eu/faq/
- **[Judgment]** Whichever library is chosen, the shape is the same: database -> repository method returning `Stream<DomainModel>` -> view model / provider / cubit -> widget. Writes go through repository methods and the UI updates because the stream re-emits, which gives the unidirectional flow the official guide asks for without manual refresh calls.
- **[Judgment]** The insights aggregates (averages per category and bucket) are the queries most likely to hit Drift's "not too computationally expensive" caveat. For those, a one-shot `Future` fetched when the insights screen opens is a reasonable alternative to a live stream.
- **[Judgment]** The running timer should not be a per-second database stream. Per ticket 02's framing, elapsed time is derived from a stored start timestamp; the view model only needs a local periodic tick to repaint. All four options handle this equally.

## Navigation: go_router vs Navigator

- **[Fact]** Recommendations page, strength "Recommend": "Go_router is the preferred way to write 90% of Flutter applications. There are some specific use-cases that go_router doesn't solve, in which case you can use the Flutter Navigator API directly or try other packages." https://docs.flutter.dev/app-architecture/recommendations
- **[Fact]** The navigation overview says `Navigator` suits small apps without complex deep linking, and Router-based packages such as `go_router` suit apps with advanced navigation needs (deep links to each screen, multiple `Navigator`s). It also says: "We don't recommend using named routes for most applications." Page last updated 2026-09-29. https://docs.flutter.dev/ui/navigation
- **[Fact]** `go_router` is published by flutter.dev, at 18.0.2 (about 9 days old on 2026-10-07), and its README states: "This package is considered feature-complete. The Flutter team's primary focus will be on addressing bug fixes and ensuring stability." https://pub.dev/packages/go_router
- **[Fact]** It supports redirection and `ShellRoute` for an inner `Navigator`, for example under a bottom navigation bar. https://pub.dev/packages/go_router
- **[Fact]** In the official case study, repositories are injected into view models in the router configuration. https://docs.flutter.dev/app-architecture/case-study/dependency-injection
- **[Judgment]** Use `go_router`. Cherry is phone-only with no web target, so plain `Navigator.push` would work, but three things tip it: the official example wires view models in route builders, so following the case study means following `go_router`; tapping the overrun check-in notification or the lock-screen timer must land on a specific task's timer screen, which is a route-by-path problem; and a bottom-bar shell is a likely screen shape (ticket 16). Skip the type-safe-routes code generation for v1.
- **[Judgment]** A version number of 18 implies many past breaking majors. "Feature-complete" suggests that pace has slowed, but I did not verify recent major-version cadence; expect occasional migration work on upgrade.

## Suggested structure for the spec (input to ticket 15)

**[Judgment]** throughout; folder names adapted from the official naming advice (https://docs.flutter.dev/app-architecture/recommendations, "Use standardized naming conventions").

```
lib/
  data/
    database/        # store setup, tables, migrations
    repositories/    # abstract class + local implementation per aggregate
    services/        # notifications, live activity, clock
  domain/
    models/          # immutable models (Task, Step, TimerSession, Retro)
    logic/           # pure Dart: suggestion picker, estimate rollup
  ui/
    core/            # shared widgets, theme
    <feature>/       # <feature>_screen.dart + <feature>_viewmodel.dart
  routing/           # go_router configuration
  main.dart          # provider setup
```

- Abstract repositories with fake implementations for tests, as the official page strongly recommends.
- Inject a clock service rather than calling `DateTime.now()` directly in timer logic, so timer and retro behaviour can be unit-tested.
- Decide once whether to use a `Command` helper (hand-written or `command_it`) or plain async methods on view models. With stream-driven reads, most Cherry writes are short local inserts, so plain async methods are likely enough for v1.

## Unknowns / could not verify

- **No primary source compares learning curve or boilerplate** across these options. Those rows are my judgment. A one-evening spike building the same "task list from a watched query" screen in option 1 and option 2 would settle it better than any reading.
- **Whether and when Riverpod 4.0 ships.** The only statement found is the 2025 changelog note that it is "quite possible... relatively soon". I found no roadmap or date, and did not search the GitHub issue tracker.
- **`provider`'s long-term status.** No deprecation or maintenance-mode statement exists in its README or pub.dev page; low commit activity is the only signal. I could not confirm the author's current stated intent.
- **Whether `flutter_riverpod` or `signals` hold Flutter Favorite status.** Not seen on their pub.dev pages via the fetch tool; not confirmed either way.
- **pub.dev popularity figures** (likes, downloads) were read through a page-summarising fetch tool; download units were inconsistent between pages (some weekly, some unlabelled), so downloads are deliberately omitted from the comparison. Versions and dates were cross-checked against changelogs for Riverpod only.
- **The content of the "Use ChangeNotifier" lesson** linked from the recommendations page (`/get-started/fwe/state-management`): the fetch returned the learning-pathway index, which lists the `ChangeNotifier` and `ListenableBuilder` lessons, but I did not read the lesson text itself.
- **`command_it` health** (maintenance, version) was not checked.
- **Signals' Flutter API details** (`Watch`, `.watch(context)`, mixins) and its testing guidance were not verified beyond the landing page.
- **go_router's recent breaking-change cadence**, its minimum Flutter SDK, and `StatefulShellRoute` behaviour for preserving tab state were not verified.
- **bloc issue-tracker health** was not checked; the maturity reading of its slow release cadence is an inference.
- **Interaction with tickets 02 and 03.** The database fit section assumes a store with query streams (Drift was used as the documented example). If ticket 03 picks a store without them, Riverpod's main advantage shrinks and option 1 strengthens. Live Activity / notification plugins (ticket 02) may impose their own state or isolate constraints that were not examined here.
- **Drift + Riverpod official example.** Drift's stream page names `StreamProvider`, but I found no worked Drift-plus-Riverpod example in either project's official docs.
