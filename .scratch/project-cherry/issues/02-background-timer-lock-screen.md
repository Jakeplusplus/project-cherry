# Background timer and lock-screen presence from Flutter

Type: research
Status: open

## Question

How does a Flutter app keep an accurate running task timer when backgrounded or killed, iOS first, Android second? Cover: timestamp-based elapsed time vs live ticking; iOS Live Activities / Dynamic Island from Flutter (plugins, native extension work required, update limits, 8-hour cap); Android foreground service / ongoing notification equivalents; scheduling a local 'overrun check-in' notification at a known future time; what survives app kill and reboot. Flag anything that forces native Swift/Kotlin work, since the developer is a Flutter beginner.
