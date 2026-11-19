# Git and repositories

State as of 2026-11-18. This file only describes the setup; nothing about it needs changing.

## Repositories
| Repo | Visibility | Last push | Notes |
|---|---|---|---|
| `drinkryeziu/lundro` | Private | 2026-10-16 21:48 UTC | The README and `index.html` link here |
| `drinkryeziu/lundro_new` | Public | 2026-10-16 21:55 UTC | Reviewed for this snapshot |

Both were pushed within a few minutes of each other on the same day. `lundro_new` looks like a public re-publish of `lundro`. Confirm which repo is the main one, and whether your local folder (`~/Claude/Lundro`) has work that hasn't been pushed (`git status`, `git log origin/main..`).

## `lundro_new`
- Branch: `main` only (default `origin/HEAD -> main`)
- History: 1 commit, `d08195e`, "Lundro: boat rental platform (Next.js app, database schema, docs and clickable prototype)", 2026-10-16
- No tags or releases

## Ignored files (`.gitignore`)
`node_modules`, `.next`, `.env`, `.env*.local`, `.vercel`, `*.log`, `.DS_Store`, `*.tsbuildinfo`.
Only `.env.example` is committed. Real secrets stay in `.env` (local) or in host env vars.

## Automation
| Workflow | Trigger | What it does | State |
|---|---|---|---|
| `.github/workflows/ci.yml` | push to `main`, PRs | Postgres 16 service → `npm ci` → migrate → seed → test → `tsc` → build | YAML is invalid, so it won't run (see [STATUS.md](../STATUS.md)) |
| `.github/workflows/pages.yml` | push to `main`, manual | Builds `_site/` from `index.html`, `prototypes/`, `public/brand`, `public/prototype` and deploys to GitHub Pages | Needs Pages turned on (Settings → Pages → Source: GitHub Actions) |

## Workflow
See [CONTRIBUTING.md](../CONTRIBUTING.md): branch from `main`, open a PR, CI must pass. Pricing or tax changes must update `lib/pricing.test.ts`.
