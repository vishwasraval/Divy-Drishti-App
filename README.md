# Divy-Drishti — release documentation

> **Current release: 1.0.1 (versionCode 2)**, the first update after the published 1.0.0 (versionCode 1).
> Everything for it is in [`v1.0.1/`](v1.0.1/).

## `v1.0.1/` — current release

| File | What it is |
|---|---|
| `RELEASE_NOTES.md` | Play "What's new" text (English, Hindi) and every change since 1.0.0. **Read first.** |
| `SIGNING_AND_RELEASE_PROCEDURE.md` | Replacement upload key, Play upload-key reset, building and naming the AAB, universal APK for sideloading, version history. |
| `CAPTCHA_READING.md` | How CAPTCHAs are found and cross-checked, with the phone measurements. |
| `DISCLAIMER.md` | The setup disclaimer text and how consent is stored. |
| `APP_FEATURES_SUMMARY.md` | Short feature summaries (under 500 characters) for listings and notes. |
| `AUDIT_AND_RELEASE_REPORT.md` | 2026-10-08 production audit and smoke-test checklist. |
| `MODEL_INVENTORY.md`, `DATASET_INVENTORY.md` | Every bundled model and dataset, with licences. |
| `PERFORMANCE_AND_COMPATIBILITY.md` | Speed, memory and 16 KB page-size checks. |
| `DivyDrishti_*.docx` | Features and user guide, press release and testing report (2026-10-08, updated 2026-10-10). |

## This folder — Play Console submission (written for 1.0.0, still current unless noted)

| File | What it is |
|---|---|
| `PLAY_STORE_LISTING.md` | App name, short/full description, graphic-asset requirements. |
| `PLAY_DATA_SAFETY.md` | Answers for the Data safety form (corrected 2026-10-08 for ML Kit diagnostics). |
| `PRIVACY_POLICY.md` | Privacy policy text to host at a public HTTPS URL. |
| `PLAY_CONSOLE_DECLARATIONS.md` | App-content declarations and the text to enter. |
| `PLAY_CONTENT_RATING.md`, `PLAY_APP_CATEGORY.md` | Content-rating answers; category (Tools). |
| `PLAY_REVIEWER_NOTES.md` | Instructions for Google's review team. |
| `PLAY_PRESUBMISSION_CHECKLIST.md` | Tick-list for build, signing, Console content and listing. |
| `OSS_LICENSES_AND_MODELS.md` | Open-source notices and model-licence checks. |
| `RELEASE_READINESS_REPORT.md` | The 1.0.0 readiness audit (2026-08-28), kept for history. |
| `Screen-*.jpg`, `feature_graphic.png` | Store screenshots and feature graphic. |

Signing: copy `keystore.properties.template` → `keystore.properties` (git-ignored) and fill it in, or set the
`DIVYDRISHTI_*` environment variables on CI.
