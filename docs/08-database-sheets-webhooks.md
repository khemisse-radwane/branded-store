# Database, Sheets, and Webhooks

## Database

Local database:

```text
postgres://dafakitchen:dafakitchen@dafakitchen_database:5432/dafakitchen?sslmode=disable
```

Backend SQLAlchemy URL:

```text
postgresql+psycopg://dafakitchen:dafakitchen@dafakitchen_database:5432/dafakitchen
```

Database name:

```text
dafa_kitchen
```

Note:

The provided internal link uses database path `dafakitchen`; the user says DB name is `dafa_kitchen`. Use `dafa_kitchen` as the canonical DB name unless deployment forces a different actual DB name. Keep the final EasyPanel env explicit.

## Order Statuses

Use these statuses:

- `pending_confirmation`
- `confirmed`
- `cancelled_unconfirmed`
- `shipped`
- `delivered`
- `refused`
- `returned`

Default after checkout:

```text
pending_confirmation
```

## Google Sheet Webhook

Backend submits order to Google Apps Script web app URL.

Webhook request from backend:

```json
{
  "secret": "shared-secret",
  "order": {
    "public_order_id": "DK-20260430-0001",
    "created_at": "2026-04-30T15:00:00Z",
    "customer_name": "سارة",
    "phone_e164": "+9665XXXXXXXX",
    "status": "pending_confirmation",
    "items_summary": "دفاية الطعام الكهربائية x1",
    "subtotal": 199,
    "discount": 0,
    "delivery_fee": 0,
    "total": 199,
    "currency": "SAR",
    "payment_method": "COD",
    "upsell_accepted": false,
    "landing_page": "",
    "utm_source": "",
    "utm_medium": "",
    "utm_campaign": "",
    "utm_content": "",
    "utm_term": "",
    "event_id": ""
  }
}
```

## Apps Script File

The coder must create:

```text
backend/google-sheets/order-webhook.gs
```

Minimum behavior:

- Accept POST.
- Validate shared secret.
- Append order row.
- Return JSON.
- Never expose secret in frontend.

Apps Script starter:

```javascript
const SHEET_NAME = 'Orders';
const SHARED_SECRET = PropertiesService.getScriptProperties().getProperty('ORDER_WEBHOOK_SECRET');

function doPost(e) {
  try {
    const payload = JSON.parse(e.postData.contents);
    if (!payload || payload.secret !== SHARED_SECRET) {
      return jsonResponse({ ok: false, error: 'Unauthorized' }, 401);
    }

    const order = payload.order || {};
    const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName(SHEET_NAME);
    sheet.appendRow([
      order.public_order_id || '',
      order.created_at || '',
      order.customer_name || '',
      order.phone_e164 || '',
      order.status || '',
      order.items_summary || '',
      order.subtotal || 0,
      order.discount || 0,
      order.delivery_fee || 0,
      order.total || 0,
      order.currency || 'SAR',
      order.payment_method || 'COD',
      order.upsell_accepted || false,
      order.landing_page || '',
      order.utm_source || '',
      order.utm_medium || '',
      order.utm_campaign || '',
      order.utm_content || '',
      order.utm_term || '',
      order.event_id || '',
      new Date()
    ]);

    return jsonResponse({ ok: true });
  } catch (err) {
    return jsonResponse({ ok: false, error: String(err) }, 500);
  }
}

function jsonResponse(obj, status) {
  return ContentService
    .createTextOutput(JSON.stringify(obj))
    .setMimeType(ContentService.MimeType.JSON);
}
```

## CSV Templates

Use the CSV files in `docs/templates/` to initialize:

- Product catalog.
- Sheet headers.
- Analytics event checklist.
- Sample order export.

