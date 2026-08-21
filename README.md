# HootieHoo

A personal check-in shortcut: one home-screen icon, two pre-written messages, sent to whichever people in your group are currently turned on.

Tap the "HootieHoo" button before you head out; tap "Resume Life" when you're back. Each tap opens your phone's Messages (or Mail) app with everyone enabled already filled in, so getting the word out is a two-tap job instead of typing it out each time.

## What it does

- **Send** — two big buttons, one per message. Whichever you're expected to send next is highlighted "Next," but either is always tappable — useful if you need to re-send or skip straight to the all-clear. A running local activity log shows what was opened and when (it can't confirm the message actually went out — see below).
- **Members** — add people with a phone number and/or email, and flip any one of them off without deleting them — handy for "not this trip" without losing their info.
- **Schedule** — optionally restrict the buttons to certain days and a time window (e.g. only during daylight hours). Outside that window the Send tab shows a lock, but a "send anyway" override is always one tap away — a schedule is a nudge, not a lockout.
- **Settings** — edit both message texts and button labels, and choose whether messages go out as a group text (SMS) or email (bcc). Email is a useful fallback for a contact without a phone number, or when group SMS gets flaky across a mix of iPhone and Android.

## Why it's not fully automatic

No phone or browser lets a website send a text message silently — that's an OS-level anti-spam/consent protection on both iOS and Android, not a limitation of this app, and there's no way around it from a page like this. What HootieHoo does instead is pre-fill everything (recipients, message body) and hand you straight to the native Messages/Mail app so all that's left is your own final tap to send.

Truly zero-tap, fire-and-forget sending is possible, but only via a paid SMS API (e.g. Twilio) with a real backend holding your credentials — a materially bigger project (server, per-message cost, phone number provisioning) than a single-page home-screen app. Worth doing as a v2 if the extra tap ever becomes the actual blocker.

## Data and privacy

Everything — members, messages, schedule, activity log — is stored **only in your browser's local storage** on the device you open it on. There's no account and no sync between devices. Nothing is sent anywhere except the message itself, and only once you tap Send in the app HootieHoo hands you off to.

Clearing your browser's site data for this page erases everything. There's no backup/export yet — worth adding if you end up relying on this.

## Running it

It's a single self-contained `index.html` — no build step, no dependencies.

**Locally:** open `index.html` directly in a browser, or serve the folder (e.g. `python3 -m http.server`) and visit it.

**On your phone, via GitHub Pages:**
1. On GitHub: **Settings → Pages → Build and deployment → Source: Deploy from a branch.**
2. Branch: `main`, folder `/ (root)`. Save.
3. After it deploys (~1–2 min), the app is at `https://rakh90.github.io/HootieHoo/`.
4. On your phone, open that URL and use "Add to Home Screen" for an app-like icon.

Note: GitHub Pages sites are public to anyone with the link, same as this repo. Your group members' names, phone numbers, and emails live only in your browser's local storage — never in the repo or on the Pages site — so hosting the app publicly doesn't expose that data. Just don't share the link somewhere your typed-in message text (visible in Settings if someone opens the page on your phone) would be a problem.

## Status

v1, personal use only: single user, no accounts, no sharing of the member list between devices.
