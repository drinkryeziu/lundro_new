# Environment variables

Copy `.env.example` to `.env` for local work. Never commit `.env`.

| Variable | Required | Used by | Local value | Production |
|---|---|---|---|---|
| `DATABASE_URL` | Yes | Prisma client (app runtime) | `postgresql://lundro:lundro@localhost:5432/lundro?schema=public` | Supabase **transaction pooler** (port 6543) + `?pgbouncer=true&connection_limit=1` |
| `DIRECT_URL` | Yes | Prisma migrations | Same as `DATABASE_URL` | Supabase **direct connection** (port 5432) |
| `AUTH_SECRET` | Yes in production | `lib/auth.ts` signs the `lundro_session` cookie | Any string (a dev fallback is used if missing) | Long random string. The app throws on start in production if it is missing. Generate one with `openssl rand -base64 48` |

## Where to set them
- **Local:** `.env` (Postgres from `docker compose up -d db`)
- **Vercel:** Project → Settings → Environment Variables (Production and Preview)
- **Docker:** `docker run --env-file .env -p 3000:3000 lundro`
- **CI:** job-level `env:` in `.github/workflows/ci.yml`

## Coming later
Stripe, email and file-upload keys will be added here when those features are built.
