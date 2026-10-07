# Background timer and lock-screen presence from Flutter

Research for ticket `02-background-timer-lock-screen`. Checked 2026-10-07 against Apple and Android developer docs and pub.dev. Every plugin version below is what pub.dev showed on that date.

## Answer

**Do not run a timer in the background at all.** Store the start instant when the user taps Start and compute `elapsed = now - start` whenever the app needs it. Nothing has to execute while the app is backgrounded, killed, or the phone is rebooted, so the timer is exact in all those cases by construction. Everything else is presentation layered on top of that one stored timestamp.

Recommended v1 shape:

| Need | iOS | Android | Native code? |
|---|---|---|---|
| Accurate elapsed time | Persisted start timestamp | Same | None |
| Overrun check-in | Local notification scheduled at start, cancelled at stop (`flutter_local_notifications`) | Same plugin | None beyond config-file edits |
| Lock-screen presence | Live Activity with an OS-rendered ticking timer (`live_activities` plugin) | Ongoing notification with a system chronometer (`flutter_local_notifications`) | **iOS: yes, Swift.** Android: none |

Key points for the decision:

1. **No push server is needed.** Apple documents starting, updating and ending a Live Activity from the app with ActivityKit as a first-class path, with push as the alternative. The user taps Start inside the app, which satisfies the "must be in the foreground to start" rule.
2. **The Live Activity is the only part that forces native work.** Its UI must be SwiftUI inside an Xcode widget extension; Flutter cannot render it. Expect one small Swift file plus Xcode target, capability and Info.plist setup. Two "write it in Dart" plugins exist but are too new and too little used to bet on (6 likes / 180 downloads, and 1 like / 73 downloads).
3. **Once started, the Live Activity needs no updates to keep ticking.** SwiftUI timer text is drawn by the system from a date, so the lock screen counts up while the app is suspended or killed.
4. **The 8-hour cap is real and collides with the "whole day" bucket.** The system ends a Live Activity after 8 hours and removes it from the lock screen at most 4 hours later. The stored timestamp is unaffected, but lock-screen presence disappears exactly when a forgotten timer most needs it. The overrun notification has to carry that case.
5. **A suspended app cannot change its Live Activity at a chosen future moment** without push. The overrun check-in must therefore be a scheduled local notification, not a Live Activity update. (A stale date may give a limited visual change; see below, unverified.)
6. **Android needs no foreground service and no Kotlin** for this design, because nothing has to run. An ongoing notification with `usesChronometer` ticks on its own.

**Suggested sequencing for a Flutter beginner:** ship timestamps + the overrun notification + the Android ongoing notification first (zero native code, one well-documented plugin). Add the iOS Live Activity as a separate, isolated step. It is additive and does not change the data model.

**Biggest caveat:** on iOS, the Swift widget extension is unavoidable for real lock-screen presence, and several of its behaviours that matter here (count-up rendering past an hour, stale-date restyling, survival across reboot, whether a free Apple developer account can build it) are thinly documented and need a throwaway prototype on a real iPhone before the spec commits to them.

## 1. Timestamp-based elapsed time vs live ticking

- Flutter exposes lifecycle states `resumed`, `inactive`, `hidden`, `paused`, `detached`; `paused` is described as the app being suspended in the background. `AppLifecycleListener` provides `onResume`, `onPause` and related callbacks. https://api.flutter.dev/flutter/widgets/AppLifecycleListener-class.html
- A Dart `Timer` or `Stopwatch` lives in the app process, so it cannot be the source of truth across suspension or kill. This is an inference from the lifecycle model, not a quoted statement; the Flutter page above does not explicitly say Dart stops executing while paused.
- Design consequence: persist `startedAt` (UTC) to the on-device database at the moment of Start. An in-app ticking display is just a 1-second UI refresh that recomputes `now - startedAt` while visible, and recomputes once in `onResume`.
- iOS confirms the same pattern is expected of apps: "the system may stop your app, or your app may crash while a Live Activity is active. When the app launches the next time, check if any activities are still active, update your app's stored Live Activity data, and end any Live Activity that's no longer relevant." https://developer.apple.com/documentation/activitykit/displaying-live-data-with-live-activities
- Risk: wall-clock timestamps are wrong if the user changes the device clock or time zone handling is sloppy. Store UTC. A monotonic clock would resist clock changes but does not survive reboot, so it cannot replace the wall-clock timestamp. The map's "forgiving repair path" covers the residual cases. (Reasoning, not sourced.)

