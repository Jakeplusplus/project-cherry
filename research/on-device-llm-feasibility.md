# On-device LLM feasibility for task breakdown

Research for ticket `01-on-device-llm-feasibility`. Sources checked on 2026-10-07.

## Answer

**Yes, it is feasible, but only as an optional enhancement on a minority of phones. The guided manual breakdown must remain the real feature; AI is a "suggest steps" button that appears only when the phone has a system model.**

- **Recommended path: OS-provided models only, no bundled model.** Use Apple Foundation Models on iOS and Gemini Nano (ML Kit GenAI Prompt API / AICore) on Android, reached through one Flutter plugin. The best-fitting plugin today is `flutter_local_ai` (verified publisher, MIT, 0.2.1, updated 10 days ago). It adds zero model bytes to the app, needs no account or API key, and inference runs on the device.
- **iOS (the first target) is the strong case.** Foundation Models is a shipped, non-beta framework since iOS 26.0, and has constrained decoding ("guided generation") that guarantees the output shape, which is exactly what "a list of 5-8 short steps" needs. `flutter_local_ai` exposes this from Dart.
- **Android is the weak case.** The Prompt API is still beta, runs only on a named list of recent flagships, and its structured-output feature is alpha and Kotlin-only, so from Flutter you get free text that you must parse yourself.
- **Coverage is low.** Roughly a third of active iPhones (third-party estimate, not an Apple figure) and a small, unquantified slice of Android flagships. Most phones worldwide cannot use it. Treat that as acceptable only because the fallback is the full-depth manual workflow the map already requires.
- **Bundled models (Gemma via LiteRT-LM, GGUF via llama.cpp) work from Flutter but are not recommended for v1.** They cost a 0.5-2.6 GB model download, about 1 GB or more of RAM, extra iOS entitlements and a higher Android `minSdk`, and they need a network download step. That is a lot of moving parts for a Flutter beginner, for a feature the app must work without anyway.
- **Biggest caveat: output quality for this specific task is unverified.** I found no primary-source evaluation of "break a todo into 5-8 concrete steps" on any of these models. Apple's own docs say the on-device model is weak at logical reasoning and world knowledge. A one-day throwaway prototype on the author's iPhone is needed before the spec commits to the feature. It also needs to be confirmed that the author's iPhone is an Apple Intelligence model (15 Pro or newer) at all.

## Options compared

| | Apple Foundation Models (iOS) | Gemini Nano / AICore (Android) | Bundled model (LiteRT-LM or llama.cpp) |
|---|---|---|---|
| API status | Shipped, iOS 26.0+ | Prompt API beta; structured output alpha | Runtimes are open source; plugins pre-1.0 or fast-moving |
| Devices | iPhone 15 Pro/Pro Max, all iPhone 16, 17, 18 lines, Air | Named flagship list (Pixel 9+, Galaxy S26, Z Fold7/8, etc.) | Any recent arm64 phone with enough RAM |
| App size cost | 0 (OS owns the model) | 0 (AICore owns the model) | 0.5-2.6 GB model, usually downloaded after install |
| Structured output from Flutter | Yes, native constrained decoding | No (free text only) | Yes with llamadart (grammar/JSON); prompt-based otherwise |
| Licence | Apple acceptable-use requirements | Google ML Kit GenAI terms | Runtime MIT/Apache; model licence varies (Gemma 4 Apache 2.0, Gemma 3 gated) |
| Beginner effort | Low | Low to medium | High |

## 1. iOS: Apple Foundation Models

**What it is.** A system framework giving apps access to the on-device language model behind Apple Intelligence. Available from iOS 26.0, not marked beta. Source: https://developer.apple.com/documentation/foundationmodels

