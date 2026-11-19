# Changelog

## [0.1.0] - 2026-10-16
First commit (`d08195e`).

### Added
- Next.js 16 app with 19 screens: renter (home, Lake Norman, search, boat, checkout, confirmation, login), owner signup plus an 11-screen owner dashboard, and admin review.
- PostgreSQL schema (Prisma), the first migration, and seed data with demo accounts.
- Auth API (register, login, logout, me) with a signed session cookie.
- Boats, bookings (prices calculated on the server), owner signup, owner bookings and admin owner-approval APIs.
- `lib/pricing.ts` for prices and the State/County/Transit tax split, with tests.
- Clickable HTML prototype, platform map, brand assets and the original design files.
- Vercel config, Dockerfile, docker-compose for Postgres, CI and GitHub Pages workflows.
- Docs: README, CONTRIBUTING, API, ARCHITECTURE, DATABASE, DEPLOYMENT, SUPABASE.
