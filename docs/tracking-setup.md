# Tracking Setup

This store sends redundant browser + server events for Meta, TikTok, and Snapchat with a shared `event_id` so each platform can deduplicate matching pixel/API events.

## Official Docs Checked

- Meta Conversions API customer information parameters: https://developers.facebook.com/docs/marketing-api/conversions-api/parameters/customer-information-parameters/
- Meta Pixel + CAPI deduplication: https://developers.facebook.com/docs/marketing-api/conversions-api/deduplicate-pixel-and-server-events/
- TikTok Events API setup and deduplication: https://ads.tiktok.com/help/article/getting-started-events-api and https://ads.tiktok.com/help/article/event-deduplication
- TikTok Business API SDK event payload models: https://github.com/tiktok/tiktok-business-api-sdk
- Snap Conversions API parameters, best practices, and test events: https://developers.snap.com/api/marketing-api/Conversions-API/Parameters

## Environment Variables

Frontend public IDs only:

```env
NEXT_PUBLIC_META_PIXEL_ID=
NEXT_PUBLIC_TIKTOK_PIXEL_ID=
NEXT_PUBLIC_SNAP_PIXEL_ID=
NEXT_PUBLIC_ENABLE_PIXEL_DEBUG=false
```

Backend secrets stay server-side:

```env
META_PIXEL_ID=
META_ACCESS_TOKEN=
META_GRAPH_VERSION=v22.0
META_TEST_EVENT_CODE=

TIKTOK_PIXEL_CODE=
TIKTOK_ACCESS_TOKEN=
TIKTOK_EVENTS_API_URL=https://business-api.tiktok.com/open_api/v1.3/pixel/track/
TIKTOK_TEST_EVENT_CODE=

SNAP_PIXEL_ID=
SNAP_ACCESS_TOKEN=
SNAP_TEST_EVENT_CODE=

ENABLE_CAPI=true
```

## Event Mapping

| Store event | Meta | TikTok | Snapchat |
| --- | --- | --- | --- |
| PageView | PageView | Pageview | PAGE_VIEW |
| ViewProduct | ViewContent | ViewContent | VIEW_CONTENT |
| AddToCart | AddToCart | AddToCart | ADD_CART |
| InitiateCheckout | InitiateCheckout | InitiateCheckout | START_CHECKOUT |
| Lead | Lead | SubmitForm | SIGN_UP |
| Purchase | Purchase | CompletePayment | PURCHASE |
| UpsellView | custom UpsellView | custom UpsellView | CUSTOM_EVENT_1 |
| UpsellAccepted | custom UpsellAccepted | custom UpsellAccepted | CUSTOM_EVENT_2 |
| UpsellRejected | custom UpsellRejected | custom UpsellRejected | CUSTOM_EVENT_3 |

## Deduplication

- Every tracked action gets one `event_id`.
- Browser pixels and backend APIs receive the same `event_id`.
- Purchase is stable and based on the backend order ID: `purchase_{order_id}`.
- Purchase pixel fires only after `/orders` confirms the order. Purchase CAPI is sent by the backend order integration, not by the frontend `/tracking/events` beacon.

## Customer Data

The store currently collects only name and phone.

- Meta: `ph`, `fn`, and `ln` are SHA-256 hashed server-side. Phone is normalized to country-code digits, without `+`, symbols, spaces, or local leading zero.
- TikTok: `phone_number` is SHA-256 hashed server-side. The integration uses the same country-code digits normalization for consistency.
- Snapchat: `ph`, `fn`, and `ln` are SHA-256 hashed server-side. Phone is normalized to country-code digits, without `+`, symbols, spaces, double-zero international prefix, or local leading zero.
- IP address, user agent, `fbp`, `fbc`, `ttp`, `ttclid`, `sc_click_id`, and `sc_cookie1` are not hashed.

## Attribution Captured

- Meta: `_fbp`, `_fbc`, and `fbclid`. If `_fbc` is missing and `fbclid` exists, the frontend creates the documented `fb.1.{timestamp}.{fbclid}` style value.
- TikTok: `ttclid` and `_ttp`.
- Snapchat: `ScCid` / `sccid` / `scid` query values and `_scid` / `sc_cookie1`.

## Testing

1. Set the public frontend pixel IDs and backend API credentials.
2. Use platform test/debug values:
   - Meta: set `META_TEST_EVENT_CODE` and inspect Events Manager Test Events.
   - TikTok: use Events Manager diagnostics/test tooling and enable `NEXT_PUBLIC_ENABLE_PIXEL_DEBUG=true` locally.
   - Snapchat: set `SNAP_TEST_EVENT_CODE` and use Events Manager or the Snap CAPI validate endpoints.
3. Place a COD test order and confirm:
   - Browser pixel Purchase fires only after `/orders` returns success.
   - Backend `tracking_events` rows show each platform response.
   - Purchase pixel and backend CAPI share `purchase_{order_id}`.
4. Run local checks:

```bash
cd frontend && npm run typecheck
cd .. && python -m unittest discover -s backend/tests
python -m compileall backend/app
```
