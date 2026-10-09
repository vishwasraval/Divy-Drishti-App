# Divy-Drishti (दिव्य-दृष्टि)

An offline-first, ad-free, privacy-focused Android accessibility app for blind, deaf and
speech-impaired users in India. One home screen, large buttons: **SEE** (दिव्य द्रष्टि),
**SIGN** (दिव्य भाषा), **SPEECH** (दिव्य श्रवण) and **Emergency**, with Settings behind the gear icon.

Published on Google Play as `com.divydrishti.app`. Current release: **1.0.1 (versionCode 2)** —
see [`docs/release/v1.0.1/`](docs/release/v1.0.1/) for the release report, model and dataset
inventories, release notes and signing procedure.

## Accessibility goals

- Every function reachable from one screen with large touch targets (≥ 48 dp; primary cards fill the screen).
- Everything important is spoken in the user's working language, and every control has a TalkBack label.
- Home layout adapts to the disability profile chosen at first run (blind: SEE + Emergency; deaf / non-verbal:
  SPEECH + SIGN + Emergency; otherwise all four).
- Honest AI: the app says when it is unsure, never invents objects, denominations, CAPTCHA characters,
  distances, words or translations, and never claims perfect accuracy.

## Features (verified against the source of this release)

**SEE — camera assistance, all on one screen, no mode buttons**
- Object detection: YOLOX-S (80 COCO classes) on LiteRT with GPU acceleration; automatic fallback to
  EfficientDet-Lite0 on low-RAM, slow or failing devices. Labels for TV/fridge/oven/microwave/laptop are
  double-checked by an EfficientNet-Lite2 classifier. Common Indian bird species via Google AIY birds_V1.
- Hazard warnings (approaching vehicles/animals, hazard signage text such as "Danger", "No Entry"),
  ambulance / police / fire-engine livery text, spoken with priority over everything else.
- Approximate distance and direction: hardware depth (ToF) → ARCore Depth (optional, never forces install)
  → camera focus distance → MiDaS relative depth (cross-check only, never metres alone) → geometric estimate.
  Spoken as ranges ("about two metres ahead"); "distance unavailable" rather than a guess.
- Indian currency: automatic, no button — reads the printed denomination numeral of notes and coins,
  says "possibly" without corroboration, never reads other note text. Not counterfeit detection.
- Reading mode (tap to start, tap to stop): CAPTCHA first, then printed text in 14 languages (ML Kit for
  Latin/Devanagari, Tesseract for Bengali-Assamese, Gujarati, Gurmukhi, Odia, Tamil, Telugu, Kannada,
  Malayalam), document type and language announced, translated to the working language where an offline
  model exists, honest notice where it does not.
- CAPTCHA (user-initiated, accessibility only — never solves or submits): characters spelled slowly
  ("Capital C", "small a", "two"), introduced and then repeated once; refused when the reading is unreliable.
  Read by ML Kit and cross-checked offline by PaddleOCR PP-OCRv2 on Paddle Lite (arm64): both must agree, or
  PaddleOCR must be confident on every character, otherwise the app says it cannot read it
  ([`docs/release/v1.0.1/CAPTCHA_FALLBACK.md`](docs/release/v1.0.1/CAPTCHA_FALLBACK.md)).
- QR codes: decoded continuously and read aloud; links are never opened automatically.

**SPEECH — Divy Shravan**
- Continuous speech-to-text using the device's speech recogniser (prefers offline), large high-contrast transcript.
- Translation into the working language with ML Kit on-device models (8 of 14 languages: hi, en, bn, gu, kn,
  mr, ta, te). Where translation is impossible, Hindi and English renderings are shown and marked.
