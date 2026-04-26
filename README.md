# FRTF Competency Self-Assessment

An interactive self-assessment tool built on the **Future Ready Talent Framework (FRTF)**. It helps users evaluate their professional readiness across 12 core competencies grouped into 4 clusters, then generates a personalised gap report highlighting strengths, emerging skills, and priority gaps.

## Key Technologies

| Layer | Technology |
|-------|------------|
| Framework | [TanStack Start](https://tanstack.com/start) |
| Frontend | React 19, TanStack Router v1 |
| Build | Vite 7 |
| Styling | Tailwind CSS 4 |
| Forms | Netlify Forms (serverless, no backend needed) |
| Language | TypeScript 5.7 (strict) |
| Deployment | Netlify |

## Features

- Rate 12 FRTF competencies (1–5 scale) across 4 clusters
- Live gap report preview before submitting
- Post-submission gap report: Strengths (4–5), Emerging Skills (3), Priority Gaps (1–2)
- Spam protection via Netlify honeypot field
- All responses saved to Netlify Forms dashboard

## Running Locally

```bash
npm install
npm run dev       # starts dev server at http://localhost:3000
```

To emulate Netlify features (Forms, Edge Functions) locally:

```bash
netlify dev       # starts at http://localhost:8888
```

## Building for Production

```bash
npm run build
```

Output is written to `dist/client/` and served via Netlify's CDN.
