# TinyLink

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