- Spoken-language detection on Android 14+ (recogniser's own detection); text-script check on Android 13 and older.
- Conversation mode: typed replies are translated and spoken aloud.

**Emergency**
- One tap on the Emergency card: speaks a confirmation, gets the best available location, opens the user's
  own SMS app pre-addressed to their saved contacts with location and name (the user taps Send), then — on
  return to the app — opens the dialer with 112 pre-filled (never auto-dials). No `SEND_SMS`/`CALL_PHONE` permission.
- Contacts added at first run or in Settings; English names with transliteration help; numbers read digit by digit.

**SIGN — Divy Bhasha:** pipeline and training kit exist (`training/sign/`), but **no trained ISL model**, so the
card is shown dimmed and disabled in this release.

**First run:** language → disclaimer (spoken, voice or tap Yes/No) → disability profile → emergency contact →
permissions, all spoken in the chosen language. Setup does not continue without a Yes to the disclaimer
(text and translations: `docs/release/v1.0.2/DISCLAIMER.md`); it can be re-read in Settings → General →
Disclaimer.

**Voice Yes/No:** every Yes/No question (disclaimer, contact confirmation, "add another contact?", "open this
QR link?") accepts English "Yes"/"No" in every working language, as well as the working language's own words.
Silence or an unclear answer is never taken as an answer. Mobile numbers can be dictated with pauses; the app
waits for all 10 digits and reads them back.

## Supported languages

Hindi, English, Assamese, Bengali, Gujarati, Kannada, Malayalam, Marathi, Meitei (Bengali script),
Mizo, Odia, Punjabi, Tamil, Telugu — UI strings in all 14. Speech recognition, TTS voices and offline
translation depend on what the device and ML Kit provide for each language; the app reports gaps rather than hiding them.
Translations of UI text were machine-assisted and still need native-speaker review.

## Offline behaviour

| Works with no network | Needs a one-time download or a device capability |
|---|---|
| Object detection, hazards, distance, currency, OCR (all 14 scripts), QR, CAPTCHA, TTS (installed voices), Emergency (SMS app + dialer), settings, contacts | ML Kit translation models (downloaded once, then offline); offline speech-recognition language packs (device-dependent); ARCore (optional) |

## AI models and licences

See [`docs/release/v1.0.1/MODEL_INVENTORY.md`](docs/release/v1.0.1/MODEL_INVENTORY.md) and
[`docs/ML_LICENSES.md`](docs/ML_LICENSES.md). All bundled weights are Apache-2.0 or MIT; ML Kit and ARCore are
under Google's terms. No model was trained or fine-tuned by this project. Datasets used upstream (COCO, ImageNet,
iNaturalist …) and Indian-context candidates: [`docs/release/v1.0.1/DATASET_INVENTORY.md`](docs/release/v1.0.1/DATASET_INVENTORY.md).
In-app notices: Settings → About → Open-source licences (`app/src/main/assets/third_party_notices.txt`).

## Technology

Kotlin 2.3, Jetpack Compose (Material 3), AGP 9.3.1, compileSdk/targetSdk 37, minSdk 24, CameraX 1.5,
LiteRT 1.4.2 (+GPU), ML Kit (text, barcode, language ID, translate), Tesseract4Android 4.8.0, ARCore 1.56
(optional), MediaPipe Tasks (SIGN), Room + DataStore, WorkManager. No DI framework, no analytics SDK of its own, no ads.

## Architecture

```
app                Shell: nav host, AppContainer (hand-wired DI), onboarding, per-app locale.
core               Taxonomy, EngineResult, AppLanguage, preferences model, pure logic.
core-ui            Material 3 theme (light/dark/high contrast, scalable type), accessible components.
core-accessibility Haptics.            core-audio     TTS engine, priority speech queue.
core-camera        CameraX/ARCore hosts, depth streams.   core-location  LocationProvider.
core-ml            All AI engines: detectors, OCR, currency, QR, CAPTCHA, depth, speech recognition, translation.
data               DataStore preferences, Room emergency contacts.
feature-home | feature-vision | feature-speech | feature-isl | feature-emergency | feature-settings
```

## Build

Requires a JDK 17+ (Android Studio's bundled JBR) and the Android SDK in `local.properties`.

```bash
./gradlew :app:assembleDebug            # debug APK (package com.divydrishti.app.debug)
./gradlew testDebugUnitTest             # 203 JVM unit tests (2026-10-08: all pass)
./gradlew :app:lintRelease              # 0 errors expected
./gradlew :app:bundleRelease            # release AAB, signed if signing is configured
```

Instrumented tests (need a device; on MIUI prefer `adb install` + `adb shell am instrument`, since a failed
`connectedAndroidTest` uninstalls the app and wipes its data): `SeeDetectorOnDeviceTest`, `ReadingModeOnDeviceTest`.

`gradle.properties` sets `android.builtInKotlin=false` and `android.newDsl=false` because Room's KSP processor
needs the classic Kotlin Android plugin under AGP 9. AGP 10 removes this escape hatch — revisit when upgrading.

## Signing (no secrets here)

Release signing reads a git-ignored `keystore.properties` (template: `keystore.properties.template`) or the
`DIVYDRISHTI_STORE_FILE / _STORE_PASSWORD / _KEY_ALIAS / _KEY_PASSWORD` environment variables. Play App Signing
is enabled; the local key is only the **upload** key. Procedure, key fingerprint and the upload-key reset steps:
[`docs/release/v1.0.1/SIGNING_AND_RELEASE_PROCEDURE.md`](docs/release/v1.0.1/SIGNING_AND_RELEASE_PROCEDURE.md).
Never commit keystores, passwords or `keystore.properties`.

## Privacy and permissions

Permissions: `CAMERA`, `RECORD_AUDIO`, `ACCESS_FINE/COARSE_LOCATION` (only for the emergency message),
`VIBRATE`, `INTERNET` (ML Kit model downloads, Play in-app updates, optional ARCore). Camera frames, audio,
OCR text, CAPTCHA images and translations are processed in memory on the device and never stored or uploaded;
the app has no network code of its own. The bundled Google ML Kit SDK does send Google anonymous diagnostics and
usage metrics (device/app info, identifiers, latency, error codes — never images, audio or text); this is declared in
`docs/release/PLAY_DATA_SAFETY.md` and the privacy policy. Stored data: settings (DataStore) and emergency contacts (Room).
`allowBackup=false`. Settings → Privacy deletes all local data. Policy: `docs/release/PRIVACY_POLICY.md`.

## Known limitations

- SIGN is disabled: no trained Indian Sign Language model.
- Object detection covers COCO's 80 classes only — no auto-rickshaws, Indian utensils, potholes or open drains.
- Currency is read from printed numerals; no image classifier, no counterfeit detection.
- Offline translation exists for 8 of 14 languages; offline speech recognition depends on device language packs.
- CAPTCHA reading uses general OCR (ML Kit + PaddleOCR PP-OCRv2), not a CAPTCHA-trained model; heavily distorted
  CAPTCHAs are refused, not guessed. Real-site accuracy is not measured (synthetic test set only).
- Distance accuracy has not yet been measured on a phone; ARCore and ToF paths are unverified on hardware.
- Full list: [`docs/release/v1.0.1/AUDIT_AND_RELEASE_REPORT.md`](docs/release/v1.0.1/AUDIT_AND_RELEASE_REPORT.md).

## Contributing

Work on a branch, keep changes small, run unit tests and `lintRelease` before a PR, and test on a physical
phone for anything touching camera, speech or Emergency (never fire Emergency on a phone with real contacts).
Add a licence row to `docs/ML_LICENSES.md` / `DATASET_INVENTORY.md` before adding any model or dataset.

## Licence

No open-source licence has been granted for the Divy-Drishti source code (there is no `LICENSE` file); all
rights are reserved by the developers unless they state otherwise. Third-party components remain under their own
licences — see the in-app notices and `docs/ML_LICENSES.md`.
