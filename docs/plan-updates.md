# Plan updates

Changes to the delivery plan since it was first written. Last updated 17 November 2026.

## Where things stand

**Phase 0 (decisions and setup): done.**
- Brand chosen: Nordlys Pharmacy, a fictional business.
- Public repository with README, MIT licence and a hosting decision record ([`docs/decisions/0001-hosting.md`](decisions/0001-hosting.md)).

**Hosting spike: done, decision accepted.**
- Vercel Hobby for the frontend, Laravel in Docker on a Render free web service (Frankfurt), Neon free Postgres (Frankfurt), database queue driver.
- The Vercel proxy to Render works, and the cookie from `/sanctum/csrf-cookie` is stored on the Vercel domain. This was the riskiest assumption in the layout.
- Cold start was about 24 s after 20 idle minutes, and over a minute after several idle days. Warm responses take about 100 to 400 ms.

**Phase 1 (foundations and first deploy): partly done.**
- Done: Laravel 12 backend with Sanctum, Docker deploy, Neon connection, queue worker, Vercel proxy test.
- Still open: the React and TypeScript app, two factor login with Fortify, one thin booking slice end to end, CI, architecture diagram.

## Cold start handling

Free hosting means the API sleeps after 15 idle minutes. These items reduce the effect on a visitor.

| # | Item | Phase | Done when |
|---|---|---|---|
| 1 | **Warm-up call.** The frontend calls `/api/ping` in the background as soon as any page loads, so the API is usually awake before the visitor reaches booking or login. | Phase 1 (thin slice), reused later | Opening the site after an idle period wakes the API without a click |
| 2 | **"Waking up" message.** A clear message with a spinner for any API request that takes more than a couple of seconds. | Phase 2, used by the booking form (Phase 3) and admin login (Phase 4) | A slow first request shows the message instead of a dead page |
| 3 | **Keep-alive ping (optional).** A scheduled GitHub Actions workflow calls `/up` about every ten minutes. | Phase 5, earlier if there is slack | Cold starts no longer happen during normal hours |
| 4 | **Fallback evidence.** A short recording of the full flow, screenshots in the README, and an honest note about free hosting. | README note done. Recording and screenshots in Phase 5 | A reviewer who does not wait still sees the whole project |

Before doing item 3:
- Check that Render's terms allow keep-alive pings on a free service.
- One always-on service uses about 744 of the 750 free hours in a 31 day month, so it only fits for a single service.
- GitHub pauses scheduled workflows after 60 days without repository activity.
- Render's own health checks do not keep the service awake, as observed during the spike.

## Notes carried forward

- Add `$middleware->statefulApi();` in `bootstrap/app.php` so authenticated `/api` calls use the session.
- Replace the default Laravel welcome page at `/` on the API with a short JSON message and a `noindex` header.
- On Windows, `php artisan serve` may fail to start. `php -S 127.0.0.1:8080 -t public` works instead.
- An old libpq in some Windows PHP builds needs the Neon endpoint ID in the password for local connections. Deployed containers use a modern libpq and do not need it.
- The throwaway `spike/vercel-proxy` folder and its Vercel project can be deleted once the real frontend project exists.

## Decision needed

Phase 1 was planned to end on 17 November and is not finished. Either extend Phase 1 and shift the later phases, or apply the time box rule and carry the open items on a later list.
