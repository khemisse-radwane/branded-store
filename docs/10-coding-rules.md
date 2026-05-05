# Coding Rules

## General

- Use TypeScript in frontend.
- Use Python typing in backend.
- Keep code modular and readable.
- Avoid magic numbers in checkout/pricing; centralize product and offer data.
- Do not hardcode secrets.
- Do not expose CAPI tokens in frontend.
- Build for Arabic RTL from the start.

## Frontend Rules

- Next.js App Router.
- Server components by default.
- Client components only for interactive parts: cart, modals, forms, tracking.
- Use Zod schemas for checkout validation.
- Use Zustand for cart state.
- Use `next/script` for pixels.
- Use consistent event IDs between browser and backend.
- Keep product data centralized.
- Use semantic HTML and accessible labels.

## Backend Rules

- Validate all incoming order payloads with Pydantic.
- Recalculate totals server-side from known product/offer data.
- Do not trust frontend totals.
- Normalize and validate KSA phone server-side.
- Persist order before external webhook/CAPI calls.
- External integrations must not prevent order creation.
- Run migrations on startup.
- Use structured logs.

## Security Rules

- CORS allow local frontend and production domain only.
- Rate-limit order endpoint if practical.
- Validate webhook shared secret.
- Never log raw access tokens.
- Avoid logging raw phone numbers in production logs.
- Store raw phone only because it is operationally needed for COD; hash for ad platforms.

## Testing Requirements

Frontend:

- Phone validation tests.
- Cart total tests.
- Offer selector tests.
- Checkout flow manual test.
- Responsive screenshot check.

Backend:

- Phone normalization tests.
- Total recalculation tests.
- Order creation test.
- Webhook payload test.
- CAPI payload unit tests with tokens mocked.

## Definition of Done

The build is not done until:

- Local Docker stack runs.
- Frontend can create an order.
- Backend saves to PostgreSQL.
- Backend sends Sheets webhook when configured.
- Thank-you page shows order number.
- Browser pixel debug logs show event IDs.
- CAPI services are implemented behind env flags.
- Mobile layout is tested.
- `.env.example` files exist.