## 2. iOS: Live Activities and Dynamic Island

### What Apple's docs say (primary)

Source for this subsection unless noted: https://developer.apple.com/documentation/activitykit/displaying-live-data-with-live-activities

- **UI is SwiftUI in a widget extension.** "To offer Live Activities, add code to your existing widget extension or create a new widget extension... Live Activities use WidgetKit functionality and SwiftUI for their user interface."
- **All presentations are mandatory.** "To add support for Live Activities to your iOS or iPadOS app, you must support all presentations": Lock Screen, plus Dynamic Island compact, minimal and expanded.
- **No push required.** "you start and update a Live Activity from your app with ActivityKit or with ActivityKit push notifications." Push is the alternative that needs a server: "Set up a remote notification server..." https://developer.apple.com/documentation/activitykit/starting-and-updating-live-activities-with-activitykit-push-notifications
- **Start needs the foreground.** "In general, your app needs to be in the foreground to start a Live Activity. You can update or end a Live Activity from your app while it runs in the background — for example, by using BackgroundTasks." The exception is an App Intent conforming to `LiveActivityIntent` (more Swift). Same statement on the `Activity` class page: https://developer.apple.com/documentation/activitykit/activity
- **8-hour cap.** "A Live Activity can be active for up to eight hours unless its app or a person ends it before this limit. After the eight-hour limit, the system automatically ends the Live Activity, and immediately removes it from the Dynamic Island. However, the Live Activity remains on the Lock Screen until a person removes it or for up to four additional hours... a Live Activity remains on the Lock Screen for a maximum of 12 hours."
- **Size limit.** Static plus dynamic data "can't exceed a combined size of 4 KB." A start timestamp and a task title fit easily.
- **Sandbox.** A Live Activity "can't access the network or receive location updates."
- **Stale date.** `staleDate` "tells the system when the Live Activity content becomes outdated. At the specified date, the activityState changes to stale and isStale changes to true", and the doc describes a Live Activity that "becomes stale and displays text to indicate that the displayed information is outdated."
- **User can remove it.** "A person can remove your Live Activity from their Lock Screen at any time. This ends the Live Activity, but it doesn't end or cancel the person's action that started it." So a dismissed Live Activity must not stop the timer. Users can also disable Live Activities per app in Settings; check `areActivitiesEnabled`.
- **Buttons.** Buttons and toggles in a Live Activity require the App Intents framework (Swift). A "Stop" button on the lock screen is therefore additional native work; tapping the activity to deep-link into the app is the cheap option.
- **Scheduled start.** A `request(...startDate:)` variant can schedule a Live Activity for a future date and requires an `AlertConfiguration`. Not needed here, since Cherry starts on a user tap. I did not confirm which iOS version introduced it.
- **Update limits.** The only documented budget is for push: "The system allows for a certain budget of ActivityKit push notifications per hour." (push article above). I found no documented numeric limit for local `update` calls. Cherry's design needs roughly two calls per timed item (start, end), so this is unlikely to matter.

### The ticking timer without updates

