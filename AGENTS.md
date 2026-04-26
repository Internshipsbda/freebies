# AGENTS.md

This document describes the project architecture, conventions, and key decisions for AI agents working on this codebase in future sessions.

## Project Purpose

An interactive FRTF (Future Ready Talent Framework) Competency Self-Assessment tool. Users rate themselves across 12 competencies on a 1–5 scale and receive a gap report categorising their skills into Strengths (4–5), Emerging Skills (3), and Priority Gaps (1–2). Responses are persisted via Netlify Forms.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Framework | TanStack Start |
| Frontend | React 19, TanStack Router v1 |
| Build | Vite 7 |
| Styling | Tailwind CSS 4 |
| Forms | Netlify Forms |
| Language | TypeScript 5.7 (strict) |
| Deployment | Netlify |

## Directory Structure

```
public/
  form-survey.html      # Hidden HTML form for Netlify Forms build detection (MUST match component form name)
  favicon.ico
src/
  components/
    SurveyForm.tsx      # Core assessment UI: rating inputs, gap report, form submission
  routes/
    __root.tsx          # HTML shell, meta tags, global styles
    index.tsx           # Home page, renders SurveyForm centred on screen
  router.tsx            # TanStack Router instance
  styles.css            # Tailwind import + base styles
netlify.toml            # Build config (vite build → dist/client), dev server port 8888
vite.config.ts          # Vite + TanStack Start + Tailwind + Netlify plugin
```

## Key Architecture Decisions

### Single-page assessment
All 12 competencies render on one page grouped by cluster. There is no multi-step wizard. This keeps the code simple and lets users scroll back to revise ratings.

### Netlify Forms
Form submissions go directly to Netlify's serverless form handling—no backend API route needed. The `public/form-survey.html` file contains a hidden duplicate of the form so Netlify's build bot can detect the form definition at build time. **This file must stay in sync with the form's `name` attribute and field names in `SurveyForm.tsx`.**

- Form name: `frtf-self-assessment`
- Fields: `name`, `total-score`, one field per competency ID (kebab-case), `bot-field` (honeypot)

### Gap report
Rendered by the `GapReport` component inside `SurveyForm.tsx`. It accepts a `ratings` object and computes categories client-side. It is shown both as a preview (before submit) and as the post-submission result.

### Competency data
All 12 competencies are defined in the `competencies` array at the top of `SurveyForm.tsx`. Each entry carries an `id` (kebab-case, doubles as the Netlify Forms field name), `clusterNum`, `name`, and `description`.

## Naming Conventions

- Components: PascalCase (`SurveyForm`, `GapReport`, `RatingInput`)
- Competency IDs: kebab-case (must match form field names in `public/form-survey.html`)
- Routes: kebab-case files under `src/routes/`
- TypeScript: strict mode, `@/` alias for `src/`

## Gotchas

- Adding a new competency requires updating **both** `SurveyForm.tsx` (the `competencies` array) and the hidden form in `public/form-survey.html`.
- Do not run `npm run build` or `netlify build` manually—the deploy system handles builds automatically.
- Data persistence uses Netlify Forms only; there is no database or external API.
