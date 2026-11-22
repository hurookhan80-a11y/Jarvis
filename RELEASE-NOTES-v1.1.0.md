# Jarvis v1.1.0 — Release notes

**One combined release. One APK. One download.**
Version `1.1.0` (versionCode 10) · package `com.jarvis.jarvis_app` · minSdk 24 · armeabi-v7a only · Flutter 3.50.0 · default model `gemini-3.8-flash`.

## One-time install note (read first)

v1.1.0 is the **first signed release**. It installs only after **uninstalling the debug-signed v1.0.8 once** and **re-entering the Gemini key** in the app. After that, signed updates install over the old version and the key stays. All your data (reminders, history, saved facts) is lost with the uninstall — that is unavoidable this once.

## What is new (42 items)

**Stop & reliability**
- **F1 — STOP that actually stops.** Saying "stop" / "chup" / "khamosh", tapping the orb, or pressing STOP now works mid-sentence *and* mid-wait: the in-flight Gemini request is cancelled and no late answer ever speaks afterwards.
- **F7 — Durable reminders.** Reminders now survive the app being swiped away and a phone restart (Workmanager + notification backstop; the in-app timer is still the exact path while the app is alive).
- **F8 — Dependency refresh.** All 10 plugins at their latest compatible versions (incl. `flutter_contacts` 2.x migration — voice calling by name still works).
- **F9 — History cap.** Stored sessions capped at 20 (was 30).

**Voice**
- **F10 — Edge TTS.** A natural UK-English voice (Sonia) as the default; falls back silently to the phone voice if Edge is unreachable. Toggle in Settings.
- **F11 — Streaming replies.** Answers appear and start speaking sentence-by-sentence instead of waiting for the full reply. STOP works mid-stream. Toggle in Settings.
- **F12 — Audio acknowledgments.** Short fresh tones: blip on wake-word, ding on command heard, low thud on error.
- **F13 — Session recording (debug, OFF by default).** Records mic audio + transcript to on-device files only, never uploaded.
- **F14 — Offline wake word.** "Hey Jarvis" now works without internet (openWakeWord, on-device model). The old system listener stays as fallback.
- **F16 — Barge-in + follow-up.** Say "Hey Jarvis" while Jarvis is speaking to interrupt; after an answer, Jarvis keeps listening ~8 s for a follow-up without the wake word.

**Memory & providers**
- **F17 — Conversational memory.** "Remember that my exam is on 5 December" — Jarvis saves facts, uses them in answers, and shows them under ⋮ → Saved facts (view/delete).
- **F18 — Provider abstraction.** Gemini sits behind a clean provider interface (no behaviour change for you).
- **F19 — Connector hub.** ⋮ → Connected apps & providers: Gemini (always on), NVIDIA NIM, Gmail — each with its own key field, stored encrypted on-device.
- **F20 — NVIDIA NIM fallback.** With a free NVIDIA key entered, Jarvis answers via the free Nemotron model only when Gemini is overloaded (429/timeout) — and labels the reply "Answered by fallback model".
- **F21 — On-device search.** ⋮ → Search this phone: searches chats, reminders, saved facts and knowledge files, offline.
- **F22 — Knowledge files.** ⋮ → Knowledge files: paste text as `.txt` files; Jarvis quotes them (with the file name) when relevant.

**Everyday tools**
- **F23 — Floating bubble.** Optional floating "J" bubble over other apps (toggle OFF by default; needs overlay permission).
- **F24 — Default assistant.** Settings → "Set as default assistant" (system dialog; declining changes nothing).
- **F25 — Onboarding.** First-run walkthrough, every permission explained and skippable one by one.
- **F26 — Code Black.** Say "code black" (or ⋮ → Code Black) to lock the phone immediately.
- **F27 — Calendar reminders.** ⋮ → Calendar: today's events (read-only); optional 15-min-before announcements (OFF by default).
- **F28 — Email triage.** ⋮ → Email triage: recent unread Gmail, read-only. **Disabled until connected** — paste a free OAuth access token (setup steps shown in the app; token lasts ~1 h).
- **F29 — Finance.** "Spent 500 on groceries" logs it; ⋮ → Finance shows totals by category. Optional SMS bank-message parsing, OFF by default.
- **F30 — Network audit.** ⋮ → Network: your Wi-Fi name, IP, and devices on the network (display only — no scanning).
- **F31 — Telemetry.** ⋮ → Telemetry: battery/charging + top apps today (needs Usage access).

