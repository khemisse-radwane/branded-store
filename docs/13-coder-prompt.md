# Prompt For AI Coder

Use this prompt with the AI coder.

```markdown
You are building the Dafa Kitchen DTC ecommerce store for KSA.

Read and follow every doc in `docs/` before coding:

- `docs/01-brand-positioning.md`
- `docs/02-icp-language-emotion-proof.md`
- `docs/03-cro-offers-cart-checkout.md`
- `docs/04-site-architecture.md`
- `docs/05-frontend-spec.md`
- `docs/06-backend-spec.md`
- `docs/07-tracking-pixels-capi.md`
- `docs/08-database-sheets-webhooks.md`
- `docs/09-design-system.md`
- `docs/10-coding-rules.md`
- `docs/11-docker-easypanel.md`
- `docs/12-ugc-proof-materials-skills.md`
- CSV files in `docs/templates/`

Build two folders:

- `frontend/`: Next.js + TypeScript + Tailwind Arabic RTL store.
- `backend/`: Python FastAPI + PostgreSQL + Alembic backend.

Brand:

- Arabic name: مطبخ دفا
- English name: Dafa Kitchen
- Domain: `dafakitchen.shop`
- Backend: `api.dafakitchen.shop`
- Market: Saudi Arabia
- Payment: COD only
- DB name: `dafa_kitchen`

Products:

- دفاية الطعام الكهربائية
- حافظة الأرز الذكية مع كوب قياس
- مجموعة تقطيع الخضار الاحترافية

Offers for each product:

- 1 piece: 199 SAR
- 2 pieces: 279 SAR
- 3 pieces: 349 SAR

Critical conversion flow:

1. Product page CTA adds chosen offer to cart.
2. Cart drawer opens.
3. Cart drawer shows cross-sells.
4. Cart checkout CTA opens modal.
5. Modal collects only name and KSA mobile number.
6. Validate KSA phone numbers.
7. After valid form submit, show a 10-15 second upsell for a relevant product at 99 SAR.
8. User accepts or skips.
9. Submit final order to backend.
10. Backend saves PostgreSQL order, sends Google Sheet webhook, and sends Meta/TikTok/Snap CAPI events if configured.
11. Frontend redirects to thank-you page.

Tracking:

- Implement Meta, TikTok, Snapchat web pixels deferred.
- Implement server CAPI behind env flags.
- Use the same `event_id` for browser and server events for deduplication.
- Normalize KSA phones to `+9665XXXXXXXX` for CRM/display and `9665XXXXXXXX` digits-only for Meta/Snap hashing.
- TikTok phone hash mode must be configurable: `TIKTOK_PHONE_HASH_MODE=e164|digits`.
- Never expose CAPI tokens in frontend.

Deliver:

- Working local Docker stack.
- `frontend/.env.example`
- `backend/.env.example`
- Root `docker-compose.yml`
- Backend migrations run on startup.
- Google Apps Script file for Sheets webhook.
- Product pages, home page, collection, about, contact, policies, thank-you.
- Responsive mobile/desktop design.
- Placeholder images where real images are missing.
- Arabic-first premium copy based on docs.

Before final response, run local checks/builds if possible and tell me exactly what passed or failed.
```
