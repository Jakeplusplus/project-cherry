# On-device LLM feasibility for task breakdown

Type: research
Status: resolved

## Question

Can an on-device LLM propose useful sub-steps for a task, with no task text leaving the phone? Survey what is reachable from Flutter on iOS (Apple Foundation Models / Apple Intelligence APIs) and Android (Gemini Nano / AICore), plus bundled-model options (e.g. llama.cpp / MediaPipe LLM Inference bindings). For each: device/OS coverage, Flutter plugin maturity, app-size and memory cost, licensing, and expected quality for short structured output (5-8 concrete steps). State what fraction of current phones could use it and what the fallback story is.

## Answer

Full findings: [`research/on-device-llm-feasibility.md`](https://github.com/Jakeplusplus/project-cherry/blob/research/on-device-llm-feasibility/research/on-device-llm-feasibility.md) on branch `research/on-device-llm-feasibility`.

**Feasible, but only as an optional "suggest steps" button on a minority of phones. The guided manual breakdown must remain the real feature.**

- **Recommended path: OS-provided models only, no bundled model.** Apple Foundation Models on iOS and Gemini Nano (ML Kit GenAI Prompt API / AICore) on Android, via the community plugin `flutter_local_ai` (verified publisher, MIT, 0.2.x). Zero added app size, no account, on-device inference.
- **iOS is the strong case.** Foundation Models has shipped since iOS 26.0 with constrained decoding that guarantees a well-formed list. Requires iPhone 15 Pro or newer, Apple Intelligence switched on, and a supported language.
- **Android is the weak case.** Prompt API still beta, limited to a named flagship list; structured output is alpha and Kotlin-only, so Flutter gets free text to parse.
- **Coverage:** roughly a third of active iPhones (secondary-source estimate) and probably a single-digit share of Android phones (researcher's own estimate, unsourced).
- **Bundled models not recommended for v1.** `flutter_edge_ai` / `llamadart` work, but cost a 0.5–2.6 GB model download, ~1 GB RAM, extra iOS entitlements and Android `minSdk 30`. MediaPipe LLM Inference is maintenance-only.
- **Fallback:** check availability at runtime, hide the button when unavailable, drop silently to the manual flow on failure. Suggestions always editable, never auto-committed. On iOS 27, use only the on-device model, never the Private Cloud Compute profile.
- **Biggest caveat:** quality of suggested steps is unverified — no primary source evaluates task decomposition on these models, and Apple's docs say the on-device model is weak at reasoning and world knowledge. A throwaway prototype with real todos is needed before the spec commits (see ticket 17). Also unverified: `flutter_local_ai`'s on-device-only behaviour (pub.dev page read, not source); Gemini Nano download size.
