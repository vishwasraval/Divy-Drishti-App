# Divy-Drishti — release documentation

> **Current release: 1.0.1 (versionCode 4)**, uploaded to Play Console on 2026-11-18. It adds offline speech
> recognition with models bundled in the app. Everything for 1.0.1 is in [`v1.0.1/`](v1.0.1/).

## `v1.0.1/` — current release

| File | What it is |
|---|---|
| `RELEASE_NOTES_VC4.md` | **Latest.** vc4 Play "What's new" (English, Hindi), checksums, changes since vc3, phone evidence. |
| `RELEASE_NOTES_VC3.md` | vc3: auto-restart after update, card layouts, 16 KB and edge-to-edge fixes. |
| `RELEASE_NOTES.md` | vc2: Play "What's new" and every change since 1.0.0. |
| `SIGNING_AND_RELEASE_PROCEDURE.md` | Replacement upload key, Play upload-key reset, building and naming the AAB, universal APK for sideloading, version history. |
| `CAPTCHA_READING.md` | How CAPTCHAs are found and cross-checked, with the phone measurements. |
| `DISCLAIMER.md` | The setup disclaimer text and how consent is stored. |
| `APP_FEATURES_SUMMARY.md` | Short feature summaries (under 500 characters) for listings and notes. |
| `AUDIT_AND_RELEASE_REPORT.md` | 2026-10-08 production audit and smoke-test checklist. |
| `MODEL_INVENTORY.md`, `DATASET_INVENTORY.md` | Every bundled model and dataset, with licences (speech models added for vc4). |
| `PERFORMANCE_AND_COMPATIBILITY.md` | Speed, memory and 16 KB page-size checks. |

The Word documents (features and user guide, testing report, technical description, comparison, press texts in
English, Hindi and Gujarati) are in [`docs/`](../); they describe vc4.

## This folder — Play Console submission

| File | What it is |
|---|---|
| `PLAY_STORE_LISTING.md` | App name, short/full description, graphic-asset requirements, screenshots submitted. |
| `PLAY_DATA_SAFETY.md` | Answers for the Data safety form (ML Kit diagnostics declared; speech is on-device since vc4). |
| `PRIVACY_POLICY.md` | Privacy policy text to host at a public HTTPS URL (updated 2026-11-18 for on-device speech). |
| `PLAY_CONSOLE_DECLARATIONS.md` | App-content declarations and the text to enter. |
| `PLAY_CONTENT_RATING.md`, `PLAY_APP_CATEGORY.md` | Content-rating answers; category (Tools). |
| `PLAY_REVIEWER_NOTES.md` | Instructions for Google's review team. |
| `Screenshot-*.png`, `feature_graphic.png` | Store screenshots (submitted 2026-10-13) and feature graphic. |

Licences of bundled models and libraries: [`docs/ML_LICENSES.md`](../ML_LICENSES.md) and the in-app notices
(`app/src/main/assets/third_party_notices.txt`).

Signing: copy `keystore.properties.template` → `keystore.properties` (git-ignored) and fill it in, or set the
`DIVYDRISHTI_*` environment variables on CI.
