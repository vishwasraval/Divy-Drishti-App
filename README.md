# Divy-Drishti (दिव्य-दृष्टि)

An offline-first, ad-free, privacy-focused Android accessibility app for blind, deaf and
speech-impaired users in India. One home screen, large buttons: **SEE** (दिव्य द्रष्टि),
**SIGN** (दिव्य भाषा), **SPEECH** (दिव्य श्रवण) and **Emergency**, with Settings behind the gear icon.

Published on Google Play as `com.divydrishti.app`. Current build: **1.0.1 (versionCode 4)**, which adds
fully offline speech recognition (see SPEECH below). Release report, model and dataset inventories, release
notes and signing procedure: [`docs/release/v1.0.1/`](docs/release/v1.0.1/).

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
  Objects are tracked across frames with camera-motion compensation, so a moving or unsteady phone keeps them.
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
  Found by its position next to a prompt ("Enter Captcha", "Type the text shown in the image"…) — letters,
  digits or both — or as a random-looking jumble in a close-up; read by ML Kit and cross-checked offline by
  PaddleOCR PP-OCRv2 on Paddle Lite (arm64), which vetoes a reading it contradicts. A visible prompt with an
  unreadable CAPTCHA is reported as such, never read as page text
  ([`docs/release/v1.0.1/CAPTCHA_READING.md`](docs/release/v1.0.1/CAPTCHA_READING.md)).
- QR codes: decoded continuously and read aloud; links are never opened automatically.

**SPEECH — Divy Shravan**
- Continuous speech-to-text, large high-contrast transcript, **entirely on the phone with the app's own models**:
  no Google speech service, no system language packs, no network.
  - English: NVIDIA NeMo Conformer-CTC small (int8, CC-BY-4.0).
  - Assamese, Bengali, Gujarati, Hindi, Kannada, Malayalam, Marathi, Meitei (Meetei Mayek), Odia, Punjabi,
    Tamil, Telugu: one shared AI4Bharat IndicConformer 600M multilingual model (int8, MIT).
  - Mizo: no open speech model exists; the app says so instead of falling back to an online service.
  - Run on ONNX Runtime with the app's own NeMo log-mel front end and utterance segmentation (`core-ml/.../speech/`).
- Spoken-language detection from the audio itself across the Indian languages, on every Android version
  (it switches only between languages in different scripts; Hindi/Marathi and Bengali/Assamese sound too alike).
- Translation into the working language with ML Kit on-device models (8 of 14 languages: hi, en, bn, gu, kn,
  mr, ta, te). Where translation is impossible, Hindi and English renderings are shown and marked.
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
(text and translations: `docs/release/v1.0.1/DISCLAIMER.md`); it can be re-read in Settings → General →
Disclaimer.

**Voice Yes/No:** every Yes/No question (disclaimer, contact confirmation, "add another contact?", "open this
QR link?") accepts English "Yes"/"No" in every working language, as well as the working language's own words.
Silence or an unclear answer is never taken as an answer. Mobile numbers can be dictated with pauses, as
digits in English or in the working language; the app waits for all 10 digits and reads them back. Short
answers are also read by the English model, so an English "No" or "nine eight…" works while the app is in an
Indian language; that reading only counts when it is nothing but a yes/no or digit words.

## Supported languages

Hindi, English, Assamese, Bengali, Gujarati, Kannada, Malayalam, Marathi, Meitei, Mizo, Odia, Punjabi,
Tamil, Telugu — UI strings in all 14.

| | Languages |
|---|---|
| Offline speech recognition (bundled) | 13 — all except Mizo |
| OCR (bundled) | all 14 scripts |
| Offline translation (ML Kit, downloaded once) | 8 — hi, en, bn, gu, kn, mr, ta, te |
| Text-to-speech | whatever offline voices the phone has installed; Hindi voice as fallback |

The app reports gaps rather than hiding them. Translations of UI text were machine-assisted and still need
native-speaker review.

## Offline behaviour

| Works with no network | Needs a one-time download or a device capability |
|---|---|
| Object detection, hazards, distance, currency, OCR (all 14 scripts), QR, CAPTCHA, speech recognition (13 languages, models installed with the app), TTS (installed offline voices only, never network voices), Emergency (SMS app + dialer), settings, contacts | ML Kit translation models (downloaded automatically after first-run setup, retried when a network appears, then offline); ARCore (optional) |

Nothing silently falls back to an online service. Verified on a Redmi 10 Prime (Android 13) after a reboot with
Wi-Fi and mobile data off: speech recognition in 13 languages, voice Yes/No and phone-number dictation.

## AI models and licences

See [`docs/release/v1.0.1/MODEL_INVENTORY.md`](docs/release/v1.0.1/MODEL_INVENTORY.md) and
[`docs/ML_LICENSES.md`](docs/ML_LICENSES.md); the speech models are pinned (Hugging Face revision + SHA-256) in
[`speech_models/models.tsv`](speech_models/models.tsv) and described in [`ml-models/README.md`](ml-models/README.md).
All bundled weights are Apache-2.0, MIT or CC-BY-4.0 (the English speech model); ML Kit and ARCore are
under Google's terms. No model was trained or fine-tuned by this project. Datasets used upstream (COCO, ImageNet,
iNaturalist …) and Indian-context candidates: [`docs/release/v1.0.1/DATASET_INVENTORY.md`](docs/release/v1.0.1/DATASET_INVENTORY.md).
In-app notices: Settings → About → Open-source licences (`app/src/main/assets/third_party_notices.txt`).

