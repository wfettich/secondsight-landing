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

Language is auto-detected from `navigator.language` (Romanian if it starts with `ro`, otherwise English), overridable via the EN/RO toggle in the header. The choice is remembered per-browser in `localStorage`.

## Survey links

The "Take the survey" CTA links out to the existing bilingual Google Form (no embed):

- EN: https://docs.google.com/forms/d/1SBcMhcV90_8P2jntN9Fs37edHEshFhHHnDwxC5ZY90M/viewform
- RO: https://docs.google.com/forms/d/172B3AGhx25FZPJuLq6e_XJKgx-G5WZOjOlhvycqdevU/viewform

Email is collected only inside the Google Form's own optional beta-invite question — there is no separate signup form or backend on this page.

## Analytics

Not wired up yet. Sign up for [Plausible](https://plausible.io) or [GoatCounter](https://goatcounter.com) and paste the tracking snippet into the `<head>` of `index.html` where marked with a `TODO analytics` comment. Consider firing a custom event on click of `[data-survey-link]` elements to track survey click-through.

## Deployment

Intended to be hosted via GitHub Pages from `main` on a dedicated repo (not yet pushed — this is a local git repo pending your go-ahead to create the remote).
