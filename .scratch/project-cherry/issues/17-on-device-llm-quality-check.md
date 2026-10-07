# On-device LLM breakdown quality check

Type: prototype
Status: open

## Question

Are steps proposed by Apple's on-device Foundation Models good enough to offer as a "suggest steps" button? Build a throwaway prototype (via `flutter_local_ai`, or a bare Swift playground if faster) that takes a task title and returns 5–8 steps, and run it against 15–20 of the author's real todos on the author's iPhone. Judge: are the steps concrete, startable, and correctly ordered; how often are they wrong or useless; how long does it take. Also confirm the plugin makes no network calls.

Precondition (fact needed from the author): is the daily iPhone a 15 Pro or newer with Apple Intelligence enabled? If not, this cannot be run on-device and the AI-breakdown decision must be made without it.