## Technology

Kotlin 2.3, Jetpack Compose (Material 3), AGP 9.3.1, compileSdk/targetSdk 37, minSdk 24, CameraX 1.5,
LiteRT 1.4.2 (+GPU), ONNX Runtime 1.22 (speech recognition), ML Kit (text, barcode, language ID, translate),
Tesseract4Android 4.8.0, ARCore 1.56
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
speech_models      Install-time Play asset pack with the speech models (~700 MB; fetched at build time, not in git).
```

## Build

Requires a JDK 17+ (Android Studio's bundled JBR) and the Android SDK in `local.properties`.

```bash
./gradlew :app:assembleDebug            # debug APK (package com.divydrishti.app.debug) - no speech models
./gradlew :app:bundleDebug              # debug AAB including the speech_models asset pack
./gradlew testDebugUnitTest             # 307 JVM unit tests (2026-11-17: all pass)
./gradlew :app:lintRelease              # 0 errors expected
./gradlew :app:bundleRelease            # release AAB, signed if signing is configured
```

**Speech models.** The first build that needs the asset pack runs `:speech_models:fetchSpeechModels`, which
downloads ~700 MB from the pinned Hugging Face revisions in `speech_models/models.tsv` (resuming and retrying),
checks every SHA-256, and places them in the git-ignored `speech_models/src/main/assets/speech/`. Later builds
only re-verify. To reuse an existing copy, pass `-Pdivydrishti.speechModelCache=<folder>` with the same layout.
Asset packs are only included in bundles: a plain `assembleDebug` APK has no speech models, and SPEECH then
reports them as missing. To run a debug build with them on a phone, build `bundleDebug` and install it with
bundletool (`build-apks --connected-device`, then `install-apks`). On an 8 GB build machine, use
`--max-workers=2` if a build is killed for low memory.

JVM tests of the speech pipeline use the desktop ONNX Runtime (pinned to 1.17.3: newer Windows builds need a newer
MSVC runtime than Android Studio's JBR ships). `BundledSpeechModelsTest` runs the real models when
`DIVYDRISHTI_SPEECH_MODELS` points to a folder with `en/` and `indic/` (plus sample WAVs), and is skipped otherwise.

Instrumented tests (need a device; on MIUI prefer `adb install` + `adb shell am instrument`, since a failed
`connectedAndroidTest` uninstalls the app and wipes its data): `SeeDetectorOnDeviceTest`, `ReadingModeOnDeviceTest`,
`OfflineSpeechLanguagesOnDeviceTest` (the phone's offline TTS speaks a sentence per language, the bundled models
transcribe it), `VoiceAnswersOnDeviceTest` (Yes/No and phone digits through the real speaker and microphone; needs a
quiet room). Set `adb shell setprop log.tag.OnDeviceSpeech DEBUG` to log what the recogniser heard.

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
`VIBRATE`, `INTERNET` (ML Kit model downloads, Play in-app updates, optional ARCore), `ACCESS_NETWORK_STATE`
(retry a missed translation-model download when a network appears). Camera frames, audio,
OCR text, CAPTCHA images and translations are processed in memory on the device and never stored or uploaded;
the app has no network code of its own. The bundled Google ML Kit SDK does send Google anonymous diagnostics and
usage metrics (device/app info, identifiers, latency, error codes — never images, audio or text); this is declared in
`docs/release/PLAY_DATA_SAFETY.md` and the privacy policy. Stored data: settings (DataStore) and emergency contacts (Room).
`allowBackup=false`. Settings → Privacy deletes all local data. Policy: `docs/release/PRIVACY_POLICY.md`.

## Known limitations

- SIGN is disabled: no trained Indian Sign Language model.
- Object detection covers COCO's 80 classes only — no auto-rickshaws, Indian utensils, potholes or open drains.
- Currency is read from printed numerals; no image classifier, no counterfeit detection.
- Offline translation exists for 8 of 14 languages. Speech recognition has no Mizo model.
- Speech recognition was tested on a phone with TTS-generated speech and the real microphone, not yet with a
  range of real speakers and accents. Assamese was clearly weaker (character error rate 0.36, against 0.00–0.09
  for the other 12). English words inside an Indian-language conversation are written phonetically in that script.
- The speech models add ~700 MB to the installed app (~455 MB Play download). The Indian-language model takes
  ~8 s to load on a Helio G88 the first time SPEECH is used, and is freed again after 60 s without listening.
- CAPTCHA reading uses general OCR (ML Kit + PaddleOCR PP-OCRv2), not a CAPTCHA-trained model; heavily distorted
  CAPTCHAs are refused, not guessed. Synthetic phone test: 34 of 120 correct, 18 wrong; real-site accuracy not measured.
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