- SwiftUI offers `Text(timerInterval:pauseTime:countsDown:showsHours:)`, "an instance that displays a timer counting within the provided interval", with `countsDown` defaulting to true, available iOS 16.0+. Apple notes that in widgets this text "becomes horizontally flexible and expands to fill available width", so it needs an explicit frame. https://developer.apple.com/documentation/swiftui/text/init(timerinterval:pausetime:countsdown:showshours:)
- `Text.DateStyle.timer` is "a style displaying a date as timer counting from now", example output `2:32` and `36:59:01`. https://developer.apple.com/documentation/swiftui/text/datestyle/timer
- Apple names timer text specifically in the Live Activity animation guidance ("request animations for timer text with numericText(countsDown:)"), which confirms timer text is an intended Live Activity element. (Live Activities article above.)
- **Evidence is thin on one point:** that a `.timer`-style text given a *past* start date counts *up* indefinitely as an elapsed-time stopwatch. This is common practice but Apple's reference page is one sentence long and does not spell it out. Verify in the prototype, including behaviour past 1 hour and near the 8-hour mark.

### What a suspended app cannot do

Apple permits background updates only "while it runs in the background". A suspended or killed app is not running, and BackgroundTasks run at the system's discretion, not at a chosen instant. With no push server, Cherry cannot flip the Live Activity to an "over your estimate" state at the exact overrun moment. Options, in order of confidence:

1. Scheduled local notification at the overrun time (section 4). Documented, reliable.
2. Set `staleDate` to the overrun time and have the SwiftUI view render differently when `context.isStale` is true. Apple's text supports this reading, but I found no Apple example using it as a deliberate scheduled state change. **Unverified; prototype it.**
3. Use a bounded `Text(timerInterval:)` so the count visibly stops at the estimate. Documented API, crude UX.

### Flutter plugins

**`live_activities`** — https://pub.dev/packages/live_activities (repo https://github.com/istornz/flutter_live_activities)

- Version 2.6.0, published about 26 days before 2026-10-07, verified publisher `dimitridessus.fr`, 656 likes, 150 pub points, about 71k downloads. Requires Flutter 3.44+ since 2.5.0. Changelog: https://pub.dev/packages/live_activities/changelog
- README, verbatim: "You need to implement in your Flutter iOS project a Widget Extension & develop in Swift/Objective-C your own Live Activity / Dynamic Island design." FAQ: "Do I have to code in Swift? Yes".
- Required Xcode steps per README: add a Widget Extension target; add `NSSupportsLiveActivities` to Info.plist for Runner and the extension; add the App Groups capability to both targets; define an `ActivityAttributes` struct named exactly `LiveActivitiesAppAttributes` ("if you rename, activity will be created but not appear!"); read Flutter-supplied values from `UserDefaults(suiteName: "YOUR_GROUP_ID")`. A URL scheme is needed to handle taps back into the app.
- Push: the README also says to add the Push Notifications capability to Runner. The API doc for `createActivity` describes `iOSEnableRemoteUpdates` (default true) as the switch that "requires Push Notifications capability", and the README says setting it to false "will prevent the activity from being updated via push notifications." Reading these together, local-only use should not need the capability, but the README's troubleshooting list still tells users to verify it is enabled. **Not confirmed either way.** https://pub.dev/documentation/live_activities/latest/live_activities/LiveActivities/createActivity.html
- Useful parameters on `createActivity`: `staleIn` (minimum 1 minute, iOS 16.2+) and `removeWhenAppIsKilled` (default false, which is what Cherry wants: the activity should outlive the app process).
- README on background: "if your app was killed or in the background, you can't update the notification... To do this, you can update it using Push Notification on a server." Consistent with the Apple constraint above; irrelevant if the timer text self-ticks.
- Android: supported since 2.4.0 via RemoteViews, described by the README as "less mature than iOS... in beta", and it requires a Kotlin `CustomLiveActivityManager` plus an XML layout. Not recommended for Cherry's Android side; see section 3.

**Dart-only alternatives (not recommended yet)**

- `live_activity_kit` 1.1.1: Dart DSL serialized to JSON and rendered by a generated extension; a CLI generates the widget extension; has `LA.countdown` and `LA.stopwatch` components. Unverified uploader, 6 likes, 180 downloads. Still needs manual App Group capability, signing team, and one Xcode build. https://pub.dev/packages/live_activity_kit
- `flutter_activity_kit` 0.7.0: Dart DSL transpiled to SwiftUI by a generator. Verified publisher `pinz.dev`, but 1 like, 73 downloads, pre-1.0. Still needs a widget extension target created in Xcode. https://pub.dev/packages/flutter_activity_kit
- Both remove the SwiftUI authoring, not the Xcode target setup. With adoption this low, a beginner would be debugging generated native code with no community to ask. Worth a second look later, not a foundation for v1.