**Device and OS coverage.**
- Apple's docs: "To use Apple Foundation Models, people need a device that supports Apple Intelligence." https://developer.apple.com/documentation/foundationmodels
- Compatible iPhones per Apple: iPhone 15 Pro and 15 Pro Max, iPhone 16, 16 Plus, 16e, 16 Pro, 16 Pro Max, iPhone 17, 17e, 17 Pro, 17 Pro Max, iPhone Air, iPhone 18 Pro, 18 Pro Max, and "iPhone Duo". No iPhone 15 (non-Pro) or older. https://www.apple.com/apple-intelligence/
- The user must have Apple Intelligence switched on, and the model must have finished downloading. The API reports three unavailable reasons: `deviceNotEligible`, `appleIntelligenceNotEnabled`, `modelNotReady`. https://developer.apple.com/documentation/foundationmodels/systemlanguagemodel/availability-swift.enum/unavailablereason
- Apple tells developers directly: "Always verify model availability first, and plan for a fallback experience in case the model is unavailable." https://developer.apple.com/documentation/foundationmodels/generating-content-and-performing-tasks-with-foundation-models
- Device language must be one Apple Intelligence supports (English, Danish, Dutch, French, German, Italian, Norwegian, Portuguese, Spanish, Swedish, Turkish, Vietnamese, Chinese simplified and traditional, Japanese, Korean). https://support.apple.com/en-us/121115
- Apple Intelligence itself takes "up to 8 GB" of device storage on most supported iPhones and "up to 14 GB" on iPhone 17 Pro and newer. That is the user's storage, not the app's. https://support.apple.com/en-us/121115
- iOS 26 adoption: 79% of all iPhones and 86% of iPhones introduced in the last four years, measured 7 June 2026. Apple has not yet published an iOS 27 figure on that page. https://developer.apple.com/support/app-store/

**Structured output.** The framework has "guided generation", which "uses constrained sampling when generating output" and "prevents the model from producing malformed output". Schemas can also be built at runtime (`DynamicGenerationSchema`), which is what makes it reachable from Dart. https://developer.apple.com/documentation/foundationmodels/generating-swift-data-structures-with-guided-generation

**Limits.**
- Context window is 4096 tokens per session, covering instructions, prompt, schema and response. Ample for one task title plus 5-8 short steps. https://developer.apple.com/documentation/technotes/tn3193-managing-the-on-device-foundation-model-s-context-window
- Apple lists what the on-device model should not be used for: basic math, code, and logical reasoning. It lists summarising, extracting, classifying, tagging and short creative text as good fits. Task breakdown is on neither list. https://developer.apple.com/documentation/foundationmodels/generating-content-and-performing-tasks-with-foundation-models
- The model changes with OS updates. Apple documents three model versions so far (26.0-26.3, 26.4, 27.0) and tells developers to re-test prompts on each. https://developer.apple.com/documentation/foundationmodels/systemlanguagemodel and https://developer.apple.com/documentation/foundationmodels/updating-prompts-for-new-model-versions

**Privacy note for the hard constraint.** As of iOS 27 the framework also fronts Private Cloud Compute and other server models ("When you need more reasoning capabilities and context size, use Private Cloud Compute or any server model provider"). Cherry must use only the on-device `SystemLanguageModel` and never a server profile. https://developer.apple.com/documentation/foundationmodels

**Licensing.** Governed by Apple's acceptable-use requirements for the framework. Two clauses are worth knowing for an ADHD app: no outputs that make unsupervised decisions with material impact in high-risk domains including medical, and nothing that "enables dependency or spiraling user interactions detrimental to a user's mental health". Suggesting editable todo steps does not obviously touch either, but the app should not present itself as treatment. No fees are mentioned. https://developer.apple.com/apple-intelligence/acceptable-use-requirements-for-the-foundation-models-framework/

## 2. Android: Gemini Nano via AICore / ML Kit GenAI

**What it is.** Gemini Nano "runs in Android's AICore system service" and lets apps generate "without needing a network connection or sending data to the cloud". Apps reach it through the ML Kit GenAI APIs. https://developer.android.com/ai/gemini-nano

**Status.** The Prompt API "is offered in beta, and is not subject to any SLA or deprecation policy." The current artifact is `com.google.mlkit:genai-prompt:1.0.0-beta4`, API level 26+. https://developers.google.com/ml-kit/genai/prompt/android and https://developers.google.com/ml-kit/genai/prompt/android/get-started

**Device coverage.** Google publishes an explicit list, by model generation (page last updated 2026-10-07): https://developers.google.com/ml-kit/genai
- nano-v2: Honor Magic V5/7/7 Pro, iQOO 13, Motorola Razr 60 Ultra / Razr Ultra 2025, OnePlus 13/13s, OPPO Find N5, several POCO F7/F8/X7/X8 models, realme GT 7 Pro, Samsung Galaxy Z Fold7 and Z TriFold, vivo X200 FE / T4 Ultra, Xiaomi 14T Pro, 15, 15T, 15T Pro, 15 Ultra, 17, 17 Ultra.
- nano-v3: Pixel 9 and Pixel 10 families, Samsung Galaxy S26 / S26+ / S26 Ultra, OnePlus 15/15R, OPPO Find X8/X9 and Reno 14/15 Pro lines, vivo X200/X300 lines, Honor Magic 8 Pro, iQOO 15, Sony Xperia 1 VIII, Sharp AQUOS R11, Motorola Signature, realme GT 7T.
- nano-v4: Pixel 11 family, Samsung Galaxy Z Flip8 / Fold8 / Fold8 Ultra.

