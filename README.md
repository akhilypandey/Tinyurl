🌐 TinyLink – Minimal URL Shortener (Next.js + Postgres)

TinyLink is a clean, production-ready URL shortening web app built with Next.js App Router, Tailwind CSS, Prisma, and Postgres (Neon). It supports link creation, custom codes, redirects with click tracking, deletion, and a polished dashboard UI.

Built as a take-home assignment with a focus on clean architecture, testability, and UI/UX clarity.

✨ Key Features
🔗 URL Shortening

Create short links for any valid URL

Optional custom short codes ([A-Za-z0-9]{6,8})

Proper validation and meaningful error messages

🚀 Redirect + Analytics

Visiting /:code issues a 302 redirect

Automatically increments click count

Stores last clicked timestamp

🗂️ Dashboard

Add new links

View all links in a sortable / searchable table

Copy short URLs

Delete links

Clean, responsive UI with Tailwind

📊 Stats Page

/code/:code shows details for one link:

Short URL

Target URL

Total clicks

Last clicked time

Created at timestamp

♻️ API Endpoints (REST)

POST /api/links — create link (409 on duplicate)

GET /api/links — list all links

GET /api/links/:code — get stats

DELETE /api/links/:code — delete link

❤️ Healthcheck

GET /healthz → { ok: true, version: "1.0" }

🛠️ Tech Stack

Next.js (App Router)

React 18

TypeScript

Tailwind CSS

Neon Postgres (or any Postgres)

Prisma ORM

Vercel Deployment

📦 Running Locally
npm install
cp .env.example .env   # Fill in your database URL
npx prisma migrate dev
npm run dev

🌍 Live Demo

(You will add your Vercel link here)

📜 Project Requirements

This project implements the specification for:

Stable routes (/, /:code, /code/:code, /healthz)

Automated testing compatibility

Clean UI/UX (loaders, empty state, errors, responsiveness)

TinyLink is a minimal URL shortener built as a take-home assignment. It provides:

- Short link creation with optional custom code
- 302 redirect endpoint
- Click counting and last-click timestamp
- Dashboard with table, search, delete
- Per-code stats page
- Healthcheck endpoint

Stack:

- Next.js App Router
- TypeScript
- Tailwind CSS
- Postgres (e.g. Neon)
- Prisma ORM

## Getting started

### 1. Install dependencies

```bash
npm install
```

### 2. Configure environment

Create `.env` from `.env.example` and set `DATABASE_URL` and `NEXT_PUBLIC_BASE_URL`.

```bash
cp .env.example .env
# then edit .env
```

### 3. Set up database

Run Prisma migrations:

```bash
npx prisma migrate dev --name init_links
```

Generate client:

```bash
npx prisma generate
```

### 4. Run dev server

```bash
npm run dev
```

Visit:

- `http://localhost:3000/` – Dashboard
- `http://localhost:3000/healthz` – Healthcheck
- `http://localhost:3000/code/:code` – Stats page
- `http://localhost:3000/:code` – Redirect

## API

- `POST /api/links` – create link  
  - Body: `{ "targetUrl": string, "code"?: string }`
  - Validates URL; code must match `[A-Za-z0-9]{6,8}`.
  - Returns **409** if `code` already exists.

- `GET /api/links` – list all links

- `GET /api/links/:code` – stats for single code

- `DELETE /api/links/:code` – delete a link

## Healthcheck

- `GET /healthz` – returns `200` with JSON:

```json
{ "ok": true, "version": "1.0", "uptimeSeconds": 123.4 }
```

## Notes

- Codes are globally unique across all users.
- Each redirect increments `totalClicks` and updates `lastClickedAt`.
- After deletion, `/:code` returns **404** and no longer redirects.
