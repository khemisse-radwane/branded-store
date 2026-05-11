# Docker and EasyPanel Deployment

## Local Docker

Create root `docker-compose.yml` with services:

- `frontend`
- `backend`
- `dafa_kitchen_database`

Ports:

- Frontend: `3000`
- Backend: `8000`
- Postgres: `5432`

Database:

```env
POSTGRES_DB=dafa_kitchen
POSTGRES_USER=dafakitchen
POSTGRES_PASSWORD=dafakitchen
```

Local backend database URL:

```env
DATABASE_URL=postgresql+psycopg://dafakitchen:dafakitchen@dafa_kitchen_database:5432/dafa_kitchen
```

## Frontend Docker

Use multi-stage Dockerfile:

- Install dependencies.
- Build Next.js.
- Run production server.

Expose:

```text
3000
```

## Backend Docker

Backend Dockerfile:

- Python 3.12 slim.
- Install dependencies.
- Copy app.
- Run migrations.
- Start FastAPI.

Startup command:

```bash
alembic upgrade head && uvicorn app.main:app --host 0.0.0.0 --port 8000
```

Expose:

```text
8000
```

## EasyPanel Production

Production domains:

- Frontend: `https://dafakitchen.shop`
- Backend: `https://api.dafakitchen.shop`

EasyPanel needs:

- PostgreSQL service.
- Backend app service.
- Frontend app service.
- Environment variables configured separately for frontend/backend.

## Production Env

Frontend:

```env
NEXT_PUBLIC_SITE_URL=https://dafakitchen.shop
NEXT_PUBLIC_API_BASE_URL=https://api.dafakitchen.shop
NEXT_PUBLIC_META_PIXEL_ID=
NEXT_PUBLIC_TIKTOK_PIXEL_ID=
NEXT_PUBLIC_SNAP_PIXEL_ID=
NEXT_PUBLIC_ENABLE_PIXEL_DEBUG=false
```

Backend:

```env
APP_ENV=production
API_BASE_URL=https://api.dafakitchen.shop
FRONTEND_URL=https://dafakitchen.shop
CORS_ORIGINS=https://dafakitchen.shop
DATABASE_URL=postgres://dafakitchen:dafakitchen@dafa_kitchen_dafakitchen_db:5432/dafakitchen?sslmode=disable
ORDER_WEBHOOK_URL=
ORDER_WEBHOOK_SECRET=
META_PIXEL_ID=
META_ACCESS_TOKEN=
TIKTOK_PIXEL_CODE=
TIKTOK_ACCESS_TOKEN=
SNAP_PIXEL_ID=
SNAP_ACCESS_TOKEN=
ENABLE_CAPI=true
ENABLE_SHEETS_WEBHOOK=true
```

## CORS

Allow:

- `http://localhost:3000`
- `https://dafakitchen.shop`

Do not use wildcard CORS in production.

Set `CORS_ORIGINS` to a comma-separated list if preview or staging frontend domains need API access.
Browser origins must include the scheme, so use `https://dafakitchen.shop`, not only `dafakitchen.shop`.

## Frontend Build Args

Next.js inlines `NEXT_PUBLIC_*` values during the image build. In EasyPanel, configure these as build arguments for the frontend app, not only runtime environment variables. The Dockerfile defaults to the production domains above, but explicit build args are safer when domains or pixel IDs change.

## Deployment Checklist

- DNS points to EasyPanel.
- SSL active for both domains.
- Frontend env uses production API URL.
- Frontend build args include production `NEXT_PUBLIC_*` values.
- Backend CORS allows production frontend.
- Database migrations ran.
- Health endpoint works.
- Test order reaches database.
- Test order reaches Sheet.
- Pixel test tools confirm events.