### Native work summary for iOS lock-screen presence

Forced, regardless of plugin: a widget extension target in Xcode, App Group capability, Info.plist key, code signing for a second target. With `live_activities`: additionally about one SwiftUI file covering lock-screen, compact, minimal and expanded layouts. An optional lock-screen Stop button adds App Intents code in Swift.

## 3. Android: ongoing notification, foreground service, Live Updates

- **No foreground service is needed for this design.** Foreground services exist to keep code running; with timestamp-based timing nothing needs to run. Avoiding one also avoids the Android 14+ type system: every foreground service must declare a type, `shortService` times out at about 3 minutes, `dataSync` is capped at 6 hours per 24, and `specialUse` requires a Play Console declaration and review. None of the listed types is a natural fit for a stopwatch. https://developer.android.com/develop/background-work/services/fgs/service-types
- **Ongoing notification with a system chronometer.** `flutter_local_notifications` exposes `ongoing`, `usesChronometer` ("shows the timestamp as a stopwatch"), `chronometerCountDown`, `when`, `showWhen`, `onlyAlertOnce`, `visibility` (lock screen) and `actions` on `AndroidNotificationDetails`. Setting `when` to the stored start time gives a count-up drawn by the system, with no Kotlin. https://pub.dev/documentation/flutter_local_notifications/latest/flutter_local_notifications/AndroidNotificationDetails-class.html
- **Ongoing is no longer undismissable.** Since Android 14, users can swipe away `FLAG_ONGOING_EVENT` notifications, except "when the phone is locked" and against "Clear all". Treat dismissal as "hide", never as "stop timer". https://developer.android.com/about/versions/14/behavior-changes-all
- **Notification permission.** On Android 13+ notifications are off by default for new installs and need the `POST_NOTIFICATIONS` runtime permission. Ask at first timer start, in context. https://developer.android.com/develop/ui/views/notifications/notification-permission
- **`flutter_foreground_task`** (11.0.3, verified publisher, 583 likes) exists if a foreground service is ever wanted, but on iOS it only offers roughly 30 seconds every 15 minutes and stops on force-close, so it solves nothing for the iOS-first target. https://pub.dev/packages/flutter_foreground_task
- **Android 16 Live Updates** (promoted ongoing notifications) are the closest analogue to Live Activities: lock screen, top of the shade, and a status-bar chip. Requirements include the `POST_PROMOTED_NOTIFICATIONS` permission, `setOngoing(true)`, `setRequestPromotedOngoing(true)`, a standard style, and no custom RemoteViews; the doc shows chronometer usage with `setUsesChronometer`. https://developer.android.com/develop/ui/views/notifications/live-update and https://developer.android.com/about/versions/16/features
  - I saw nothing about promoted/Live Update support in the `flutter_local_notifications` changelog through 22.3.1. Treat as a later enhancement, likely needing Kotlin or a newer plugin. https://pub.dev/packages/flutter_local_notifications/changelog
  - Apple-style guidance applies: Android lists "upcoming events" and general alerts as inappropriate uses; a user-started running timer is the appropriate kind.

## 4. Scheduling the overrun check-in notification

**Plugin: `flutter_local_notifications`** — https://pub.dev/packages/flutter_local_notifications

- Version 22.3.1, published about 24 days before 2026-10-07, verified publisher `dexterx.dev`. Minimums since 21.0.0: Android API 24, iOS 13, Flutter 3.38.1. Since 20.0.0, `initialize`, `show`, `zonedSchedule` and `cancel` take named parameters, so tutorials older than that will not compile as written. https://pub.dev/packages/flutter_local_notifications/changelog
- Schedule with `zonedSchedule()` and a `TZDateTime` from the `timezone` package at Start; cancel by id at Stop. Schedule one or a few check-ins (for example at the top of the estimate bucket and again later) rather than a repeating series.
- No background Dart execution is involved: the OS holds and delivers the notification.

