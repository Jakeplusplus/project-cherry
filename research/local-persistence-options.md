# Local persistence options for Project Cherry

Research for ticket `03-local-persistence-options`. All package and repository facts were checked on 2026-10-07 against pub.dev, the package repositories and the official docs. Nothing here was verified by building an app; see "Unknowns" at the end.

## Answer

**Use Drift (SQLite).** It is the only candidate that meets every stated need at once: real relations with foreign keys, `GROUP BY` and `AVG` aggregates in the database, tooling-assisted and tested schema migrations, a single-file database that is easy to export, and unit tests that run on the laptop without a device. It is actively maintained and a Flutter Favorite.

The cost is code generation (`build_runner`) and a larger API to learn than the alternatives. The fallback if that proves too much is sqflite, which gives the same SQLite file and the same SQL with no codegen but no type safety.

**Biggest caveat:** Drift 3 is in active development. A `drift3_preview 3.0.0-alpha.0` package was published in mid-September 2026 and the maintainer is committing to it weekly. No release date, no support window for Drift 2, and no migration tool are published yet. An app started on Drift 2.35 today should expect a major-version upgrade during its life. The on-disk format is plain SQLite either way, so the data is not at risk; the Dart code is what would need changes.

### Ranked shortlist

| Rank | Option | Verdict |
|---|---|---|
| 1 | **Drift 2.35** | Recommended. Fits every requirement. Costs: codegen, Drift 3 upgrade later, effectively one maintainer. |
| 2 | **sqflite 2.4** | Sound fallback. Same SQLite strengths, no codegen, used by the official Flutter docs. You hand-write SQL strings, row mapping and migrations. |
| 3 | **ObjectBox 5.3** | Workable but a worse fit. Healthy and fast, but no `GROUP BY`, a proprietary native core, and no migration of a property's type. |
| 4 | **Hive CE 2.20** | Not for the main store. Healthy, but a key-value store with no queries, relations or aggregates. Fine for settings only. |
| 5 | **Isar (community fork 3.3)** | Avoid. Original is abandoned; the fork is bug-fix-only and slow-moving. No `GROUP BY`. |
| -- | Hive (original), Realm, Floor | Rule out. Unmaintained or deprecated. |

## Why the data shape decides this

The insights the map asks for are grouped aggregates over a join: average actual minutes per category and estimate bucket, and estimate accuracy over time. In SQL that is one statement, for example:

```sql
SELECT t.category_id, t.estimate_bucket, AVG(s.actual_minutes)
FROM tasks t JOIN timer_sessions s ON s.task_id = t.id
GROUP BY t.category_id, t.estimate_bucket;
```

The object stores (Isar, ObjectBox) offer whole-query aggregates such as `average()` but no documented grouping, so each category and bucket pair needs its own query or the grouping is done in Dart. Hive has no query layer at all. This is the main reason the SQLite options rank first; it is a fit argument, not a performance argument, since the data volume of a personal task app is tiny.

## Comparison

Versions, dates, download counts and pub points are from the pub.dev API. Repository figures are from the GitHub API. "Open" counts marked with * include open pull requests.