**Actions**
- **F32 — Air gestures.** Wave a hand over the top of the phone to silence Jarvis (toggle OFF by default; needs a working proximity sensor — untested on this phone).
- **F33 — Macros.** "Record macro morning routine" → say commands → "stop recording" → "run morning routine".
- **F34 — Message drafting.** "Message Ami that I'll be late" shows the draft and opens WhatsApp/SMS prefilled — **you press send; nothing is ever auto-sent**.
- **F35 — Form drafting.** "Help me fill a leave application" — Jarvis asks each field, then shows the finished draft (never submitted anywhere).
- **F36 — Multi-task queue.** "Do these three things: …" runs them in order with an honest per-step report.
- **F37 — Commute.** "How long to Faisalabad Medical University?" — free routing + ETA (no live traffic, no key).
- **F38 — Local worker.** Simple questions (time, date) and tool requests are answered directly on the phone without a Gemini round trip.
- **F39 — Shizuku controls.** Optional, OFF by default; needs the free Shizuku app + wireless debugging (force-stop app, mobile data, etc. — allowlist only).
- **F40 — Agent loop.** Multi-step requests are planned, executed step by step, verified honestly, and reported as "Done: … Could not do: … because …" — never claimed done when failed.
- **F41 — Accessibility service.** Optional (OFF by default): voice "tap Send" / "type …" / "go back" / "scroll down" in other apps; every action is logged on screen (⋮ → Accessibility actions). Needs enabling in Android Settings → Accessibility.
- **F42 — Do-the-task actions.** Messages, calls and alarms now ask for confirmation first: Jarvis shows the exact message/number and waits for "yes" (or the on-screen button) — 15 s of silence cancels. Alarms open the Clock app for you to save.

**Repo & CI**
- **F2 — Repo hygiene.** Real source tree in the repo, real README, MIT LICENSE, tag v1.0.8.
- **F3 — CI hardening.** `flutter analyze` + `flutter test` must pass — no more `continue-on-error`.
- **F4 — Dead emulator workflow removed.**
- **F5 — Release signing.** Signed APKs via GitHub Secrets (keystore never in the repo).
- **F6 — Release workflow.** Pushing a `v*` tag builds the signed APK and publishes the GitHub Release.

## Skipped (per the approved plan — not built, not asked again)
- **F15 — Porcupine wake word.** Not needed: openWakeWord (F14) passed verification.
- **S1–S12:** Termux worker, live Google traffic (paid key), Hermes self-evolving engine, ElevenLabs, Fish Audio, on-device LLM llama.cpp (~400 MB), offline whisper.cpp (~75 MB), Vosk (~40 MB), Claude/OpenAI (paid), MCP client, OCR/Screen Vision, cross-app form autofill.

## Known limits (honest list)
- The APK is ~25 MB bigger than v1.0.8 (on-device wake-word libraries). One download only — this is the single release.
- The Workmanager reminder backstop delay is a *minimum* — under Doze it can arrive a few minutes late; while the app is alive the timer fires exactly on time.
- Gmail triage needs a fresh pasted access token about every hour (free OAuth Playground route shown in the app).
- Air gestures (F32) are unverified on the Infinix Smart 9 — report whether the wave works.
- Shizuku (F39) and Accessibility (F41) need their own setup steps in the app; both are off by default.
- Re-importing the same bank SMS can duplicate a finance entry (F29).

## Verification before release
- `flutter analyze`: 0 errors (36 info/warning notes, all pre-existing patterns).
- `flutter test`: 229/229 pass.
- Native Kotlin changes (new channels, Accessibility service, Shizuku provider, overlay plugin, tflite JVM pin) are compile-checked by CI, not locally — the first green CI run is the gate.
- No API key anywhere in code or repo. No copied code, images, logos, audio or keys. Default model `gemini-3.8-flash` only.

## Phone-test list (after the single download)
1. STOP mid-answer and mid-wait ("stop" / "chup" / STOP button) — must go silent and stay silent.
2. Tick sound gone while listening in the background.
3. "Hey Jarvis" in airplane mode (wake-up works offline; the answer needs internet).
4. Barge-in mid-answer; 8 s follow-up window.
5. Reminder: set one, swipe the app away — notification must fire; restart the phone — it must still fire.
6. Edge TTS voice (natural UK voice) vs phone voice; streaming (text appears in ~2 s).
7. "Remember that my exam is on 5 December" → ask later "what do you remember about me".
8. "Message Ami that I'll be late" → say "no" (nothing opens), then "yes" (opens prefilled — you press send).
9. "Call Ami" → confirm the name/number, say "yes" (dialer opens — you tap Call).
10. "Set alarm for 7 am" → Clock app opens showing the alarm; you save it there.
11. Floating bubble, Code Black, calendar, finance ("spent 500 on groceries"), commute ("how long to Faisalabad Medical University?"), "what time is it?" (instant, works in airplane mode).