**iOS**

- "The system handles delivery of notifications based on a time or location that you specify. If the delivery of the notification occurs when your app isn't running or in the background, the system interacts with the user for you." A request "remains active until its trigger condition is met, or you explicitly cancel it." https://developer.apple.com/documentation/usernotifications/scheduling-a-notification-locally-from-your-app
- Requires user authorization for alerts. If the user declines, there is no overrun check-in on iOS; the Live Activity becomes the only signal.
- The plugin README notes iOS keeps only 64 pending notifications per app. Not a constraint for one running timer.
- `DarwinNotificationDetails` has an `interruptionLevel` property ("priority and delivery timing of a notification") and `categoryIdentifier` for action buttons. Time Sensitive would let the check-in break through Focus; I did not verify what entitlement or capability that needs. https://pub.dev/documentation/flutter_local_notifications/latest/flutter_local_notifications/DarwinNotificationDetails-class.html
- Native edit required: one line in `AppDelegate` assigning the `UNUserNotificationCenter` delegate, plus plugin-registrant setup if notification action buttons should run Dart in the background. Copy-paste from the README, but it is a Swift file.

**Android**

- Inexact alarms can be deferred, up to about an hour on Android 12+; in Doze, use the allow-while-idle variants. Exact alarms need `SCHEDULE_EXACT_ALARM` (denied by default on Android 14 for new installs, user must grant in Settings) or `USE_EXACT_ALARM` (auto-granted, policy-restricted). https://developer.android.com/develop/background-work/services/alarms/schedule and https://developer.android.com/about/versions/14/behavior-changes-all
- Google Play restricts `USE_EXACT_ALARM` to apps whose core function needs it, naming "an alarm or timer app" and calendar apps. Whether a task manager with a timer qualifies is a judgement call by Play review. https://support.google.com/googleplay/android-developer/answer/13161072
- Recommendation: an overrun check-in does not need to-the-second delivery. Start with the plugin's inexact allow-while-idle schedule mode and no exact-alarm permission; revisit only if delays are noticeable in real use.
- Config edits required (no Kotlin): manifest permissions and two receivers, plus Gradle core-library desugaring and `compileSdk` 35+ (36 per the 21.0.0 changelog). https://github.com/MaikuB/flutter_local_notifications/blob/master/flutter_local_notifications/README.md
- OEM caveat from the README: "Some Android OEMs have their own customised Android OS that can prevent applications from running in the background. Consequently, scheduled notifications may not work when the application is in the background on certain devices (e.g. by Xiaomi, Huawei)."

## 5. What survives app kill and reboot

| Thing | App suspended / killed by OS | User force-quits (iOS swipe / Android force stop) | Reboot |
|---|---|---|---|
| Start timestamp in on-device DB | Survives | Survives | Survives |
| iOS scheduled local notification | Delivered by the system (Apple doc, section 4) | Expected to be delivered; not explicitly documented | **Not explicitly documented.** Apple says a request stays active until triggered or cancelled |
| iOS Live Activity | Stays; Apple tells apps to reconcile on next launch | Stays, per developer reports that it lingers after force-quit unless explicitly ended (forum, not Apple staff) | **Unknown** |
| iOS Live Activity after 8 h | Ended by system; off lock screen within 4 more hours | Same | n/a |
| Android scheduled notification | Delivered, subject to Doze and OEM battery managers | **Cancelled** on force stop until the user reopens the app | **Cancelled** by the OS; the plugin reschedules on boot if `RECEIVE_BOOT_COMPLETED` and its boot receiver are declared |
| Android ongoing chronometer notification | Expected to stay (system draws it) | Expected to be removed | Removed; app must re-post on next launch |

Sources: Apple Live Activities article and local-notification article (above); Apple forum thread https://developer.apple.com/forums/thread/732418; Android alarms doc (above) for reboot and force-stop behaviour; plugin README for boot rescheduling.

