# J.A.R.V.I.S. — 32-bit Android voice assistant

A Flutter voice assistant for **32-bit ARM (armeabi-v7a)** Android phones
(minSdk 24). Voice-first: wake word, conversation, reminders, timers,
calling, device controls, and Gemini chat — with your own Gemini API key,
stored encrypted on the device.

> Built for one phone (Infinix Smart 9, Android 14, 32-bit userspace) and
> one user. It is not a general-purpose public app.

## What is in this repo

| Path | What it is |
| --- | --- |
| `jarvis_app/` | The full Flutter source tree (browseable, the real source of truth) |
| `JARVIS-32bit-corrected-source.zip` | The same source, zipped — this is what CI builds from |
| `.github/workflows/android-armv7-build.yml` | CI: pub get → analyze → test → signed release APK on every push |
| `.github/workflows/release.yml` | CI: on a `v*` tag, builds the signed APK and publishes a GitHub Release |
| `RELEASE-NOTES-v1.2.0.md` | What is in the current release and what was skipped |

The ZIP is generated from `jarvis_app/` at release time; the two never
diverge by hand. CI builds from the ZIP because that path is proven
(17 Nov 2026: v1.0.8, run #14). A future change may switch CI to build
from the tree directly.

## Version

**v1.2.0 (versionCode 11).** One combined release, one APK, one download.
See `RELEASE-NOTES-v1.2.0.md`.

## Build

Local Gradle builds do not work in this environment (JVM loopback), and
there is no emulator route — builds run in GitHub Actions:

1. Push a new `JARVIS-32bit-corrected-source.zip` (or the workflow file).
2. CI runs `flutter pub get`, `flutter analyze`, `flutter test` (all must
   pass — no `continue-on-error`), then
   `flutter build apk --release --target-platform android-arm`.
3. The APK artifact is `jarvis-armv7-release`.
4. To publish: push a tag `v1.2.0` — the release workflow builds the
   signed APK and creates the GitHub Release with it attached.

Local checks that do work (Flutter 3.50.0 / Dart 3.14.0):

```bash
cd jarvis_app
flutter pub get
flutter analyze
flutter test
```

## Release signing

Release APKs are signed with the owner's keystore. The keystore and its
passwords live only in GitHub Secrets — never in this repo:

- `JARVIS_RELEASE_KEYSTORE_B64` — the keystore file, base64-encoded
- `JARVIS_RELEASE_STORE_PASSWORD`
- `JARVIS_RELEASE_KEY_ALIAS`
- `JARVIS_RELEASE_KEY_PASSWORD`

CI decodes the keystore at build time and writes `android/key.properties`
from the secrets. Without the secrets the build falls back to the debug
signing key (test installs only).

**One-time install note:** release signing is still deferred (the 4
secrets were never entered in GitHub), so v1.2.0 ships debug-signed
like v1.1.0. It installs only after uninstalling v1.1.0 once and
re-entering the Gemini key in the app (and re-pasting the Google /
Notion tokens if those connectors are used — app data is lost on
uninstall). Once signing is set up, signed updates will install over
the old version and the key stays.

## Gemini key

- Default model: `gemini-3.8-flash` (verified real). Never `gemini-2.5-flash`
  / `gemini-2.5-pro` (deprecated) or any `gemini-3.5-*` (unverified).
- No API key is embedded in the source or the APK — ever. Enter your own
  key in the app's Settings; it is encrypted with AES/GCM using a key
  generated in Android Keystore.
- Requests use Google's `generateContent` REST endpoint through the
  `http` package (hand-written client in `lib/gemini_client.dart`). The
  archived `google_generative_ai` SDK is deliberately not used.

## Features (v1.2.0)

Everything in v1.1.0, plus the 14 free Jarvis 2.0 ideas (G1–G13, G15;
G14 cloud backup was excluded by the owner's choice): Google Docs /
Sheets / Drive / Calendar / Gmail by voice (your own OAuth token),
on-device calendar events, email drafts + sends behind the
confirmation gate, named multi-step workflows with live step log and
per-workflow autonomy ("ask me first" vs "run fully on its own"),
scheduled workflows (Workmanager notification when the app is closed,
run-or-ask when it is open), specialist agent modes (Research /
Writer / Publisher), keyless web research (Wikipedia + DuckDuckGo),
attached files and images in a task (Gemini vision), Notion read /
write (your own integration token), assistant personality + custom
instructions, an on-device usage dashboard, an agent retry with
diagnosis (one retry, then an honest report), and a dark / light mode
toggle. The full list is in `RELEASE-NOTES-v1.2.0.md`. Features that
need optional free accounts (NVIDIA, Picovoice, Gmail OAuth, Google
Workspace OAuth, Notion) ship disabled until the user connects them.

## Permissions

Microphone, Internet, foreground-service (mic/dataSync), notifications,
battery-optimisation exemption, contacts, camera (torch only), usage
stats, overlay, calendar, SMS (optional), Accessibility (only for
do-the-task actions). Each is granted one by one in the in-app onboarding
screen; Accessibility and SMS are optional.

## License

MIT — see `LICENSE`. Ideas and patterns from other open-source Jarvis
projects are re-implemented fresh; no code, images, logos, audio, or keys
are copied from anywhere.
