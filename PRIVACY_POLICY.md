# Divy-Drishti — Privacy Policy

**Last updated:** 2026-10-08
**Applies to:** Divy-Drishti Android application (package `com.divydrishti.app`), all versions from 1.0.0.

> Publish this text at a stable public HTTPS URL (e.g. a GitHub Pages site or the
> project website) and enter that URL in Play Console → App content → Privacy
> policy. Google requires the policy to be reachable without login and to match
> the app's actual behaviour and the Data Safety form.

---

## Summary

Divy-Drishti is an offline-first accessibility app for blind, deaf and
non-speaking users. **It does not have user accounts, does not show ads, and does
not include any analytics, advertising or tracking SDK of its own.** The Google ML Kit library it uses for on-device recognition sends Google anonymous performance and usage metrics (see section 3) — never your images, audio or text. Camera images, microphone
audio and location are processed on your device and are **never uploaded to us or
to any third party by this app**. We (the developers) receive **no personal data
from your use of the app**.

## 1. Who we are

Divy-Drishti is developed by the Divy-Drishti project team. For privacy questions,
contact: **<INSERT CONTACT EMAIL>**.

## 2. Data the app processes on your device (never transmitted by the app)

| Data | Why | Where it goes |
|---|---|---|
| **Camera frames** | Real-time object, hazard, text (OCR) and currency-denomination recognition in the "See" feature | Held in memory only, analysed frame-by-frame, then discarded. Never written to storage. Never uploaded. |
| **Microphone audio** | Speech-to-text in the "Speech" feature | Passed to the phone's speech-recognition service (Android `SpeechRecognizer`), asking for offline recognition. Not recorded to a file by this app. If your phone has no offline speech pack for the language, that service (usually Google's) may recognise the speech online; the app shows a notice on screen whenever this happens. |
| **Precise / approximate location** | Included in the text of an emergency (SOS) alert so your chosen contacts know where you are | Read only at the moment you trigger SOS. Inserted into the alert message you send. Not stored, not logged, not uploaded to the developers. |
| **Emergency contact names and phone numbers** | So the app can pre-fill an SOS alert to the people you choose | Stored **only** in a local database on your device. Never uploaded. Excluded from Google cloud backup. |
| **App settings** (language, accessibility options, speech rate, an optional note you add to SOS alerts) | To run the app the way you configured it | Stored **only** in local app storage on your device. Excluded from Google cloud backup. |

There is **no code path** in the app that writes camera, audio or video data to
disk or sends it to a network endpoint.

## 3. Network connections the app makes

The app is designed to work fully offline. The only network activity is with
**Google** (Google Play services, Google ML Kit, ARCore), over HTTPS, for:

- **One-time download of on-device recognition models** (text recognition,
  language identification, and offline translation language packs) the first time
  a given capability or language pair is used. After download, these run offline.
- **Google Play in-app update checks** — the app asks Google Play whether a newer
  version exists and, if you choose to update, hands off to Google Play's own
  update flow. The app cannot and does not download or install APKs itself.
- **ML Kit diagnostics and usage metrics** — Google ML Kit sends Google information
  about how its recognition features perform: device and app information (such as
  phone model, Android version and app version), device or other identifiers,
  performance metrics (such as processing time), feature configuration, event types
  and error codes. Google states that ML Kit never sends the images, text or other
  content it processes, that this data is encrypted in transit and that it is not
  shared with third parties
  ([ML Kit data disclosure](https://developers.google.com/ml-kit/android-data-disclosure)).
  The developers do not receive this data.
- **ARCore (optional)** — only if you switch ARCore on in Settings → SEE.

These connections are governed by
[Google's Privacy Policy](https://policies.google.com/privacy) and the
[ML Kit terms](https://developers.google.com/ml-kit/terms). The content you point
the camera or microphone at is **not** sent to Google by these on-device models.

Cleartext (non-HTTPS) traffic is disabled at the OS level for this app.

## 4. Emergency (SOS) feature — specific notes

- When you activate SOS, the app **opens your phone's own SMS app** pre-addressed
  to the emergency contacts **you have saved in the app**, with a pre-written
  message (your name if you set one, a maps link to your location if available,
  and the time). **You** review and complete the send. The app does not send SMS
  silently.
- SOS messages go **only to your saved emergency contacts**. When you return to
  the app afterwards, it opens your phone's dialer with **112** filled in so you can
  call if you choose; the app never places a call itself and holds no calling or
  SMS permission.
- Delivery of an SMS **cannot be confirmed by the app** and is never claimed as
  guaranteed. See the in-app emergency disclaimer.
- SOS is started with one tap on the Emergency button on the home screen. The app
  does **not** run a background service and does **not** draw over other apps.

## 5. Permissions and why each is requested

| Permission | Purpose | Required? |
|---|---|---|
| Camera | Object / hazard / text / currency recognition | Optional — only the "See" feature needs it |
| Microphone (`RECORD_AUDIO`) | Speech-to-text | Optional — only the "Speech" feature needs it |
| Location (fine / coarse) | Location line in SOS alerts | Optional — SOS works without it, but cannot include your location |
| Vibration (`VIBRATE`) | Haptic confirmation of gestures and SOS | Always available; no runtime prompt |
| Internet | On-device model downloads and Play update checks (see §3) | Used for first-time model download only |

The app requests **no** SMS, phone-call, contacts, storage, Bluetooth or
background-location permissions.

Declining any permission never crashes the app; the affected feature simply asks
again next time you open it. You can review and change every permission in
**Settings → Privacy**, which links directly to the system permission screen.

## 6. Data sharing

We do **not** sell, rent or share any personal data, and the developers receive
none. Apart from the ML Kit diagnostics described in section 3 (sent to Google, not
to us), data leaves your device only when **you** send an SOS SMS or place a call
through your phone's own apps, to the recipients **you** chose.

## 7. Data retention and deletion

- Emergency contacts and settings persist on your device until you delete them.
- **Settings → Privacy → Delete all local data** erases the local settings store
  and the emergency-contacts database and returns the app to its first-run state.
- Uninstalling the app removes all of its local data.
- We hold no server-side data. ML Kit diagnostics are held by Google under its
  privacy policy and are not linked to an account.

## 8. Children

The app is a general-purpose accessibility tool and is not directed at children.
It contains no ads and the developers collect no personal data.

## 9. Security

- All app network traffic is HTTPS; cleartext is disabled.
- On-device data is stored in private app storage and excluded from cloud backup.
- The app holds no credentials, API keys or secrets.

## 10. Changes to this policy

Material changes will be reflected here with an updated "Last updated" date and,
where significant, noted in the app's release notes.

## 11. Contact

**<INSERT CONTACT EMAIL>**
