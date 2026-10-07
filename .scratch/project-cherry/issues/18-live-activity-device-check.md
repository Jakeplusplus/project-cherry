# Live Activity device check

Type: prototype
Status: open
Blocked by: 02

## Question

Does an iOS Live Activity timer behave as the design needs on a real iPhone? Build a throwaway Flutter app with the `live_activities` plugin and a minimal SwiftUI widget extension, plus a scheduled local notification, and verify the unknowns from the background-timer research: count-up timer text stays correct over hours; what the activity looks like past the overrun time (`staleDate`); survival across app kill and reboot; whether it works without the Push Notifications capability; whether the widget extension can be signed with the author's Apple developer account. Also a difficulty check: how painful was the native Xcode work for a Flutter beginner — is the Live Activity in v1, or does v1 ship with the scheduled notification only?

Precondition (fact needed from the author): free or paid Apple developer account?
