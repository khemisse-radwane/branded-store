# Backend Spec

## Stack

Use:

- Python 3.12.
- FastAPI.
- Pydantic v2.
- SQLAlchemy 2.x.
- Alembic for migrations.
- PostgreSQL.
- httpx for outbound webhook/CAPI calls.
- Uvicorn/Gunicorn for serving.

## Backend Responsibilities

The backend handles:

- Order creation.
- KSA phone normalization.
- PostgreSQL persistence.
- Google Sheets webhook submission.
- Meta/TikTok/Snap CAPI events.
- Server-side hashing for ad platforms.
- Event deduplication IDs.
- Health checks.

## Suggested Backend Structure

```text
backend/
  app/
    main.py
    core/
      config.py
      security.py
    db/
      session.py
      models.py
      migrations/
    schemas/
      orders.py
      tracking.py
    services/
      phone.py
      hashing.py
      sheets.py
      capi_meta.py
      capi_tiktok.py
      capi_snap.py
    routers/
      orders.py
      health.py
  alembic.ini
  Dockerfile
  requirements.txt
  .env.example
```

## API Endpoints

### Health

`GET /health`

Returns:

```json
{ "status": "ok" }
```

### Create Order

`POST /orders`

Request:

```json
{
  "event_id": "uuid-or-cuid",
  "name": "سارة",
  "phone": "+9665XXXXXXXX",
  "items": [
    {
      "product_id": "food_warmer",
      "offer_id": "one",
      "quantity": 1,
      "unit_price": 199,
      "total_price": 199
    }
  ],
  "upsell": {
    "accepted": true,
    "product_id": "rice_dispenser",
    "price": 99
  },
  "totals": {
    "subtotal": 298,
    "delivery_fee": 0,
    "discount": 100,
    "total": 298,
    "currency": "SAR"
  },
  "client": {
    "landing_page": "https://dafakitchen.shop/products/dafaya-food-warmer",
    "referrer": "",
    "user_agent": "",
    "ip": "",
    "fbp": "",
    "fbc": "",
    "ttp": "",
    "ttclid": "",
    "sc_click_id": "",
    "utm_source": "",
    "utm_medium": "",
    "utm_campaign": "",
    "utm_content": "",
    "utm_term": ""
  }
}
```

Response:

```json
{
  "ok": true,
  "order_id": "DK-20260430-0001",
  "status": "pending_confirmation"
}
```

## Phone Handling

Normalize phone to:

- `phone_e164`: `+9665XXXXXXXX`
- `phone_digits`: `9665XXXXXXXX`

For CAPI hashing:

- Meta: hash digits-only with country code, no `+`.
- Snap: hash digits-only with country code, no `+`.
- TikTok: use a provider adapter. Default should hash normalized E.164 and also allow switching to digits-only through env if diagnostics require it. TikTok docs confirm phone hashing is required; verify final accepted normalization in TikTok Events Manager test diagnostics.

## Hashing

Use SHA-256 lowercase hex.

Normalize before hashing:

- Trim strings.
- Lowercase emails if used later.
- Phone must be normalized first.

Do not hash:

- IP address.
- User agent.
- `fbp`.
- `fbc`.
- `ttp`.
- Click IDs.

## Database Models

Minimum tables:

- `orders`
- `order_items`
- `tracking_events`

Recommended order fields:

- id.
- public_order_id.
- customer_name.
- phone_e164.
- phone_digits.
- status.
- subtotal.
- delivery_fee.
- discount.
- total.
- currency.
- payment_method.
- source_url.
- user_agent.
- ip_address.
- fbp.
- fbc.
- ttp.
- ttclid.
- sc_click_id.
- utm fields.
- event_id.
- sheet_synced_at.
- capi_meta_sent_at.
- capi_tiktok_sent_at.
- capi_snap_sent_at.
- created_at.
- updated_at.

## Backend Env Example

Create `backend/.env.example`:

```env
APP_ENV=local
API_BASE_URL=http://localhost:8000
FRONTEND_URL=http://localhost:3000

DATABASE_URL=postgresql+psycopg://dafakitchen:dafakitchen@dafakitchen_database:5432/dafa_kitchen

ORDER_WEBHOOK_URL=
ORDER_WEBHOOK_SECRET=

META_PIXEL_ID=
META_ACCESS_TOKEN=
META_TEST_EVENT_CODE=

TIKTOK_PIXEL_CODE=
TIKTOK_ACCESS_TOKEN=
TIKTOK_TEST_EVENT_CODE=
TIKTOK_PHONE_HASH_MODE=e164

SNAP_PIXEL_ID=
SNAP_ACCESS_TOKEN=

ENABLE_CAPI=true
ENABLE_SHEETS_WEBHOOK=true
```

## Migrations On Backend Start

Docker entrypoint should run:

```bash
alembic upgrade head
```

Then start FastAPI.

Do not use destructive migrations on startup.
