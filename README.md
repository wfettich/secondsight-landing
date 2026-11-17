# SecondSight landing page

Single static page (no build step) for gauging interest in SecondSight ahead of a public launch. See the design doc for context: `../Camera/docs/plans/2026-09-28-secondsight-landing-page-design.md`.

## Structure

- `index.html` — everything: markup, CSS, and a small vanilla-JS i18n layer (EN/RO), all inline. No dependencies, no framework.

## Local preview

```bash
python3 -m http.server 8000
# open http://localhost:8000/index.html
```

## Language

Language is set by the `?lang=en` / `?lang=ro` URL parameter (shareable, one URL per language). Without it, it falls back to the choice saved in `localStorage`, then `navigator.language` (Romanian if it starts with `ro`, otherwise English). The EN/RO toggle updates the URL and the saved choice.

## Download section (primary call to action)

The hero button "Get the app" scrolls to `#signup`, the download section. It has no form and collects no data:

- **Android:** a button and QR code for the Firebase App Distribution invite link (group "Public testers", app `com.wf.secondsight`). Visitors join with their Google account; you can see who joined in Firebase console > App Distribution > Testers & Groups.
- **iPhone / iPad:** a button and QR code for the public TestFlight link (`https://testflight.apple.com/join/KQEuW31z`). It is **hidden** (`hidden` on `#dl-ios-item`) until Apple has approved the beta build for external testing; remove the attribute then. Add `?preview=1` to the page address to see it before that.
- The QR codes are static files (`qr-android.svg`, `qr-ios.svg`). If a link changes, regenerate them (for example with the Python `segno` package) and update the `href`.
- The "Why we're asking" text and the optional survey sit directly under the download buttons.

## Survey (optional)

The bottom section links out to the existing bilingual Google Form (no embed) as an optional "help us shape SecondSight" survey. It no longer handles signup; the form's "testing the app" question is being removed.

- EN: https://docs.google.com/forms/d/1SBcMhcV90_8P2jntN9Fs37edHEshFhHHnDwxC5ZY90M/viewform
- RO: https://docs.google.com/forms/d/172B3AGhx25FZPJuLq6e_XJKgx-G5WZOjOlhvycqdevU/viewform

## Analytics

Not wired up yet. Sign up for [Plausible](https://plausible.io) or [GoatCounter](https://goatcounter.com) and paste the tracking snippet into the `<head>` of `index.html` where marked with a `TODO analytics` comment. Consider custom events for clicks on `[data-survey-link]` elements and for successful signups.

## Deployment

Hosted via GitHub Pages from `main` on [github.com/wfettich/secondsight-landing](https://github.com/wfettich/secondsight-landing), live at https://wfettich.github.io/secondsight-landing/. Pushing to `main` redeploys automatically.
