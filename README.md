# Divy-Drishti — documentation

> **Current release: 1.0.1 (versionCode 6)**, 2026-11-19: fixes a speech crash in the release build; SPEECH listens
> at once; SEE and SPEECH keep the screen on; low-light help in SEE. Release files and checksums: `RELEASE_NOTES.md`.

All project documents are in this one folder.

## Word documents (for people outside the team)

| File | What it is |
|---|---|
| `DivyDrishti_Features_and_User_Guide.docx` | User guide; the source of truth for every claim in the store listing. |
| `DivyDrishti_Technical_Description.docx` | Architecture, AI models, privacy design and honest-AI principles. |
| `DivyDrishti_Testing_Report.docx` | Test cycles of 8–10 October (vc2), 16 October (vc3), 17–18 November (developed as vc4, released as vc5) and 19 November (vc6, no device run yet). |
| `DivyDrishti_Background_and_App_Overview.docx` | Why the app exists and what it does, for general readers. |
| `DivyDrishti_Problem_Audience_Impact.docx` | Problem, audience and impact answers (form-length). |
| `DivyDrishti_Comparative_Analysis.docx` | Comparison with nine other assistive apps. |
| `DivyDrishti_English.docx`, `_Hindi.docx`, `_Gujarati.docx` | Press and media description in three languages. |

## Releasing

| File | What it is |
|---|---|
| `RELEASE_NOTES.md` | Every release (vc6, vc5, vc3, vc2): Play "What's new" in English and Hindi, checksums, changes, phone evidence. |
| `SIGNING_AND_RELEASE_PROCEDURE.md` | Upload key, building and signing, debug symbols, universal APK for sideloading, installing with the speech pack, version history. |
| `PERFORMANCE_AND_COMPATIBILITY.md` | Size, speed, memory, 16 KB page size, edge-to-edge. |

## Google Play Console

| File | What it is |
|---|---|
| `PLAY_STORE_LISTING.md` | Name, short and full description, short feature summaries, screenshots and feature graphic. |
| `PLAY_CONSOLE_DECLARATIONS.md` | App content declarations, content rating answers, category and tags, permissions. |
| `PLAY_DATA_SAFETY.md` | Data safety form answers (ML Kit diagnostics declared). |
| `PLAY_REVIEWER_NOTES.md` | Instructions for Google's review team. |
| `PRIVACY_POLICY.md` | Privacy policy to host at a public HTTPS URL (re-publish after every change). |
| `Screenshot-*.png`, `feature_graphic.png` | Store screenshots (submitted 2026-10-13) and feature graphic. |

## AI models, data and features

| File | What it is |
|---|---|
| `MODEL_INVENTORY.md` | Every model the app ships or was asked to ship, with its real status and device evidence. |
| `ML_LICENSES.md` | Exact model files, sources, checksums and licences; models considered and rejected. |
| `DATASET_INVENTORY.md` | Datasets used upstream, Indian-context candidates, and rules for any future training. |
| `SEE_MODEL_CARD.md` | SEE detector: selection, label check, tracking across frames, limits, benchmarks. |
| `CAPTCHA_READING.md` | How CAPTCHAs are found and cross-checked, with phone measurements. |
| `DISTANCE_VALIDATION.md` | How to measure SEE's distance estimates on real phones (not yet done). |
| `DISCLAIMER.md` | The setup disclaimer in all languages and how consent is stored. |
| `SIGN_REPORT.md` | SIGN (Indian Sign Language): pipeline, status and why it is switched off. |

Licences shown in the app: Settings → General → About → Open-source licences
(`app/src/main/assets/third_party_notices.txt`).

Signing: copy `keystore.properties.template` → `keystore.properties` (git-ignored) and fill it in, or set the
`DIVYDRISHTI_*` environment variables on CI.
