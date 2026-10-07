# Project Cherry v1 spec — wayfinder map

Label: wayfinder:map

## Destination

A v1 spec for Project Cherry: a Flutter phone app (iOS first, Android too) that helps an ADHD brain manage tasks and train time awareness. The spec covers the core loop, domain model, screens, logic rules, and tech stack, ready to hand to build sessions. Full depth: todos, breakdown, retrospective. Basic: weighted-random suggestion by time bucket. Very thin: similar-task estimate hints.

## Notes

- **Domain**: mobile app product design + Flutter engineering. Developer is a Flutter beginner (one prior app) — weigh simplicity and documentation quality.
- **Planning only**: tickets resolve decisions; no app code in this map (prototypes are throwaway).
- **Tracker**: this map and its tickets are local markdown in `.scratch/project-cherry/`. Once the spec is done, build stories/tasks go to GitHub issues on `Jakeplusplus/project-cherry`.
- **Skills**: `/grilling` + `/domain-modeling` for grilling tickets; `/prototype` for prototype tickets; `/research` for research tickets (findings in `research/<slug>.md` on a `research/<slug>` branch).
- **Standing constraints** (settled while charting — do not reopen without the user):
  - On-device only. No accounts, ever, unless a real reason to leave the device appears.
  - Built for the author first; avoid choices that block a public release later.
  - Phone only: iOS + Android. Author carries iOS — first test target.
  - Assistive tool, not a planner replacement.
  - Estimate is a **bucket** (short / medium / long / whole day); actual is **precise minutes**.
  - Three numbers per timed thing: estimate (before), felt guess (after, before reveal), actual (clock).
  - Timer is primary, with a forgiving repair path and a lower-confidence self-report fallback.
  - Micro-retro at every completion (light, skippable) + a pull-only insights screen. No rituals, no streaks.
  - Two levels only: task → steps. Steps are estimated and timed. A too-big step is promoted to its own task.
  - Capture requires title + bucket only. Optional: category, due date, important flag.
  - Category is the v1 similarity key (average actuals per category + bucket).
  - Suggestion = weighted random among tasks fitting the stated time bucket.
  - Breakdown = guided manual workflow at full depth; AI assistance only if on-device.
  - Notifications are timer-tied only: lock-screen presence + overrun check-in.
  - Stack chosen from research, not taste.

## Decisions so far

<!-- one line per closed ticket -->

- [State management and app architecture for a Flutter beginner](issues/04-state-management-architecture.md) — official Flutter MVVM (ChangeNotifier + provider + go_router) ranked first, Riverpod second; flips if a reactive-stream database is chosen. Input to the stack decision, not the decision itself.
- [Local persistence options](issues/03-local-persistence-options.md) — Drift (SQLite) recommended, sqflite fallback; Isar/Hive/Realm/Floor ruled out. Caveat: Drift 3 in alpha. Reactive query streams available, which favours Riverpod in the state-management ranking. Input to the stack decision.
- [On-device LLM feasibility for task breakdown](issues/01-on-device-llm-feasibility.md) — feasible only as an optional "suggest steps" button using OS-provided models (strong on iPhone 15 Pro+, weak on Android); no bundled model. Output quality unverified — spawned a prototype ticket.
- [Background timer and lock-screen presence from Flutter](issues/02-background-timer-lock-screen.md) — no background timer: store start timestamp, compute elapsed. Overrun check-in = scheduled local notification. iOS Live Activity needs no push server but needs a native SwiftUI widget extension and caps at 8 h; Android needs no native code. Device behaviour unverified — spawned a prototype ticket.

## Not yet specified

- **Insights screen content** — which patterns are shown and how; depends on what the micro-retro stores and on the evidence research.
- **Similar-task hint presentation** — where and when "your Chores short tasks average 22m" appears (at capture? at estimate?); depends on domain model and bucket decisions.
- **Backup / export** — on-device only means a lost phone is lost data. Some escape hatch (file export, OS backup behaviour) probably needed; shape unclear until persistence is chosen.
- **ADHD UX principles and accessibility** — friction budget, tone, visual load; likely falls out of the prototypes.
- **First-run experience** — what an empty app shows; matters more for the public door than for the author.
- **Final spec assembly** — format and location of the spec document once decisions are in.

## Out of scope

- Accounts, sync, cloud storage — violates the on-device constraint.
- Desktop and web targets — no sync means stranded data per device.
- Reminders, due-date alerts, calendar, scheduling, daily planning — planners already do this; the app assists, not replaces.
- Recurring tasks — strong similarity signal but large complexity; later effort.
- Energy level and context/location fields — capture friction; later effort.
- Smart similarity matching (fuzzy text, embeddings) — v1 uses category only.
- Monetization, store listing, marketing.
