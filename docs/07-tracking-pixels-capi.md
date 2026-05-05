# Tracking Pixels and CAPI

## Goal

Use web pixels for fast browser events and CAPI for reliable server-side purchase/order events.

Platforms:

- Meta/Facebook.
- TikTok.
- Snapchat.

## Events To Track

Browser pixel events:

- `PageView`
- `ViewContent`
- `AddToCart`
- `InitiateCheckout`
- `Purchase`

Server CAPI events:

- `Purchase` after final order submission.
- Optional: `InitiateCheckout` after valid checkout form submit.

## Deduplication

Use the same `event_id` in browser and server events.

Meta:

- Browser event uses `eventID`.
- Server event uses `event_id`.
- Meta dedupes by event name + matching event ID.

TikTok:

- Browser and Events API should share `event_id`.
- TikTok deduplication requires matching `event_id` when the same event is sent through Pixel and Events API.

Snap:

- Server `event_id` must match the Snap Pixel `client_dedup_id` for matching events.
- For purchase, transaction/order id can also be used.

## Web Pixels

Load deferred:

- Use Next.js `Script` with `strategy="afterInteractive"` or lazy strategy for non-critical scripts.
- Do not block rendering.
- Provide debug logging when `NEXT_PUBLIC_ENABLE_PIXEL_DEBUG=true`.

Important:

- Do not place CAPI tokens in frontend.
- Do not send raw phone to browser pixels unless a platform-specific advanced matching implementation is intentionally added and privacy-reviewed.
- For this build, send PII through backend CAPI only.

## Phone Normalization For CAPI

Accept KSA mobile numbers:

- `05XXXXXXXX`
- `5XXXXXXXX`
- `9665XXXXXXXX`
- `+9665XXXXXXXX`
- `009665XXXXXXXX`

Normalize:

- E.164 display: `+9665XXXXXXXX`
- Digits only: `9665XXXXXXXX`

Hash:

- SHA-256 lowercase hex.

Platform adapter rules:

| Platform | Phone sent to CAPI | Hashing |
|---|---|---|
| Meta | `9665XXXXXXXX` before hash | Required for `ph` |
| Snapchat | `9665XXXXXXXX` before hash, no `+` | Required for `ph` |
| TikTok | Start with E.164 hash mode, configurable with `TIKTOK_PHONE_HASH_MODE=e164|digits` | Required for phone match key |

Note on TikTok:

TikTok official help confirms phone hashing is required for Events API matching. Some TikTok integrations expect E.164-style normalization and some diagnostics are sensitive to formatting. Implement the env switch so testing can confirm whether `+966...` or `966...` performs best without code changes.

## Meta CAPI Notes

Official guidance found:

- Contact identifiers such as phone/email require hashing.
- Phone numbers should include country code and remove symbols/leading zeros.
- `client_ip_address`, `client_user_agent`, `fbp`, and `fbc` must not be hashed.
- Use matching `event_id` for browser/server deduplication.

Server purchase event should include:

- `event_name`: `Purchase`
- `event_time`
- `event_id`
- `action_source`: `website`
- `event_source_url`
- `user_data`
  - `ph`
  - `client_ip_address`
  - `client_user_agent`
  - `fbp`
  - `fbc`
- `custom_data`
  - `currency`: `SAR`
  - `value`
  - `contents`
  - `content_ids`
  - `num_items`

## TikTok Events API Notes

Official guidance found:

- TikTok recommends Pixel + Events API together.
- Phone/email/external ID matching values require SHA-256 hashing.
- IP and user agent need manual configuration for Events API.
- Deduplication requires `event_id` when sending overlapping Pixel and Events API events.

Server event should include:

- `event`: `Purchase`
- `event_id`
- `event_time`
- `event_source`: `web`
- `event_source_url`
- `user`
  - hashed phone.
  - IP.
  - user agent.
  - `ttp` if available.
  - `ttclid` if available.
- `properties`
  - value.
  - currency.
  - contents.

Exact payload shape should be implemented against the current TikTok Business API docs at build time and tested in Events Manager.

## Snapchat CAPI Notes

Official guidance found:

- `ph` requires normalization and hashing.
- Phone should include country code, remove double leading zero, remove local leading zero, and exclude non-numeric characters including `+`.
- Include `event_id` for deduplication.
- If using Snap Pixel, `event_id` should match `client_dedup_id`.
- At least one matching identifier is required, such as hashed phone or IP + user agent.

Server purchase event should include:

- `event_name`: `PURCHASE`
- `event_time`
- `event_id`
- `action_source`: `WEB`
- `event_source_url`
- `user_data`
  - `ph`
  - `client_ip_address`
  - `client_user_agent`
- `custom_data`
  - `currency`: `SAR`
  - `value`
  - item IDs.

## Debug Requirements

Frontend:

- Console log event name, event ID, and platform only when debug flag is enabled.
- Never log access tokens.
- Never log hashed PII in production.

Backend:

- Store CAPI sent timestamps.
- Store provider response status and error message in logs.
- Do not block order creation if one CAPI provider fails.
- Retry webhook/CAPI asynchronously if practical.

## Consent and Privacy

Add privacy page explaining:

- The site uses tracking pixels for ads measurement.
- Orders are submitted to Dafa Kitchen for confirmation.
- Phone is used for COD confirmation and delivery.
- Data may be shared with ad platforms in hashed form for measurement.