Design rule that follows: **on every launch and every resume, reconcile.** Read the running timer from the DB, then re-create or end the Live Activity / ongoing notification and re-schedule or cancel the check-in to match. That one routine covers kill, force-quit, reboot, the 8-hour expiry, and user dismissal.

## 6. Everything that forces native Swift/Kotlin work

| Item | Kind of work | Avoidable? |
|---|---|---|
| iOS Live Activity UI (lock screen + 3 Dynamic Island presentations) | SwiftUI code | Only by skipping Live Activities, or betting on an immature Dart-DSL plugin |
| Widget extension target, App Group, Info.plist key, signing | Xcode configuration | No, for any Live Activity route |
| Stop/Done button on the Live Activity | Swift App Intent | Yes: deep-link into the app on tap instead |
| Starting a Live Activity while backgrounded | Swift `LiveActivityIntent` | Yes: Cherry starts timers in the foreground |
| iOS notification delegate line in `AppDelegate` | One-line Swift edit | No, if using `flutter_local_notifications` |
| Android manifest receivers/permissions, Gradle desugaring | XML/Gradle config | No, but it is copy-paste |
| Android foreground service | Manifest + Play declaration | Yes: not needed |
| Android Live Activity-style UI via `live_activities` | Kotlin + XML layout | Yes: use the chronometer notification |
| Android 16 Live Updates chip | Probably Kotlin today | Yes: defer |

## Unknowns / could not verify

1. **Count-up rendering.** Whether `Text(date, style: .timer)` with a past date renders a correct count-up for the full 8 hours on the lock screen and in the Dynamic Island. Apple's reference is one sentence. Needs a device test.
2. **Stale date as an overrun signal.** Whether the lock-screen view reliably re-renders at `staleDate` with `isStale == true` while the app is suspended. Supported by Apple's wording, no worked example found.
3. **Live Activity across reboot.** No primary source found for whether an active Live Activity reappears after a restart.
4. **iOS pending local notifications across reboot and force-quit.** Apple's wording implies persistence; no explicit statement found. A web summary of the `flutter_local_notifications` page claimed scheduled notifications do not fire when the app is killed on iOS; that contradicts Apple's documentation and the README does not say it, so I am treating it as a summarisation error, but it is worth a two-minute device test.
5. **Push Notifications capability for local-only Live Activities via `live_activities`.** README says add it; the API doc ties it to `iOSEnableRemoteUpdates`. Unclear whether a build without it works.
6. **Apple developer account tier.** I did not check whether a free personal team can sign a widget extension with App Groups (and Push, if unknown 5 goes the wrong way), or whether the paid programme is required to test on the author's own iPhone.
7. **Local Live Activity update limits.** No documented number for app-initiated `update` calls; only the push budget is documented.
8. **Scheduled-start API version.** The `startDate` request variant is documented but I did not confirm its minimum iOS version. Not needed for v1.
9. **Time Sensitive notifications.** Not verified what capability/entitlement the `timeSensitive` interruption level needs or how the plugin exposes it.
10. **Android schedule mode names.** I did not verify the exact current `AndroidScheduleMode` enum values in 22.x; check the API docs when implementing.
11. **`USE_EXACT_ALARM` eligibility.** Play policy does not define "timer app"; whether Cherry qualifies is undecided and only matters if inexact delivery proves too loose.
12. **Android notification persistence details.** That an ongoing notification stays after the OS kills the process, and is removed on force stop, is expected platform behaviour that I did not trace to a primary-source sentence.
13. **Android Live Updates minimum version and plugin support.** The Live Update doc page does not state a minimum version; Android 16 is where the related progress-centric APIs are introduced, and a secondary source places the permission in Android 16 QPR1. No evidence of support in `flutter_local_notifications`.
14. **Plugin health over time.** Versions, likes and download counts are a snapshot from 2026-10-07. I did not review open issues for either recommended plugin against the current iOS and Android releases.
