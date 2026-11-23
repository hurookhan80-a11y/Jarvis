# Jarvis v1.2.0 — Release Notes

**Version 1.2.0 (versionCode 11). One APK, one download.**
Built 23 Nov 2026. Debug-signed (release signing is still deferred —
the 4 secrets were never hand-entered in GitHub), exactly like v1.1.0:
**uninstall v1.1.0 once, then install v1.2.0 and re-enter the Gemini
key.** App data is lost on uninstall — re-paste the Google / Notion
tokens too if those connectors are used.

## What is new — 14 free Jarvis 2.0 ideas (G1–G13 + G15)

All ideas/patterns only, fresh Flutter code. Nothing was copied from
the course repo (its free starter is empty placeholders anyway, no
LICENSE = all rights reserved). **G14 cloud backup/sync via Supabase
was EXCLUDED by Habib's choice on 23 Nov 2026** — no Supabase code,
no backup/sync UI in this release.

- **G1 — Google Docs / Sheets / Drive by voice.** "Create a doc
  titled X", "create a sheet titled X", "search drive for X". Also in
  the ⋮ menu → Google Workspace (create doc/sheet, search Drive).
  Needs your own Google OAuth access token, pasted in Settings.
- **G2 — Calendar events by voice.** "Create a calendar event team
  meeting tomorrow at 3 pm". With a Google token it goes to Google
  Calendar; without one it opens your on-device calendar prefilled
  and you save it there. Both paths say where the event went.
- **G3 — Email draft + send by voice.** "Draft an email to …" saves
  a Gmail draft (never sends). "Send an email to …" goes through the
  confirmation gate — Jarvis shows the exact recipient, subject and
  body and waits for a yes. Nothing is sent without it.
- **G4 — Workflow templates.** Named multi-step routines with a live
  step log (⋮ menu → Workflows, or "run X workflow"). Four built in:
  morning briefing, research and save, email production, make a post
  from this file. Add your own in the Workflows screen.
- **G5 — Scheduled workflows.** "Schedule morning briefing workflow
  at 7 am" (add "on weekdays" / "every day" / "on monday"). While
  Jarvis is open, a due workflow runs (or asks first, per its
  autonomy). When the app is closed, a due workflow only sends a
  notification — open Jarvis to run it. Background isolates cannot
  run the Gemini agent loop.
- **G6 — Specialist agent modes.** "Research mode", "writer mode",
  "publisher mode", "general mode" (or pick one in Settings). Each
  mode has its own prompt: Research searches the web first and cites
  sources; Writer answers in your voice and offers to save; Publisher
  saves to Docs/Sheets/Calendar and reports exactly where.
- **G7 — Web research.** "Search the web for X" / "research X".
  Keyless Wikipedia + DuckDuckGo, no account, no key. Results are
  spoken short and shown in full on screen.
- **G8 — Attach a file / image in a task.** "Attach a file" / "attach
  an image", then say the task. Images go to Gemini vision; text
  files go to Gemini as context. Other file types say so plainly.
  10 MB limit.
- **G9 — Notion read / write.** "Search notion for X", "read notion
  page …", "add … to notion page …". Also in ⋮ menu → Notion. Needs
  your own free Notion integration token, pasted in Settings.
- **G10 — Assistant personality + custom instructions.** Name, tone
  (short / detailed / friendly / formal) and free-text instructions
  in Settings; "be brief" / "be detailed" / "be friendly" /
  "be formal" by voice too.
- **G11 — Usage dashboard.** ⋮ menu → Usage dashboard (or "show
  usage"). On-device counts only: conversations, messages spoken,
  reminders fired, tasks done / failed — all-time plus the last
  7 days. Nothing leaves the phone. A Clear button wipes it.
- **G12 — Per-workflow autonomy.** "Make workflow X autonomous" /
  "make workflow X ask first", or tap a workflow in the Workflows
  screen to flip it. Ask-first shows the plan and waits for a yes;
  autonomous runs straight away. The confirmation gate still applies
  to any send/call step either way.
- **G13 — Agent retry with diagnosis.** When an agent-loop step
  fails, Jarvis asks Gemini for a corrected command, retries once,
  and reports honestly if it still fails. No silent fixes, no
  invented success.
- **G15 — Dark / light mode.** Settings → Theme, or "light mode" /
  "dark mode" by voice. The main console background, chat box, app
  bar and status text follow the theme; the orb and circuit art keep
  their dark-console look in both modes, and the older screens keep
  their dark styling.

## Tokens (your own, pasted in Settings — never in code)

- **Google OAuth access token** (Docs/Sheets/Drive/Calendar/Gmail):
  get one from the Google OAuth Playground with the scopes
  documents, spreadsheets, drive.file, calendar.events,
  gmail.compose. **It lasts about an hour** — paste a fresh one when
  a Google command starts failing with an expiry message.
- **Notion integration token**: create a free integration at
  notion.so/my-integrations and share the pages with it.
- Both connectors ship **disabled** until you paste a token and
  switch them on (same pattern as the Gmail triage in v1.1.0).

## Phone tests (new in v1.2.0)

1. "Search the web for black holes" — results on screen, short spoken summary.
2. "Research mode" then ask a question — the answer cites sources.
3. "Run research and save workflow about diabetes" — asks for the topic if missing, asks first, then runs the steps.
4. "Schedule morning briefing workflow at 7 am" — check it appears in ⋮ → Workflows → Schedules.
5. Settings → Theme → Light — the console background and chat box go light; back to Dark.
6. ⋮ → Usage dashboard — counts grow after a few chats.
7. "Attach an image" → pick a photo → "what is in this picture?" — Gemini vision answers.
8. Google token pasted in Settings → "create a doc titled test" — doc appears in Drive.
9. "Send an email to …" — Jarvis shows the exact email and waits for yes; say no and nothing is sent.

## Known limitations (honest)

- A scheduled workflow fires only a notification while the app is
  closed; it runs when you open Jarvis.
- Two workflows due in the same minute: the second one's ask-first
  prompt replaces the first's — the first stays unrun until you run
  it by hand.
- Light mode recolours the main console and all new screens; the
  older v1.1.0 screens keep their dark styling.
- The Google token expiry (~1 hour) and the Notion token flow are
  untestable in unit tests (same limitation as the F28 Gmail token
  flow) — verified on-device only.
- A weekday event whose time already passed ("monday at 9 am" said
  on Monday at 10 am) rolls to next week; an unnamed past time
  ("at 9 am") rolls to tomorrow.

## Rollback

If v1.2.0 misbehaves on the phone, reinstall v1.1.0 from
`~/workspace/your_files/jarvis-ci-armv7-v1.1.0.apk` (uninstall +
re-enter the Gemini key). v1.1.0 keeps working as before.
