# Design System

## Design Goal

The store must feel like a premium Saudi kitchen brand, not a generic dropshipping page.

Design words:

- Warm.
- Clean.
- Organized.
- Trustworthy.
- Family-ready.
- Premium but practical.

## Layout

Use full-width sections with constrained inner content.

Do not overuse cards. Cards are for:

- Product cards.
- Reviews.
- Offer selectors.
- Cart/cross-sell items.

Desktop sections:

- Alternate text/image layouts.
- For RTL, primary text usually on the right and image on the left.
- Next section can reverse for rhythm.

Mobile:

- Stack content.
- Keep CTAs visible.
- Use sticky bottom CTA on product pages.

## Colors

Suggested palette:

- Warm white: `#FFFCF7`
- Soft cream: `#F7EFE3`
- Date brown: `#5A3825`
- Deep charcoal: `#1F1B18`
- Olive accent: `#6E7A4F`
- Warm gold: `#C99A45`
- Error red: `#B42318`
- Success green: `#1F7A4D`

Avoid making the whole UI beige/brown. Use warm white and charcoal as the main base, with olive/gold accents.

## Typography

Arabic:

- `Tajawal` for approachable ecommerce.
- Or `IBM Plex Sans Arabic` for more premium/clean.

Recommended:

- Use `IBM Plex Sans Arabic` for headings and body if available.

English brand subtext:

- Inter or system sans.

## Header

Logo concept:

- Circle or rounded rectangle containing a simple kitchen/warmth icon.
- Next to it:
  - **مطبخ دفا**
  - smaller **Dafa Kitchen**

Header must include:

- Menu.
- Cart icon.
- Mobile menu drawer.

## Icons

Use lucide-react icons:

- ShoppingCart.
- Menu.
- Star.
- ShieldCheck.
- Truck.
- Phone.
- MessageCircle.
- PackageCheck.
- Flame or CookingPot if available.

## Components

Required components:

- AnnouncementBar.
- Header.
- Footer.
- ProductCard.
- OfferSelector.
- CartDrawer.
- CheckoutModal.
- UpsellModal.
- ReviewCard.
- TrustStrip.
- FAQAccordion.
- SectionImagePlaceholder.
- StickyMobileCTA.

## Image Style

Until real images arrive:

- Use polished placeholders.
- Avoid dark blurry stock images.
- Show product clearly.
- Use kitchen counter/table scenes.
- Use Arabic labels inside image only sparingly.

## UI Rules

- Buttons must look tappable on mobile.
- Minimum tap target: 44px.
- Cards radius: 8px or less.
- Avoid nested cards.
- No decorative gradient blobs/orbs.
- Text must not overflow buttons or cards.
- Use star ratings carefully; make mock data configurable.

## Product Page Visual Rhythm

Each product page should include:

- Hero image.
- Detail image.
- Before/after image.
- Lifestyle image.

Use alternating sections:

1. Text right / image left.
2. Image right / text left.
3. Text right / image left.