| | Drift | sqflite | ObjectBox | Hive CE | Isar community | Isar (original) | Hive (original) |
|---|---|---|---|---|---|---|---|
| Model | SQLite, typed query layer | SQLite, raw SQL | Object store | Key-value boxes | Object store | Object store | Key-value boxes |
| Latest stable | 2.35.2 (2026-10-07) | 2.4.4+1 (2026-10-05) | 5.3.2 (2026-05-20) | 2.20.2 (2026-10-06) | 3.3.2 (2026-03-23) | 3.1.0+1 (2023-04-25) | 2.2.3 (2022-06-30) |
| Pre-release | 3.0 alpha (separate preview package) | none | 6.0.0-preview.4 (2026-10-06) | none | none | 4.0.0-dev.14 (2023-08-21) | 4.0.0-dev.2 (2023-08-25) |
| Last commit | 2026-10-07 | 2026-10-05 | 2026-10-07 | 2026-10-06 | 2026-09-08 | 2025-06-14 | not verified |
| Commits since 2026-04-07 | 100+ | 99 | 100+ | 73 | 4 | 0 | not verified |
| Open issues | 202 | 5 | 73* | 6* | 28 | 173 | 558 |
| Downloads, 30 days | 1.47M | 3.32M | 174K | 1.20M | 105K | 3.5K | 1.07M |
| Pub points | 160/160 | 160/160 | 160/160 | 160/160 | 140/160 | 130/160 | 120/160 |
| Flutter Favorite | yes | yes | no | no | no | no | no |
| Relations | Foreign keys, joins | Foreign keys, joins (raw SQL) | ToOne / ToMany links | None in README | Links | Links | -- |
| Grouped aggregates | Yes, in the database | Yes, in raw SQL | Aggregates yes, grouping not documented | No query layer | Aggregates yes, grouping not documented | same | -- |
| Migrations | Versioned, generated step files and tests | Versioned callbacks, hand-written | Automatic add/remove; no type-change migration | Not covered in README | Automatic; renames need `@Name` | same | -- |
| Codegen | Required | None | Required | Optional but usual | Required | Required | -- |
| Host unit tests | In-memory, no setup | Via `sqflite_common_ffi` | Needs native library downloaded by script | not checked | not checked | -- | -- |
| Export | One SQLite file, `VACUUM INTO` | One SQLite file | Proprietary format | Proprietary box files | Proprietary format | -- | -- |
| Licence | MIT | BSD-2-Clause | Apache-2.0 bindings, proprietary core | Apache-2.0 / BSD-3 | Apache-2.0 | Apache-2.0 | -- |

