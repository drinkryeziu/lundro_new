# Project status

Snapshot of what is built in Lundro, as of 2026-11-18 (commit `d08195e` on `main`).

## Summary
Lundro is a boat rental marketplace for NC lakes, starting with Lake Norman. All 19 screens exist as Next.js pages. The renter booking flow and owner signup use a real Postgres database through Prisma. The owner dashboard and admin screens are still UI only, with sample data.

## Stack
| Part | Version / tool |
|---|---|
| Framework | Next.js **16.4.2** (App Router, Turbopack), React 19 |
| Styling | Tailwind 4 (no preflight reset) plus the design's inline styles per page |
| Database | PostgreSQL 16 with Prisma 6 (one migration: `20261009012435_init`) |
| Auth | Signed HttpOnly cookie `lundro_session` (14 days), `AUTH_SECRET` required in production |
| Tests | `node --test`: 8 tests in `lib/pricing.test.ts` and `lib/auth.test.ts` |
| Hosting | Vercel (`vercel.json`), Docker (`Dockerfile`, `docker-compose.yml`), GitHub Pages for the prototype |

## Screens
| Screen | Route | Status |
|---|---|---|
| Home | `/` | UI, static content |
| Lake Norman | `/lake-norman` | UI, static content |
| Search | `/search` | **Wired**: `GET /api/boats` |
| Boat detail | `/boat?id=` | **Wired**: `GET /api/boats/:id` |
| Checkout | `/checkout?boat=` | **Wired**: `POST /api/bookings` (price computed on the server) |
| Confirmation | `/confirmation?code=` | **Wired**: `GET /api/bookings/:code` |
| Log in | `/login` | **Wired**: `/api/auth/*` |
| Owner signup | `/owner/signup` | **Wired**: `POST /api/owner/signup` |
| Owner overview | `/owner` | UI only, sample data |
| Owner calendar | `/owner/calendar` | UI only |
| Owner bookings | `/owner/bookings` | UI only (`GET /api/owner/bookings` is ready but not used yet) |
| Owner boats | `/owner/boats` | UI only |
| Owner extras | `/owner/extras` | UI only |
| Owner customers | `/owner/customers` | UI only |
| Owner messages | `/owner/messages` | UI only |
| Owner earnings | `/owner/earnings` | UI only |
| Owner reviews | `/owner/reviews` | UI only |
| Owner verification | `/owner/verification` | UI only |
| Owner settings | `/owner/settings` | UI only |
| Admin review | `/admin/review` | UI only (`GET/POST /api/admin/owners` is ready but not used yet) |

## Backend
- 11 API route files under `app/api` (auth, boats, bookings, owner, admin). See [docs/API.md](docs/API.md).
- Database models: User, Owner, Document, Lake, Boat, BoatPhoto, Extra, BoatExtra, CalendarBlock, Booking, BookingExtra, Review, Conversation, ConversationParticipant, Message, Payout. See [docs/DATABASE.md](docs/DATABASE.md).
- `lib/pricing.ts` is the only place boat, extras, captain, service fee and State/County/Transit tax are calculated.
- The seed creates demo accounts (`admin@`, `owner@`, `renter@lundro.test`, password `lundro-demo-1`).

## Prototypes and design
- `prototypes/lundro-prototype.html`: the whole product in one clickable HTML file. A copy is served at `/prototype` (`public/prototype/index.html`).
- `prototypes/platform-map.html`: overview of the architecture, screens, data model, API and roadmap.
- `design-source/`: 19 original `.dc.html` canvas files, kept for reference only.
- `site/`: a separate static copy of the landing page and platform map.

## Checks (run 2026-11-18)
| Check | Result |
|---|---|
| `npm test` | Pass, 8/8 |
| `npx tsc --noEmit` / `npm run build` | Not verified: the sandbox could not download Prisma's engine binaries, so `prisma generate` did not run. The type errors seen there (`@prisma/client` has no `Boat` export, plus implicit-`any` errors that follow from it) come from the missing generated client. Re-run locally after `npm install`. |
| CI workflow | **Broken YAML** (see Known issues) |

## Known issues
1. **`.github/workflows/ci.yml` does not parse.** The line `AUTH_SECRET: ci-secret` is at the wrong indent inside the `postgres` service's `env:`. GitHub will not run the CI job until it is fixed. It should move to the job-level `env:` block, next to `DATABASE_URL`.
2. **The docs are out of date in places:**
   - `docs/ARCHITECTURE.md` and `docs/DEPLOYMENT.md` say Next.js 15. `package.json` pins 16.4.2.
   - `docs/ARCHITECTURE.md` says the data is mock and client-side only. The database, auth and booking API are now built.
   - `docs/DEPLOYMENT.md` says "No env vars needed" for Vercel. `DATABASE_URL`, `DIRECT_URL` and `AUTH_SECRET` are now required (see [docs/ENVIRONMENT.md](docs/ENVIRONMENT.md)).
3. **The prototype links point at `drinkryeziu/lundro`, which is private.** GitHub Pages on a free plan and htmlpreview.github.io only work for public repos. The public copy is `drinkryeziu/lundro_new`. See [docs/GIT.md](docs/GIT.md).
4. **The prototype exists in two copies:** `prototypes/lundro-prototype.html` and `public/prototype/index.html`. They have to be updated together.
5. **The demo seed accounts** have a known password. Remove or change them before launch.

## Not built yet
- Payments (Stripe) and payouts
- Email notifications, password reset
- Photo and document uploads
- Connecting the owner dashboard (11 screens) and the admin review screen to the API
- A messaging backend (the tables exist, but there are no routes)
- Moving the per-page inline styles and plain-JS page logic to Tailwind components
