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

## Survey links

The "Take the survey" CTA links out to the existing bilingual Google Form (no embed):

- EN: https://docs.google.com/forms/d/1SBcMhcV90_8P2jntN9Fs37edHEshFhHHnDwxC5ZY90M/viewform
- RO: https://docs.google.com/forms/d/172B3AGhx25FZPJuLq6e_XJKgx-G5WZOjOlhvycqdevU/viewform

Name, email and platform are collected only inside the Google Form's own beta-invite page, which appears (and is required) only when the respondent answers yes to testing the app — there is no separate signup form or backend on this page.

## Analytics

Not wired up yet. Sign up for [Plausible](https://plausible.io) or [GoatCounter](https://goatcounter.com) and paste the tracking snippet into the `<head>` of `index.html` where marked with a `TODO analytics` comment. Consider firing a custom event on click of `[data-survey-link]` elements to track survey click-through.

## Deployment

Hosted via GitHub Pages from `main` on [github.com/wfettich/secondsight-landing](https://github.com/wfettich/secondsight-landing), live at https://wfettich.github.io/secondsight-landing/. Pushing to `main` redeploys automatically.