Sources for the table:
[drift](https://pub.dev/packages/drift),
[sqflite](https://pub.dev/packages/sqflite),
[objectbox](https://pub.dev/packages/objectbox),
[hive_ce](https://pub.dev/packages/hive_ce),
[isar_community](https://pub.dev/packages/isar_community),
[isar](https://pub.dev/packages/isar),
[hive](https://pub.dev/packages/hive);
repositories
[simolus3/drift](https://github.com/simolus3/drift),
[tekartik/sqflite](https://github.com/tekartik/sqflite),
[objectbox/objectbox-dart](https://github.com/objectbox/objectbox-dart),
[IO-Design-Team/hive_ce](https://github.com/IO-Design-Team/hive_ce),
[isar-community/isar-community](https://github.com/isar-community/isar-community),
[isar/isar](https://github.com/isar/isar),
[isar/hive](https://github.com/isar/hive).

## Details per option

### Drift (recommended)

**Health.** Version 2.35.2 was published on the day of this research, and the companion generator `drift_dev` 2.35.1 on 2026-09-30 ([drift](https://pub.dev/packages/drift), [drift_dev](https://pub.dev/packages/drift_dev)). It is a Flutter Favorite with 160/160 pub points and about 1.47M downloads in 30 days ([pub.dev](https://pub.dev/packages/drift)). The repository had more than 100 commits in the last six months and 202 open against 2,259 closed issues ([repo](https://github.com/simolus3/drift)). The 15 most recent commits were all authored by one person, `simolus3` (one co-authored), so the bus factor is effectively one ([commit log](https://github.com/simolus3/drift/commits/develop)).

**Relations and aggregates.** The docs cover joins (`innerJoin`, `leftOuterJoin`), `groupBy`, and aggregate expressions such as `avg()` through `selectOnly`. Any query, including an aggregate, can be watched as a stream that re-emits when the underlying tables change ([select docs](https://drift.simonbinder.eu/dart_api/select/)). That suits an insights screen that updates after each retro.

**Migrations.** You bump `schemaVersion` and supply a `MigrationStrategy`. The recommended path is `dart run drift_dev make-migrations`, which saves each schema version, generates a step-by-step migration file, and generates tests that verify the migrations. The docs say of the manual route: "Writing migrations manually is error-prone and can lead to data loss." ([migrations docs](https://drift.simonbinder.eu/migrations/)). No other candidate generates migration tests.

**Codegen burden.** Setup is three runtime dependencies (`drift`, `drift_flutter`, `path_provider`) and two dev dependencies (`drift_dev`, `build_runner`), then `dart run build_runner build` or `watch` ([setup docs](https://drift.simonbinder.eu/setup/)). The generator can break against a new Flutter release: issue #3828, "drift_dev 2.34.0 fails to compile with Flutter 3.44.2 / Dart 3.12.2", was opened and closed in July 2026 ([issue list](https://github.com/simolus3/drift/issues?q=is%3Aissue+drift+3+in%3Atitle)). It was fixed, but a beginner should expect to hit this kind of thing occasionally and to upgrade Flutter and Drift together.

**Testability.** The testing guide says unit tests "can be run and debugged on your computer without additional setup, you don't need a physical device to run them", using `NativeDatabase.memory()`. For widget tests it gives a specific setting, `closeStreamsSynchronously: true` ([testing docs](https://drift.simonbinder.eu/testing/)).

**Export and backup.** The docs give a one-line export, `customStatement('VACUUM INTO ?', [file.path])`, a restore procedure, and point to a working backup and restore example in the repository ([existing databases docs](https://drift.simonbinder.eu/examples/existing_databases/)). The result is a standard SQLite file readable by any SQLite tool.

**Native SQLite bundling changed in 2026.** `sqlite3` 3.x now bundles SQLite through Dart build hooks, and `sqlite3_flutter_libs` is end-of-life: "Starting from version `0.6.0`, this package no longer does anything." ([sqlite3_flutter_libs](https://pub.dev/packages/sqlite3_flutter_libs), [upgrade notes](https://github.com/simolus3/sqlite3.dart/blob/main/UPGRADING_TO_V3.md)). `drift_flutter` 0.3.1 depends on `sqlite3 ^3.0.0` and handles this for you ([drift_flutter](https://pub.dev/packages/drift_flutter)). Practical effect: tutorials and answers written before 2026 that tell you to add `sqlite3_flutter_libs` are out of date. Follow the current setup page only.

**Drift 3.** The repository has a `future/` directory holding `drift3`, `drift3_flutter_preview`, `drift_manager` and `drift_sqlite` ([future/](https://github.com/simolus3/drift/tree/develop/future)). `drift3_preview 3.0.0-alpha.0` is on pub.dev, described as an "In-development preview of breaking changes planned for drift version 3" ([drift3_preview](https://pub.dev/packages/drift3_preview)). `drift_sqlite 0.1.0-alpha.2` was published on 2026-10-07 ([drift_sqlite](https://pub.dev/packages/drift_sqlite)). The only upgrade document is a short working list titled as a "Collection of API changes in demo apps to help come up with a migration tool"; the changes listed so far are an import path change, a web worker API change, and removing the trailing `()` from column definitions ([upgrading.md](https://github.com/simolus3/drift/blob/develop/future/upgrading.md)). That wording suggests a migration tool is intended, but none exists yet and the evidence here is thin.

### sqflite (fallback)

**Health.** Version 2.4.4+1 was published 2026-10-05; Flutter Favorite, 160/160 points, 3.32M downloads in 30 days, the most of any candidate ([pub.dev](https://pub.dev/packages/sqflite)). The repository has 5 open and 882 closed issues and 99 commits in six months ([repo](https://github.com/tekartik/sqflite)).

**Official standing.** Flutter's own SQLite cookbook uses it exclusively (page updated 2026-05-05), and the official architecture guide's SQL example uses it too, while noting the same service could be implemented with "`sqlite3`, `drift`" ([cookbook](https://docs.flutter.dev/cookbook/persistence/sqlite), [architecture guide](https://docs.flutter.dev/app-architecture/design-patterns/sql)). For a beginner this means the most first-party tutorial material.

**What you write yourself.** No codegen. Models use hand-written `toMap()` and `fromMap()`. Migrations are `onCreate`, `onUpgrade` and `onDowngrade` callbacks, wrapped in a transaction, with "Automatic version managment during open" ([README](https://pub.dev/packages/sqflite)). Queries are SQL strings, so a typo in a column name fails at runtime, not at compile time. Nothing checks that a migration produces the schema the code expects.

**Testability.** The testing doc says plainly that testing with `test` or `flutter_test` "is not supported" for the plugin itself and "requires running on a real supported platforms". The workaround is `sqflite_common_ffi`, which runs against the SQLite installed on the development machine and supports in-memory databases; widget tests need the `databaseFactoryFfiNoIsolate` variant ([testing doc](https://github.com/tekartik/sqflite/blob/master/sqflite/doc/testing.md)). This works but is more setup than Drift, and the host SQLite version may differ from the phone's.

**Export.** Same single SQLite file as Drift.

### ObjectBox

**Health.** 5.3.2 stable (2026-05-20), with a 6.0.0 preview series active as of 2026-10-06; 160/160 points, 174K downloads in 30 days ([pub.dev](https://pub.dev/packages/objectbox)). Commits daily, company-backed ([repo](https://github.com/objectbox/objectbox-dart)).

**Fit problems.**
- Aggregates exist (`min`, `max`, `avg`, `sum`, `count` on a property query) but the query docs do not mention grouping or aggregates across relations ([queries docs](https://docs.objectbox.io/queries)).
- Schema changes are automatic for adding and removing, need a `@Uid` annotation for renames, and "ObjectBox does not support migrating existing property data to a new type" ([data model updates](https://docs.objectbox.io/advanced/data-model-updates)).
- The Dart binding is Apache-2.0 but the core database is a proprietary binary. The vendor states it "will always be free to use" ([FAQ](https://objectbox.io/faq/)). That is a dependency on one company's goodwill for an app whose data lives only on the device.
- The file format is proprietary, so export means writing your own serialiser.
- Unit tests on the host need the native library downloaded by a shell script first ([README](https://pub.dev/packages/objectbox)).
- Codegen is still required (`objectbox_generator` plus `build_runner`), so it does not save the cost Drift has ([README](https://pub.dev/packages/objectbox)).

### Hive CE

**Health.** Very healthy: 2.20.2 published 2026-10-06, 160/160 points, 1.20M downloads in 30 days, 73 commits in six months ([pub.dev](https://pub.dev/packages/hive_ce), [repo](https://github.com/IO-Design-Team/hive_ce)). It describes itself as "A spiritual continuation of Hive v2" and adds isolate support and a `GenerateAdapters` annotation that reduces per-field annotation work ([README](https://pub.dev/packages/hive_ce)).

**Fit.** It is "a lightweight and blazing fast key-value database". The README does not describe queries, relations, aggregates or schema migration ([README](https://pub.dev/packages/hive_ce)). Every insight would be computed by loading boxes into memory and looping in Dart, and referential integrity between tasks, steps and sessions would be manual. That is feasible at this data size but puts the hardest logic in hand-written code with no schema to guard it. It is a reasonable choice for a small settings store beside the main database, and unnecessary if Drift is already present.

### Isar

**Original package: abandoned in practice.** Last stable release 3.1.0+1 on 2023-04-25; last v4 dev release 2023-08-21; 3.5K downloads in 30 days ([pub.dev](https://pub.dev/packages/isar)). Last commit 2025-06-14, none in the last six months, 173 open issues ([repo](https://github.com/isar/isar)). The README still warns "ISAR V4 IS NOT READY FOR PRODUCTION USE" ([repo](https://github.com/isar/isar)). Open issues include "Isar is dead, long live Isar" (#1689, 52 comments), "This project is dead! Don't use it!!!" (#1739), and Android 16KB page size problems (#1751, #1741) ([issues](https://github.com/isar/isar/issues?q=is%3Aissue+abandoned+OR+maintained+OR+dead)).

**Community fork.** `isar_community` 3.3.2 (2026-03-23), 105K downloads in 30 days, 140/160 points ([pub.dev](https://pub.dev/packages/isar_community)). Its stated scope is narrow: "a fork of the original project, focusing primarily on bug fixes and small updates for version 3" ([repo](https://github.com/isar-community/isar-community)). Only 4 commits since 2026-04-07 ([repo](https://github.com/isar-community/isar-community)). A second fork, `isar_plus` 1.3.9 (2026-08-04), is more active but has 3.2K downloads in 30 days and 72 stars, too small to rely on ([pub.dev](https://pub.dev/packages/isar_plus), [repo](https://github.com/ahmtydn/isar_plus)).

**Fit.** Aggregations are `.min()`, `.max()`, `.sum()`, `.average()` and `.count()` on a query; no grouping is documented, and link queries carry a documented cost warning ([queries docs](https://isar-community.dev/v3/queries.html)). Renaming a field without `@Name` makes the database "delete and re-create the field or collection" ([schema docs](https://isar-community.dev/v3/schema.html)), a silent data-loss trap for a beginner.

### Ruled out quickly

- **Hive (original).** Last stable release 2022-06-30, 558 open issues, and the documentation site `docs.hivedb.dev` now serves a domain-for-sale page ([pub.dev](https://pub.dev/packages/hive), [repo](https://github.com/isar/hive), [docs site](https://docs.hivedb.dev/)). It still shows 1.07M downloads in 30 days, which reflects legacy use, not health. Use Hive CE if a key-value store is wanted.
- **Realm.** The package page carries the notice "We announced the deprecation of Atlas Device Sync + Realm SDKs in September 2024." Last release 20.2.0 on 2025-09-24, no commits in the last six months ([pub.dev](https://pub.dev/packages/realm), [repo](https://github.com/realm/realm-dart)).
- **Floor.** A SQLite ORM with codegen, last released 2024-05-08, last commit 2024-05-29, 110/160 points ([pub.dev](https://pub.dev/packages/floor), [repo](https://github.com/pinchbv/floor)). Stale; Drift covers the same ground.
- **Sembast.** Maintained (3.8.11, 2026-09-20) by the sqflite author, but a NoSQL document store with no SQL aggregates ([pub.dev](https://pub.dev/packages/sembast)). No advantage here over Hive CE or SQLite.
- **`sqlite3` directly.** Maintained (3.7.0, 2026-09-30) and is what Drift sits on ([pub.dev](https://pub.dev/packages/sqlite3)). Synchronous low-level API with no migration helpers; more work than sqflite for no gain in this app.
- **PowerSync / `sqlite_async`.** Maintained, but built around sync to a backend, which the map puts out of scope ([powersync](https://pub.dev/packages/powersync), [sqlite_async](https://pub.dev/packages/sqlite_async)).

## Notes for the decision

- **Beginner cost of Drift is front-loaded.** The first hour (setup, first table, running the generator) is harder than sqflite. After that the compiler catches mistakes that sqflite would surface at runtime, and `make-migrations` removes the riskiest hand-written part. For someone on a second app, that trade favours Drift, provided they follow the current official setup page and not older tutorials.
- **Backup ticket.** Either SQLite option keeps the "not yet specified" backup and export question open in the easiest form: one file to copy, plus the option of CSV or JSON export written as ordinary queries.
- **Lock-in is low for both SQLite options.** Moving between sqflite and Drift, or from Drift 2 to Drift 3, changes Dart code but not the database file. Moving off ObjectBox, Isar or Hive means a data conversion.

## Unknowns / could not verify

- **Nothing was built or run.** No test project was created. Claims about setup effort, test ergonomics and iOS behaviour come from documentation, not from hands-on use.
- **Drift 3 timing and impact.** No release date, no statement on how long Drift 2 will receive fixes, and no migration tool were found. `future/README.md` in the repository is empty. The size of the eventual breaking change is unknown; the three changes listed in `upgrading.md` are a working list, not a complete one.
- **Build hooks on iOS.** I did not confirm the minimum Flutter version that `sqlite3` 3.x build hooks need, nor test an iOS device build. The Drift setup page did not state a minimum Flutter version. `drift_flutter` still lists the end-of-life `sqlite3_flutter_libs` as a dependency; I assume this is harmless since that package "no longer does anything", but did not verify.
- **Drift's bus factor.** "Effectively one maintainer" is inferred from the 15 most recent commits only. I did not check for funding, co-maintainers or a succession plan.
- **Grouping in Isar and ObjectBox.** "Not documented" is what the docs pages show. I did not search issue trackers or API references exhaustively for an undocumented grouping feature.
- **ObjectBox licence terms.** Taken from the vendor FAQ. I did not read the binary licence text itself, and did not find out what the 6.0 preview changes.
- **Isar data migration recipe and Hive CE migration guidance.** The Isar schema page links a data-migration recipe I did not read. The Hive CE README is silent on migrations; I did not check its separate docs.
- **Hive (original) repository activity.** The last commit date and archive status could not be retrieved (the repository moved to `isar/hive` and the API calls were rate-limited). The 2022 release date and 558 open issues are verified.
- **Issue counts marked * in the table** are GitHub's combined open issues and pull requests, because the issue-only search was rate-limited for those repositories.
- **Hive CE and Isar host-test setup** was not checked.
- **Download counts** are pub.dev's 30-day figures on 2026-10-07 and include CI traffic; treat them as relative popularity only.
