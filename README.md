<div align="center">

# Nordlys Pharmacy

**A fictional pharmacy website and booking system, built as a full stack portfolio demo.**

![Status](https://img.shields.io/badge/status-work%20in%20progress-orange)
![Laravel](https://img.shields.io/badge/Laravel-API-FF2D20?logo=laravel&logoColor=white)
![React](https://img.shields.io/badge/React-frontend-61DAFB?logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?logo=postgresql&logoColor=white)
<!-- Add the CI badge once the workflow exists:
![CI](https://github.com/SergioSNW/nordlys-pharmacy/actions/workflows/ci.yml/badge.svg) -->

[Live demo](#) · [Demo recording](#) · [Case study](#)

</div>

> [!IMPORTANT]
> **This is a fictional demo project.** Nordlys Pharmacy is not a real business. No real patients, no real data, no real emails and no real payments are involved.

<!-- Screenshot placeholder, add one after Phase 2:
![Nordlys Pharmacy home page](docs/images/home.png) -->

## About

Nordlys Pharmacy is a pharmacy website with an appointment booking system. I built it to show full stack skills in one place: a Laravel API, a React and TypeScript frontend, PostgreSQL, background queues, two factor authentication, automated tests and CI.

It is inspired by a real pharmacy brief I worked on separately. That project is private and is not part of this repository. This demo has its own invented brand, content and data.

**Status:** work in progress (Phase 0, started 9 November 2026).

## Contents

- [Ground rules](#ground-rules)
- [Features](#features)
- [Tech stack](#tech-stack)
- [Architecture](#architecture)
- [Running locally](#running-locally)
- [Demo access](#demo-access)
- [Safeguards on the public demo](#safeguards-on-the-public-demo)
- [Roadmap](#roadmap)
- [Licence](#licence)
- [Author](#author)

## Ground rules

- **Fictional business only.** The name, brand and content are invented. No real pharmacy name, logo, brand lineup or photos are used.
- **Fake data only.** All demo patients are invented. No real personal or health data appears anywhere, including screenshots and recordings.
- **Honest description.** This is a demo project inspired by a real brief. It is not delivered client work.
- **Clearly marked as a demo.** Every page shows a demo banner and is set to `noindex`, so it never appears in search results as a real pharmacy.
- **Sample legal pages.** The privacy and cookie pages are marked as samples.
- **Images.** Only freely licensed or self made images, with each licence checked. Brand names in the gallery are invented.

## Features

<details open>
<summary><b>Public site</b></summary>

- Home, Services (four routes), Brands gallery (invented brands), Contact, urgent advice page, sample privacy and cookie pages
- Pre-rendered public pages, responsive and accessible
- Soft Scandinavian style with gradients

</details>

<details open>
<summary><b>Booking</b></summary>

- Four services with 15 minute slots
- One booking per slot across a shared calendar
- Opening hours per service route, plus closures
- Double booking protection at database level
- Multi step request form (name, date of birth, contact details only), consent step and a confirmation page with a reference

</details>

<details open>
<summary><b>Emails</b></summary>

- Patient confirmation and staff alert, sent through a background queue with retries
- Shown on an in-app outbox page. No real email is sent.

</details>

<details open>
<summary><b>Admin panel</b></summary>

- Login with two factor authentication
- Dashboard, list and filters
- Create, edit and cancel appointments (with a reason)
- Internal notes, status history and audit log
- Closures editor and data retention job

</details>

<details open>
<summary><b>Engineering evidence</b></summary>

- Automated tests and CI
- Architecture diagram and short decision records
- Case study and demo recording

</details>

<details>
<summary><b>Not included (on purpose)</b></summary>

- Symptom or screening questions, red flag logic, or any health data
- Real client content, logo, photos, domain or email service
- Payments, SMS, reminders and patient accounts
- Real legal compliance paperwork. The technical controls stay, the paperwork does not.

</details>

## Tech stack

| Area | Choice |
|---|---|
| Backend | Laravel (PHP), Sanctum, Fortify (two factor) |
| Frontend | React, TypeScript, React Hook Form, Zod |
| Database | PostgreSQL |
| Queue | Laravel database queue driver (no Redis) |
| Testing | PHPUnit / Pest, Playwright for end to end |
| CI | GitHub Actions |
| Hosting | Frontend on Vercel (Hobby), Laravel in a container, managed PostgreSQL *(to be confirmed in the hosting spike)* |

## Architecture

*Architecture diagram to be added in Phase 1.*

Hosting layout in brief: the React frontend is served from Vercel, and Vercel rewrites `/api` and `/sanctum` to the Laravel host so the browser sees a single origin and cookie based authentication works without CORS. The fallback is one container serving both the API and the built frontend.

Decision records will live in [`docs/decisions`](docs/decisions).

## Running locally

*To be completed in Phase 1.*

```bash
git clone https://github.com/SergioSNW/nordlys-pharmacy.git
cd nordlys-pharmacy
```

## Demo access

*To be completed after the first deploy.*

The public demo uses a demo admin account without enforced two factor, rate limited, with data reset every night. Two factor authentication is shown through the automated tests, this README and the demo recording.

## Safeguards on the public demo

- Demo banner and `noindex` on every page
- Rate limits on the booking and login routes
- No real email sending and no file uploads
- Nightly job that restores the seed data

## Roadmap

| Phase | Dates | Outcome |
|---|---|---|
| 0. Decisions and setup | 9 to 10 Nov | Brand, public repository, hosting spike |
| 1. Foundations and first deploy | 11 to 17 Nov | Login with two factor and one booking saved, deployed |
| 2. Design and public site | 18 Nov to 1 Dec | All public pages and the gallery live |
| 3. Booking engine and emails | 2 to 15 Dec | Full booking flow, queued emails, outbox page |
| 4. Admin panel | 16 Dec to 5 Jan | Staff can run a day of appointments |
| 5. Polish and evidence | 6 to 12 Jan | Tests, README, case study, recording, final deploy |

Stretch goals, only if time allows: live dashboard updates and extra service pages with sample text.

## Licence

*To be decided (MIT is the likely choice).*

## Author

**Sergio Santamaria Nora**
[GitHub](https://github.com/SergioSNW) · [LinkedIn](https://www.linkedin.com/in/sergio-santamaria-nora/)