Mid-range and budget Android phones, and older flagships, are not on the list.

**Operational limits (all from Google's docs).**
- Different devices run different model generations, so the same prompt gives different results per phone. https://developers.google.com/ml-kit/genai
- Per-app inference quota: too many requests returns `BUSY`, and there is a longer-term battery quota. https://developers.google.com/ml-kit/genai
- Foreground only. https://developers.google.com/ml-kit/genai
- Input must be under 4000 tokens. Not a problem for this use. https://developers.google.com/ml-kit/genai/prompt/android/get-started
- Not supported on devices with an unlocked bootloader. https://developers.google.com/ml-kit/genai/prompt/android/get-started
- The model may need to be downloaded by AICore on first use (status `DOWNLOADABLE`), which needs network once. The task text is not part of that. https://developers.google.com/ml-kit/genai/prompt/android/get-started

**Structured output.** Exists but is alpha, "works in Kotlin only", and is generated at compile time from annotated Kotlin classes. https://developers.google.com/ml-kit/genai/prompt/android/structured-output  
Consequence for Flutter: `flutter_local_ai` states that on Android, supplying a schema throws `STRUCTURED_OUTPUT_UNSUPPORTED` because "there is no runtime schema API for a Dart map". https://pub.dev/packages/flutter_local_ai  
So on Android the app would prompt for a plain numbered list and parse lines defensively.

## 3. Flutter plugins for the OS models

There is no first-party Flutter plugin from Apple, Google or the Flutter team for either system model. All options are community packages. Figures are from pub.dev on 2026-10-07.

| Package | Version / age | Publisher | Likes | Covers | Structured output | Source |
|---|---|---|---|---|---|---|
| `flutter_local_ai` | 0.2.1, 10 days ago | vezz.io (verified) | 50 | iOS/macOS 26+, Android API 26+, Windows, Chrome | Apple: yes, mapped to native `GenerationSchema`. Android: no | https://pub.dev/packages/flutter_local_ai |
| `flutter_edge_ai_builtin_ai` | part of flutter_edge_ai 2.1.0, 1 day ago | sashadenisov.dev (verified) | (parent has 436 as flutter_gemma) | Same, as a thin adapter over `flutter_local_ai` | Prompt-based only | https://github.com/DenisovAV/flutter_edge_ai/tree/main/packages/flutter_edge_ai_builtin_ai |
| `flutter_native_ai` | 0.5.3, 20 days ago | pooka.app (verified) | 3 | iOS + Android, text and streaming only | No | https://pub.dev/packages/flutter_native_ai |
| `foundation_models_framework` | 0.2.1, 10 months ago | unverified | 25 | Apple only | "still in development" | https://pub.dev/packages/foundation_models_framework |
| `flutter_foundation_models` | 0.3.0, 8 months ago | unverified | 3 | Apple only | Yes, with code generation | https://pub.dev/packages/flutter_foundation_models |

Reading of maturity:
- `flutter_local_ai` is the best fit: one API over both OS models, an `isAvailable()` / `availabilityReason()` check that maps onto the fallback story, real constrained output on iOS, and another well-used package (`flutter_edge_ai`) now depends on it for its native layer, which is a mild endorsement. It builds from iOS 13 and reports unavailable at runtime below iOS 26, so it does not raise the app's deployment target. https://pub.dev/packages/flutter_local_ai and https://github.com/DenisovAV/flutter_edge_ai/tree/main/packages/flutter_edge_ai_builtin_ai
- It is still version 0.2.x from a single maintainer, with 50 likes. It requires Flutter 3.44+, Dart 3.12+, Xcode 26+ and Kotlin 2.3.21 on Android, so it pins the toolchain to very recent versions. https://pub.dev/packages/flutter_local_ai
- Risk control: the plugin surface Cherry needs is tiny (is it available, send one prompt, get a list back). If the plugin is abandoned, replacing it with a small hand-written platform channel is realistic. Hide it behind one Dart interface in the app.

## 4. Bundled-model options

**MediaPipe LLM Inference.** Google has put it in "maintenance-only mode" and recommends migrating to LiteRT-LM. Do not start a new project on it. https://developers.google.com/edge/mediapipe/solutions/genai/llm_inference

**`flutter_edge_ai` (formerly `flutter_gemma`).** The package was renamed in the last few days: `flutter_gemma` 1.11.4 is "the final release under the name flutter_gemma". Verified publisher, MIT, 436 likes on the old name, 634 GitHub stars, pushed today. Engines: LiteRT-LM, MediaPipe, ONNX Runtime and the built-in OS models. https://pub.dev/packages/flutter_gemma , https://pub.dev/packages/flutter_edge_ai , https://github.com/DenisovAV/flutter_edge_ai
- Setup cost: iOS 15.0+ with static pod linkage; for large models three extra entitlements (extended virtual addressing, increased memory limit, increased debugging memory limit); iOS Simulator is CPU only. Android LiteRT needs `minSdk 30` and is arm64 only, plus the INTERNET permission to download models. https://github.com/DenisovAV/flutter_edge_ai/blob/main/packages/flutter_edge_ai/README.md
- No constrained JSON output is documented; tool calling is model- or prompt-based. https://flutteredge.ai/docs/models
- The LiteRT-LM package's troubleshooting list (GPU crashes on Mali, a Play Store 16 KB page-size rejection, tool calls killing the app, all fixed in recent point releases) shows an actively maintained but fast-moving native stack. https://github.com/DenisovAV/flutter_edge_ai/blob/main/packages/flutter_edge_ai_litertlm/README.md

**`llamadart` (llama.cpp and LiteRT-LM).** 0.11.0 published today, verified publisher (leehack.com), MIT, 48 likes, 160 pub points. Prebuilt native binaries arrive through a build hook ("Consumers do not need a local C++ toolchain"). Lists "Structured JSON output and grammar support". Needs iOS 16.4+, Flutter 3.38+. Android GPU (Vulkan) is "experimental, opt-in". Pre-1.0. https://pub.dev/packages/llamadart and https://github.com/leehack/llamadart

**`llama_cpp_dart`.** 0.2.2, 9 months old, unverified uploader; you must compile llama.cpp yourself. Not suitable for a beginner. https://pub.dev/packages/llama_cpp_dart

**Model size, memory and licence.**
- Gemma 4 E2B (`.litertlm`): 2.58 GB file, Apache 2.0, memory "as low as 0.8 GB" text-only; about 52-56 tokens/s on a Galaxy S26 Ultra or iPhone 17 Pro GPU. Those are top-end phones. https://huggingface.co/litert-community/gemma-4-E2B-it-litert-lm and https://ai.google.dev/gemma/docs/core/model_card_4
- Gemma 3 1B: about 529 MB to 1 GB depending on quantisation, about 0.5-1 GB memory, but under the Gemma licence and gated on Hugging Face (accept terms, token needed to download). https://huggingface.co/litert-community/Gemma3-1B-IT
- Qwen3 0.6B: 586 MB. https://flutteredge.ai/docs/models
- Store limits make shipping the model inside the app impractical or unattractive: Google Play's base module limit is 500 MB with 1.5 GB per asset pack; iOS allows up to 4 GB uncompressed. In practice the model would be downloaded on first use. https://support.google.com/googleplay/android-developer/answer/9859372 and https://developer.apple.com/help/app-store-connect/reference/maximum-build-file-sizes/

A first-use model download does not send task text anywhere, so it does not break the privacy constraint. It does add a network dependency, a multi-hundred-megabyte wait, storage pressure and a failure path to an app that otherwise never touches the network.

## 5. Expected quality for 5-8 concrete steps

Evidence here is thin. Stated plainly:

- **Format reliability on iOS: high.** Constrained decoding guarantees a well-formed list of strings, and guides can bound the count. This is documented behaviour, not an inference. https://developer.apple.com/documentation/foundationmodels/generating-swift-data-structures-with-guided-generation
- **Format reliability on Android and with small bundled models: moderate.** Free text needs line parsing and a "could not understand, try again or do it by hand" path.
- **Content usefulness: unknown.** No primary source evaluates task decomposition. Reasonable expectation, based on Apple's capability lists (good at short generation, weak at reasoning and world knowledge), is that common tasks ("clean the kitchen", "file taxes") get plausible generic steps, while personal or domain-specific tasks get vague or wrong ones. The model has no knowledge of the user's situation beyond the title. https://developer.apple.com/documentation/foundationmodels/generating-content-and-performing-tasks-with-foundation-models
- **Consistency across phones: poor by design.** Three Apple model versions and three Gemini Nano generations are in the field, and both vendors say outputs differ between them. https://developer.apple.com/documentation/foundationmodels/systemlanguagemodel and https://developers.google.com/ml-kit/genai

Design implications that follow regardless of quality: present output as editable suggestions inside the manual breakdown flow, never auto-commit steps, and do not ask the model for time estimates (Apple says to avoid math-like tasks, and the estimate bucket is the user's training exercise anyway).

## 6. What fraction of phones could use it

No vendor publishes this number. Best available:

- **iPhone.** Counterpoint Research reports Apple had shipped over 450 million Apple Intelligence-capable iPhones (published 8 June 2026; I could read only the headline). https://counterpointresearch.com/en/insights/apple-has-shipped-over-450-million-apple-intelligence-capable-iphones  
  A secondary commentary site puts capable devices at "roughly a third of the active iPhone user base entering fall 2026". Low-confidence source, but consistent with the shipment figure. https://appsops.store/news/apple-intelligence-device-gap-ios26-app-strategy  
  The usable share is lower again: the phone must be on iOS 26+ (79% of all iPhones in June 2026, though nearly all capable phones will be), have Apple Intelligence turned on, and use a supported language. https://developer.apple.com/support/app-store/
- **Android.** Only the flagship list in section 2. I found no credible installed-base percentage. Given the list excludes all mid-range and budget phones and most phones older than about two years, my estimate is a single-digit percentage of active Android phones. That is my inference, not a sourced figure.
- **Overall.** A clear minority of phones. For a public release, assume most users will never see the AI button. For the author (iOS, first test target) it depends entirely on which iPhone they carry.
- **Bundled models** would widen coverage to most arm64 phones from the last few years with enough free RAM and storage, at the costs in section 4. No sourced percentage for this either.

## 7. Fallback story

1. **Baseline for everyone:** the guided manual breakdown, which the map already specifies at full depth. It is the feature; AI is not required for anything.
2. **Capability check at runtime:** call the plugin's availability check when the breakdown screen opens. Show "Suggest steps" only when the model is ready. Hide it otherwise, with no error and no nagging. Both vendors require this pattern.
3. **Soft states:** if Apple Intelligence is off or the model is still downloading, optionally show a one-line hint in settings, not in the task flow.
4. **Failure at call time** (Android quota `BUSY`, unsupported language, guardrail refusal, unparseable text): drop silently back to the manual flow with whatever the user had typed intact.
5. **Not in v1:** downloading a bundled model as a second-tier fallback. Revisit only if the prototype shows AI suggestions are valuable enough to justify it. If revisited, `llamadart` (constrained JSON, no toolchain) or `flutter_edge_ai` with Gemma 4 E2B (Apache 2.0) are the candidates.

## Unknowns / could not verify

- **Quality of step suggestions.** No primary-source evaluation exists for this task on any of the models. Needs a prototype with 20-30 real todos on a real device.
- **Whether the author's iPhone supports Apple Intelligence.** If it is older than iPhone 15 Pro, the iOS path cannot even be tested without new hardware; the iOS Simulator behaviour for Foundation Models was not checked.
- **Share of phones.** The iPhone "roughly a third" figure is from a secondary site, and I could read only the headline of the Counterpoint report. The Android figure is my own estimate. Apple's adoption page still shows June 2026 data with no iOS 27 number.
- **Apple Intelligence requirement wording.** Apple's support page currently lists iOS 27 under software requirements, while the developer docs state Foundation Models is available from iOS 26.0. I read this as the support page describing the current release, but did not confirm that Foundation Models still works on a device left on iOS 26.
- **`flutter_local_ai` internals.** I read the pub.dev page, not the source. Not verified: that it only ever uses the on-device `SystemLanguageModel` (and never an iOS 27 Private Cloud Compute profile), how it behaves on each unavailable reason, and how its JSON schema subset maps to Apple's guides (for example bounding list length to 5-8). Its GitHub repository (https://github.com/kekko7072/flutter_local_ai) was not inspected for issue count or test coverage.
- **Exact size of the Gemini Nano download** that AICore performs, and whether it requires Wi-Fi. Google's docs describe the download but I found no size.
- **AICore privacy guarantees in detail.** The Android overview says no data is sent to the cloud; I could not load a dedicated AICore architecture page (404) to cite the isolation mechanism.
- **Google's ML Kit GenAI terms.** A "GenAI API Terms" page exists in the ML Kit docs navigation; I did not read it.
- **Real memory use of bundled models on mid-range phones.** Published benchmarks are for top-end devices only (Galaxy S24/S25/S26 Ultra, iPhone 17 Pro).
- **Added binary size of the LiteRT-LM and llama.cpp runtimes** themselves (separate from the model). Neither README states it.
- **llamadart's structured-output API details.** The pub.dev page lists the feature; the guide page I tried returned 404.
- **Apple's on-device model size and architecture.** Not stated in the developer docs I read, so not asserted here.
