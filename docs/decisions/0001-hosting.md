# 0001. Hosting for the public demo

- **Status:** Proposed (pending the hands-on spike below)
- **Date:** 2026-11-09
- **Context owner:** Sergio Santamaria Nora

## Context

The demo needs a public URL that stays up from the first deploy in Phase 1 until at least mid January, and ideally for as long as it sits in a portfolio. The budget is zero. The stack is a React frontend and a Laravel API with PostgreSQL, a database queue and a nightly reset job.

Constraints that drive the choice:

- No credit card, if possible.
- The database must not expire or be deleted while the demo is in use.
- The API must run as a Docker container.
- Cookie based login (Sanctum) works best when the browser sees a single origin, so the frontend host must be able to proxy `/api` and `/sanctum` to the API.

## Options considered

Figures below come from provider docs where I could open them (Vercel, Neon via its own FAQ, Render via its own article) and from third party comparison pages for the rest. They were read on 2026-11-09 and can change. Re-check before relying on them.

| Option | Role | What it offers | Problem |
|---|---|---|---|
| Vercel Hobby | Frontend | 100 GB fast data transfer, 1M CDN requests, 10 GB fast origin transfer a month. No card needed. | Hobby is for non-commercial, personal use only. A personal portfolio demo fits, client work would not. |
| Render free web service | API container | Docker supported, 750 instance hours a month, 512 MB RAM, no card. | Sleeps after 15 minutes idle and takes about a minute to wake. Render's own docs say not to use free services for production. |
| Render free Postgres | Database | 1 GB, no card. | Expires 30 days after creation, then is deleted after a 14 day grace period. Rejected. |
| Neon free | Database | 1 GB per project, 100 compute hours a month, scales to zero after 5 minutes, resumes in a few hundred milliseconds. | Compute stops if the monthly hours run out. Inactive project retention is not clearly documented. |
| Koyeb free | API container | One free instance, 512 MB RAM. | A third party page reports a card hold of about $29 and that the card-free plan was removed in February 2026. Not verified, but enough to skip it. |
| Fly.io | API container | Trial only (2 machine hours or 7 days). | Card required to keep anything running. Rejected. |
| Railway | API container | $1 of credit a month after a 30 day trial. | Credit may not cover an always-on service. Fallback only. |
| Google Cloud Run | API container | 2M requests, 180,000 vCPU-seconds a month free, scales to zero. | Needs a billing account with a card. Fallback only if the Render cold start is unacceptable. |

## Decision (proposed)

1. **Frontend:** Vercel Hobby. Static, pre-rendered public pages, with rewrites for `/api` and `/sanctum` pointing at the API.
2. **API:** Laravel in a Docker container on a Render free web service.
3. **Database:** Neon free Postgres, connected over SSL.
4. **Queue:** Laravel `database` driver. The worker runs inside the same container as the web process (for example under Supervisor), because the free tier has no separate worker service.
5. **Fallback:** one container serving both the API and the built frontend, if the Vercel proxy turns out to be a problem for cookies.

## Consequences

- The first request after idle will be slow (about a minute) while Render wakes the API. Public pages are served by Vercel and stay fast, so only booking and admin calls are affected. A small "waking up" message in the UI and a warm-up call when the booking page loads should cover most of it.
- A sleeping container cannot run the nightly reset or the queue worker. Options to decide in Phase 3: ping the service from a scheduled GitHub Actions workflow (one always-on service uses about 744 of the 750 free hours, but check Render's terms first), or trigger the reset from an external schedule.
- Neon pooled connections and Laravel need checking (SSL mode, and whether the pooled host works with migrations). Use the direct connection for migrations if it does not.
- Everything here depends on free tiers staying free. The README should say the demo may be offline at times.

## Spike checklist (about one hour)

- [ ] Neon: create a project, note the connection string, connect from local Laravel and run a migration.
- [ ] Render: deploy a minimal Laravel container from GitHub, confirm it builds and answers `/up`.
- [ ] Render to Neon: confirm the deployed container can read and write the database.
- [ ] Vercel: deploy a placeholder React app and add rewrites for `/api` and `/sanctum` to the Render URL.
- [ ] Confirm a cookie set by `/sanctum/csrf-cookie` is stored by the browser on the Vercel domain.
- [ ] Record the wake up time after 15 idle minutes.
- [ ] Write the real, current limits here and change the status to Accepted or Rejected.
