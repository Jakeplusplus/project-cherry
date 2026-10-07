# Local persistence options

Type: research
Status: resolved

## Question

Which on-device store fits this app: Drift (SQLite), sqflite, Isar, Hive/Hive CE, ObjectBox, or other? Needs: relational data (task -> steps -> timer sessions -> retros), aggregate queries for insights (average actual minutes per category and bucket, estimate-accuracy over time), schema migrations, no account/cloud, a future door for export/backup, and beginner-friendly docs. Report maintenance status and health of each package as of late 2026, codegen burden, and testability.

## Answer

Full findings: [`research/local-persistence-options.md`](https://github.com/Jakeplusplus/project-cherry/blob/research/local-persistence-options/research/local-persistence-options.md) on branch `research/local-persistence-options`.

**Use Drift (SQLite); sqflite is the fallback.**

- **Drift 2.35.x** (Flutter Favorite, actively maintained) is the only candidate covering every need: foreign keys and joins, `GROUP BY`/`AVG` in the database, watchable aggregate queries, a single SQLite file exportable with `VACUUM INTO`, and in-memory unit tests with no device.
- **Migrations** are its strongest point: `dart run drift_dev make-migrations` generates step-by-step migrations and tests for them.
- **Cost:** codegen (`drift_dev` + `build_runner`); the generator broke once against a new Flutter release in July 2026 (fixed).
- **sqflite**: no codegen, used in the official Flutter cookbook, but SQL strings, row mapping and migrations are hand-written.
- **ObjectBox**: healthy, but no documented grouping, proprietary native core, cannot migrate a property's type.
- **Hive CE**: healthy key-value store; no queries, relations or aggregates — settings only.
- **Ruled out:** Isar (abandoned in practice; community fork bug-fix-only), original Hive, Realm (deprecated), Floor (stale).
- **Caveats:** Drift 3 is in alpha (preview published September 2026) with no release date, no stated Drift 2 support window, and no migration tool yet — expect a major-version code upgrade; the SQLite file itself is unaffected. Nothing was built or run. iOS build behaviour for `sqlite3` 3.x untested; older tutorials telling you to add `sqlite3_flutter_libs` are out of date. Drift is effectively single-maintainer (inferred from recent commits).
