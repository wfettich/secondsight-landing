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

## Beta signup (primary call to action)

The hero button scrolls to `#signup`, a small form: email (required) + platform (iPhone/iPad or Android), a hidden honeypot field, and a consent line. Submit does a plain `fetch` POST to the Firestore REST API (no SDK, no backend) creating a document in project `secondsight-29914`, collection `betaSignups`, with fields `email`, `platform` (`ios`|`android`), `lang` (`en`|`ro`), `createdAt` (client ISO timestamp), `source` (`landing`). The Web API key in `index.html` (`SIGNUP_ENDPOINT`) is the public key from the Android `google-services.json`; it is public by design, so restrict it in Google Cloud console (APIs & Services > Credentials) to this site's referrer and the Firestore API. A localStorage flag stops the same browser re-sending the same email+platform; a different email always goes through.

Security is enforced by `../Camera/firestore.rules` (`match /betaSignups/{id}`): unauthenticated `create` only, exact key set, email string <= 254 chars containing `@`, platform/lang whitelists, `source == 'landing'`; no read/list/update/delete. **The rules must be deployed (`firebase deploy --only firestore:rules`) before the form works**, otherwise submissions get a 403 and the page shows the error message.

### Owner workflow: from signup to invite

Signups are not emailed anywhere and the page never invites anyone. To process them:

1. Firebase console > Firestore Database > `betaSignups` (export via console, or `gcloud firestore export`, or the Admin SDK).
2. Add the emails to testers: Firebase console > App Distribution > Testers & Groups (add to a group, e.g. `beta-ios` / `beta-android`), or CLI: `firebase appdistribution:testers:add --project secondsight-29914 a@x.com b@y.com`, and add them to the release's group.
3. Platform difference: **iOS ad-hoc distribution requires each tester's device UDID** (registered in the provisioning profile; App Distribution can collect it from the tester), Android needs nothing extra.

## Survey (optional)

The bottom section links out to the existing bilingual Google Form (no embed) as an optional "help us shape SecondSight" survey. It no longer handles signup; the form's "testing the app" question is being removed.

- EN: https://docs.google.com/forms/d/1SBcMhcV90_8P2jntN9Fs37edHEshFhHHnDwxC5ZY90M/viewform
- RO: https://docs.google.com/forms/d/172B3AGhx25FZPJuLq6e_XJKgx-G5WZOjOlhvycqdevU/viewform

## Analytics

Not wired up yet. Sign up for [Plausible](https://plausible.io) or [GoatCounter](https://goatcounter.com) and paste the tracking snippet into the `<head>` of `index.html` where marked with a `TODO analytics` comment. Consider custom events for clicks on `[data-survey-link]` elements and for successful signups.

## Deployment

Hosted via GitHub Pages from `main` on [github.com/wfettich/secondsight-landing](https://github.com/wfettich/secondsight-landing), live at https://wfettich.github.io/secondsight-landing/. Pushing to `main` redeploys automatically.
