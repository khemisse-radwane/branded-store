# Dafa Kitchen Docs

Brand: **مطبخ دفا / Dafa Kitchen**  
Domain: `dafakitchen.shop`  
Backend domain: `api.dafakitchen.shop`  
Market: Saudi Arabia  
Language: Arabic-first, RTL  
Payment model: COD only  
Database name: `dafa_kitchen`

This folder is the implementation handoff for the AI coder. Build a branded DTC store that feels like it owns the products, sells at premium prices, increases AOV, and protects COD confirmation/delivery rates.

## Docs Map

1. [Brand Positioning](./01-brand-positioning.md)
2. [ICP, Language, Emotion, Proof](./02-icp-language-emotion-proof.md)
3. [CRO, Offers, Cart, Checkout](./03-cro-offers-cart-checkout.md)
4. [Site Architecture](./04-site-architecture.md)
5. [Frontend Spec](./05-frontend-spec.md)
6. [Backend Spec](./06-backend-spec.md)
7. [Tracking Pixels and CAPI](./07-tracking-pixels-capi.md)
8. [Database, Sheets, and Webhooks](./08-database-sheets-webhooks.md)
9. [Design System](./09-design-system.md)
10. [Coding Rules](./10-coding-rules.md)
11. [Docker and EasyPanel Deployment](./11-docker-easypanel.md)
12. [UGC, Proof, Materials, and Skills](./12-ugc-proof-materials-skills.md)
13. [Coder Prompt](./13-coder-prompt.md)

## CSV Templates

- [Product catalog](./templates/products.csv)
- [Sheet columns](./templates/sheet-columns.csv)
- [Sample order export](./templates/orders-sample.csv)
- [Analytics events](./templates/analytics-events.csv)

## Required Output From Coder

The coder must deliver:

- `frontend/` Next.js app.
- `backend/` FastAPI app.
- Docker setup for local testing.
- `.env.example` files for frontend and backend.
- PostgreSQL migrations that run on backend start.
- Google Sheet Apps Script webhook file.
- CSV templates copied into the repo.
- Tracking for Meta, TikTok, and Snapchat web pixels plus server CAPI.
- Responsive Arabic-first store with product pages, cart drawer, checkout popup, upsell, thank-you page, and webhook order submission.
