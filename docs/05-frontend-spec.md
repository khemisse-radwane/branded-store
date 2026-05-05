# Frontend Spec

## Stack

Use:

- Next.js 15+ with App Router.
- React 19+ if compatible with selected dependencies.
- TypeScript.
- Tailwind CSS.
- shadcn/ui for accessible primitives where useful.
- Radix UI for dialog/drawer primitives if not using shadcn wrappers.
- Zustand for cart state.
- React Hook Form + Zod for checkout validation.
- Framer Motion only for small, purposeful transitions.
- Lucide React for icons.
- `next/image` for image optimization.

Avoid:

- Heavy animation libraries.
- Large UI kits with poor RTL support.
- Client-only whole app architecture.

## RTL and Arabic

Set:

```html
<html lang="ar" dir="rtl">
```

Use Arabic font:

- Primary: `Tajawal` or `IBM Plex Sans Arabic`.
- Secondary English text: system sans or Inter.

Use logical CSS where possible:

- `ms-*`, `me-*`, `start`, `end`.
- Avoid hard-coded left/right unless intentionally designing alternating sections.

## Performance Requirements

- Defer web pixels.
- Lazy-load below-fold images.
- Use static product data at build time where possible.
- Avoid blocking third-party scripts.
- Product page LCP should prioritize hero image and headline.
- Keep JavaScript lean.

## Suggested Frontend Structure

```text
frontend/
  app/
    layout.tsx
    page.tsx
    products/
      page.tsx
      [slug]/
        page.tsx
    about/page.tsx
    contact/page.tsx
    thank-you/page.tsx
    privacy/page.tsx
    returns/page.tsx
    delivery/page.tsx
  components/
    layout/
    product/
    cart/
    checkout/
    tracking/
    ui/
  data/
    products.ts
    copy.ts
  lib/
    api.ts
    phone.ts
    tracking.ts
    event-id.ts
  store/
    cart-store.ts
  public/
    images/
      placeholders/
```

## Product Data

Use product data from `docs/templates/products.csv` and mirror it in `frontend/data/products.ts`.

Each product object:

- id.
- slug.
- Arabic name.
- English internal name.
- short headline.
- subheading.
- benefits.
- specs.
- offers.
- cross-sell priority.
- placeholder images.

## Image Placeholders

Create tasteful placeholders until real images are supplied.

Requirements:

- Use neutral warm kitchen backgrounds.
- Show obvious product silhouette or product card mock.
- Use 3-4 placeholder images per product page.
- Use accessible alt text in Arabic.

Possible placeholder filenames:

- `hero-dafa-kitchen.jpg`
- `food-warmer-hero.jpg`
- `food-warmer-detail-1.jpg`
- `rice-dispenser-hero.jpg`
- `rice-dispenser-detail-1.jpg`
- `vegetable-cutter-hero.jpg`
- `vegetable-cutter-detail-1.jpg`

If generating placeholders with CSS/HTML instead of real images, ensure the layout still reserves image dimensions and looks polished.

## Cart State

Cart item:

```ts
type CartItem = {
  productId: string;
  offerId: "one" | "two" | "three" | "upsell_99";
  quantity: number;
  unitPrice: number;
  totalPrice: number;
  titleAr: string;
};
```

Cart drawer behavior:

- Opens after product CTA.
- Persists to localStorage.
- Shows cross-sells.
- Can open checkout modal.

## Checkout Flow

1. User adds offer.
2. Cart drawer opens.
3. User clicks checkout.
4. Checkout modal validates name and phone.
5. On valid submit, show upsell modal.
6. If accepted, append upsell item.
7. Submit final order to backend.
8. Fire browser purchase event with event ID.
9. Redirect to thank-you page with order number.

Important:

- Generate a stable `event_id` before sending browser/server purchase events.
- Use the same `event_id` for web pixels and backend CAPI.
- Do not expose CAPI tokens in frontend.

## Frontend Env Example

Create `frontend/.env.example`:

```env
NEXT_PUBLIC_SITE_URL=http://localhost:3000
NEXT_PUBLIC_API_BASE_URL=http://localhost:8000

NEXT_PUBLIC_META_PIXEL_ID=
NEXT_PUBLIC_TIKTOK_PIXEL_ID=
NEXT_PUBLIC_SNAP_PIXEL_ID=

NEXT_PUBLIC_ENABLE_PIXEL_DEBUG=true
```

