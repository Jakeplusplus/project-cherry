# Background timer and lock-screen presence from Flutter

Type: research
Status: resolved

## Question

How does a Flutter app keep an accurate running task timer when backgrounded or killed, iOS first, Android second? Cover: timestamp-based elapsed time vs live ticking; iOS Live Activities / Dynamic Island from Flutter (plugins, native extension work required, update limits, 8-hour cap); Android foreground service / ongoing notification equivalents; scheduling a local 'overrun check-in' notification at a known future time; what survives app kill and reboot. Flag anything that forces native Swift/Kotlin work, since the developer is a Flutter beginner.

## Answer

Full findings: [`research/background-timer-lock-screen.md`](https://github.com/Jakeplusplus/project-cherry/blob/research/background-timer-lock-screen/research/background-timer-lock-screen.md) on branch `research/background-timer-lock-screen`.

- **Don't run a background timer.** Persist the start timestamp (UTC) and compute `elapsed = now - start`. Exact across suspend, kill and reboot with no background execution.
- **No push server needed for Live Activities.** ActivityKit can start, update and end from the app; start requires the foreground, which tapping Start satisfies.
- **iOS lock-screen presence is the only part forcing native work.** UI must be SwiftUI in an Xcode widget extension (App Group, Info.plist key, second signing target). Recommended plugin: `live_activities` 2.6.x.
- **The lock-screen timer ticks without updates** (OS draws timer text from a date). But a suspended app cannot change the Live Activity at a chosen moment, so the **overrun check-in must be a scheduled local notification**.
- **Overrun check-in:** `flutter_local_notifications` `zonedSchedule` at Start, cancel at Stop. Delivered with the app not running.
- **Android needs no foreground service and no Kotlin:** ongoing notification with `usesChronometer`. Since Android 14 the user can swipe it away; alarms are cancelled on reboot (plugin reschedules via boot receiver) and on force stop.
- **8-hour cap is real:** the system ends a Live Activity at 8 h and removes it by 12 h. Collides with the "whole day" bucket; the notification must cover long forgotten timers.
- **Suggested sequencing:** timestamps + notifications + Android chronometer first (zero native code), then the iOS Live Activity as an isolated step. Reconcile timer state on every launch and resume.
- **Biggest caveat:** several iOS behaviours are thinly documented and not verified on a device — count-up timer text rendering for the full 8 h, `staleDate` restyling at overrun, survival across reboot, whether `live_activities` works without the Push Notifications capability, and whether a free Apple developer account can sign the widget extension. 14 unknowns listed in the file; a device prototype should precede committing the spec to the Live Activity (see ticket 18).
