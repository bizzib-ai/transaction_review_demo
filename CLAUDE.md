# Project: transaction-review-demo

BizziB AI public demo. Interactive web dashboard for reviewing and categorizing financial transactions across multiple accounts for small business tax prep. Serverless backend, three-stage Python data pipeline, React dashboard.

## Project-specific conventions

(Things that differ from the global CLAUDE.md or add to it.)

- **This is public and a marketing artifact** for bizzib.ai. It is the anonymized twin of the private `../ehas_transactions`. Data here must stay synthetic — never copy real client transactions, names, or amounts in from the private project.
- **Deploy only via `git push`.** Netlify auto-builds from the push; do NOT use Windsurf's deploy tool or `netlify` CLI manual deploys (they spin up a separate site).
- `src/data.json` is **anonymized generated sample data** (~1,165 fake transactions); regenerate it via the data-generation script rather than hand-editing.
- Stack: React 19 + Vite + TailwindCSS 4, Recharts, Lucide. The whole dashboard is a single-file app in `src/App.jsx`. State is React Context + localStorage, with edits debounced (~3s) to the server.
- `npm run dev` serves the UI but **not** the serverless functions — analytics/sync/stats only work under `netlify dev`.
- Server state lives in Netlify Blobs via three functions: `analytics.mjs` (sessions/events/signups/identities), `sync.mjs` (per-visitor edits), `stats.mjs` (cached aggregate stats). Respect the existing blob store/key layout and the record caps when touching them.
- Pull analytics/visitor edits with `python scripts/pull_demo_analytics.py` (writes to `data/exports/`, gitignored).
- Identity is demo-only: viewing is open, interaction prompts for name+email. Keep that gating behavior; don't lock down viewing.

## Project structure

- `src/App.jsx` -- the entire dashboard (single-file React app). `src/main.jsx`, `src/index.css` (Tailwind entry), `src/data.json` (generated sample data).
- `netlify/functions/` -- `analytics.mjs`, `sync.mjs`, `stats.mjs` (serverless backend over Netlify Blobs).
- `scripts/pull_demo_analytics.py` -- admin pull of analytics + visitor edits.
- `docs/` -- `CHANGELOG.md` and `blog/` write-ups.
- `netlify.toml`, `vite.config.js`, `eslint.config.js`, `index.html` -- build/config.
- `data/exports/` -- local analytics archives (gitignored).

## What not to touch

- `src/data.json` -- generated sample data; regenerate, don't hand-edit.
- `data/exports/` -- gitignored local pulls.

## Things Claude has gotten wrong before

-
