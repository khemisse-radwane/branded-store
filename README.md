# Dafa Kitchen

Arabic-first COD ecommerce store for **مطبخ دفا / Dafa Kitchen** in Saudi Arabia.

## Structure

```text
frontend/   Next.js Arabic RTL storefront
backend/    FastAPI order API, PostgreSQL, Sheets webhook, CAPI
docs/       Strategy and implementation docs
```

## Local Development

Copy env examples:

```bash
cp frontend/.env.example frontend/.env.local
cp backend/.env.example backend/.env
```

Start the full stack:

```bash
docker compose up --build
```

Services:

- Frontend: http://localhost:3000
- Backend: http://localhost:8000
- Health: http://localhost:8000/health

## Production Targets

- Storefront: `https://dafakitchen.shop`
- Backend: `https://api.dafakitchen.shop`
- Database: `dafa_kitchen`

