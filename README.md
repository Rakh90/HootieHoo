# HootieHoo

A personal check-in shortcut: one home-screen icon, two pre-written messages, sent to whichever people in your group are currently turned on.

Tap the red "HootieHoo" button before you head out; tap the green "Nesting" button when you're back. Each tap opens your phone's Messages (or Mail) app with everyone enabled already filled in, so getting the word out is a two-tap job instead of typing it out each time. A smaller blue "CaCaw" button below those two is for the rare emergency case — see below for why it behaves differently.

## What it does

- **Send (the dashboard)** — a dark, illuminated-button control panel: a red circular button and a green circular all-clear button, with fully editable titles and text (Settings). Whichever you're expected to send next pulses gently, but either is always tappable — useful if you need to re-send or skip straight to the all-clear. A running local activity log shows what was opened and when (it can't confirm the message actually went out — see below).
- **Emergency button** — a smaller blue circle below the main two, set apart with its own "Emergency" label since it's meant to be used rarely. Unlike the other two, it needs **two taps**: the first arms it (it'll pulse and show "Tap again," with about 4 seconds to confirm before it quietly disarms itself), the second actually sends. That's there on purpose — a message meant for genuine emergencies shouldn't be one accidental tap away. Fully editable in Settings like the other two.
- **System armed / standby toggle** — a master switch at the top of the dashboard. Flip it to Standby to dim both buttons and block taps entirely, without touching your members or messages — a quick "off" for whenever you don't want this thing live.
- **Members** — add people with a phone number and/or email, and flip any one of them off without deleting them — handy for "not this trip" without losing their info. On browsers that support it (currently Chrome on Android), an **Add from Contacts** button lets you pick people straight from your phone's contact list instead of typing them in — see below for where this does and doesn't work.
- **Settings** — edit both message texts and button labels, and choose whether messages go out as a group text (SMS) or email (bcc). Email is a useful fallback for a contact without a phone number, or when group SMS gets flaky across a mix of iPhone and Android.

There's intentionally no schedule/quiet-hours feature: it could only ever gate *your* phone, not your recipients'. HootieHoo doesn't hold or queue messages — the moment you tap Send, the native Messages/Mail app sends it immediately, and it lands on every recipient's phone right away, same as any other text. There's no mechanism for a website to mute or delay delivery on someone else's phone, so a "schedule" here would only have been misleading.

## Adding contacts directly from your phone

The Members tab uses the browser's [Contact Picker API](https://developer.mozilla.org/en-US/docs/Web/API/Contact_Picker_API) when it's available, so you can select people straight from your phone's address book instead of typing each name/number/email by hand. As of now, browser support is narrow and worth knowing before you rely on it:

- **Works today:** Chrome (and Chromium-based browsers like Edge, Samsung Internet) on **Android**.
- **Doesn't work:** Safari on iPhone/iPad, desktop browsers generally. Apple hasn't shipped this API in Safari — some iOS versions have it behind an experimental flag buried in Settings → Safari → Advanced → Feature Flags, but that's not something to rely on or ask your group to go dig up.

Where it's unsupported, the "Add from Contacts" button simply doesn't appear — you get an explanatory note and the regular manual-entry form instead, which always works everywhere. This is a browser platform limitation, not something specific to HootieHoo's code; it'll quietly start working on more devices as/if Apple adds support in the future, with no changes needed here.

## Installing it as a home-screen dashboard

HootieHoo ships with a `manifest.json` and app icon, so "Add to Home Screen" launches it full-screen with no browser address bar — it looks and feels like an installed app, not a bookmark. One real limit worth knowing: like any website or installed web-app, it only runs while you actually have it open. There's no way for a page like this to keep running, listen for anything, or act in the true background while your phone is locked or you're in another app — that would require a native app with OS-level background permissions, a different and much bigger project than a static page. What you get here is an app-like *launch experience*, not background execution.

## How updates reach an already-installed icon

HootieHoo has **no service worker** — that's deliberate. Without one, the home-screen icon isn't a cached offline app shell; tapping it just loads `index.html` fresh from GitHub Pages like any normal webpage. So when this code changes (a new feature, a bug fix, a default tweak), there's nothing to reinstall — the next time you open the app, you're already running the new version.

Two things that update this way *don't* carry over: your own saved data (members, message text, armed state, activity log), which intentionally always wins over the code's defaults so an update can never silently overwrite something you've customized — and your recipients, who never install anything at all; they only ever receive a normal text message, so this whole question doesn't apply to them.

One caveat: right after a push, it can take a minute or two for GitHub Pages to finish redeploying, and your browser may briefly hold a cached copy of the old page. If you open the app right after an update and don't see the change, force-closing and reopening it (or a pull-to-refresh) clears that up.

## Why it's not fully automatic

No phone or browser lets a website send a text message silently — that's an OS-level anti-spam/consent protection on both iOS and Android, not a limitation of this app, and there's no way around it from a page like this. What HootieHoo does instead is pre-fill everything (recipients, message body) and hand you straight to the native Messages/Mail app so all that's left is your own final tap to send.

Truly zero-tap, fire-and-forget sending is possible, but only via a paid SMS API (e.g. Twilio) with a real backend holding your credentials — a materially bigger project (server, per-message cost, phone number provisioning) than a single-page home-screen app. Worth doing as a v2 if the extra tap ever becomes the actual blocker.

## Data and privacy

Everything — members, messages, activity log — is stored **only in your browser's local storage** on the device you open it on. There's no account and no sync between devices. Nothing is sent anywhere except the message itself, and only once you tap Send in the app HootieHoo hands you off to.

Clearing your browser's site data for this page erases everything. There's no backup/export yet — worth adding if you end up relying on this.

Since your saved messages/members always take priority over the code's built-in defaults (so an update never silently overwrites wording you've customized — see below), Settings has a **Reset messages to default** button for when you actually do want to go back to HootieHoo's stock titles/text/send-channel, including the emergency button. It leaves your members list and activity log alone.

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
