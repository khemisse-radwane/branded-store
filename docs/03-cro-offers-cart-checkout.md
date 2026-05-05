# CRO, Offers, Cart, Checkout

## Offers

Each product uses the same quantity offer ladder:

- 1 piece: **199 SAR**
- 2 pieces: **279 SAR**
- 3 pieces: **349 SAR**

The discount should feel like a family/household bundle, not a cheap clearance sale.

Arabic labels:

- قطعة واحدة - 199 ريال
- قطعتين - 279 ريال
- 3 قطع - 349 ريال

Badge examples:

- الأكثر طلبا
- وفر 119 ريال
- مناسب للعائلة
- عرض محدود اليوم

## Product Card Requirements

Every product card must include:

- Product image placeholder.
- Arabic product name.
- Benefit headline.
- Short emotional subheading.
- Star rating row.
- Price from 199 SAR.
- Scarcity/trust microcopy.
- CTA.

Example card copy:

Product: دفاية الطعام الكهربائية  
Heading: **خلي سفرتك دافئة وقت العشاء والعزائم**  
Subheading: **تحافظ على حرارة الأطباق وتترتب بسهولة بعد الاستخدام.**  
Rating: 4.8 / 5  
Scarcity: **الكمية محدودة للطلبات المؤكدة اليوم**  
CTA: **اختاري العرض**

## Product Page CTA Behavior

On every product page:

1. User selects offer.
2. CTA adds selected offer to cart.
3. Cart drawer opens immediately.
4. Cart drawer shows cross-sells.
5. User can add cross-sell or continue checkout.

Primary CTA:

**أضيفي العرض للسلة**

Secondary reassurance:

**الدفع عند الاستلام - تأكيد الطلب عبر واتساب**

## Cart Drawer

Cart drawer must include:

- Product summary.
- Offer savings.
- Quantity.
- Remove/edit actions.
- Cross-sell cards.
- Order total.
- Trust row.
- Checkout CTA.

Cart CTA:

**إتمام الطلب**

Trust row:

- دفع عند الاستلام.
- تأكيد عبر واتساب.
- استبدال عند وجود عيب.

## Cross-Sell Rules

Show the other two products in cart as cross-sells.

If cart has warming mat:

- Cross-sell rice dispenser.
- Cross-sell vegetable cutting set.

If cart has rice dispenser:

- Cross-sell warming mat.
- Cross-sell vegetable cutting set.

If cart has vegetable cutting set:

- Cross-sell rice dispenser.
- Cross-sell warming mat.

Cross-sell pricing in cart uses normal product offer logic. The only discounted upsell is after checkout form submit.

## Checkout Popup

The cart CTA opens a modal checkout popup.

Must include:

- Order summary.
- Social proof block.
- Scarcity line.
- Name field.
- Phone field.
- Submit CTA.
- COD reassurance.

Fields:

- `name`: required, min 2 Arabic/English characters.
- `phone`: required, KSA mobile only.

CTA:

**تأكيد الطلب**

Social proof:

**+1200 عميلة وثقوا في مطبخ دفا لتحسين ترتيب وتجهيز المطبخ**

Use this as placeholder/mocked copy until real count exists. In production, make it configurable.

Scarcity:

**نحجز الكمية للطلبات المؤكدة فقط.**

## KSA Phone Validation

Accept these forms:

- `05XXXXXXXX`
- `5XXXXXXXX`
- `9665XXXXXXXX`
- `+9665XXXXXXXX`
- `009665XXXXXXXX`

Normalize internally:

- Display / CRM format: `+9665XXXXXXXX`
- Digits-only format: `9665XXXXXXXX`

Reject:

- Non-Saudi numbers.
- Landlines.
- Fewer or more than expected digits.
- Fake repeated numbers if simple fraud checks catch them, e.g. `0500000000`.

## Post-Submit Upsell

After valid checkout form submit:

1. Do not immediately show thank-you.
2. Show 10-15 second upsell popup.
3. Offer one relevant product for **99 SAR**.
4. This is the only place a product is discounted.
5. User can accept or skip.
6. Then create/confirm final order and route to thank-you page.

Upsell copy:

**عرض خاص قبل تأكيد الطلب**

**أضيفي [product] الآن بـ 99 ريال فقط مع نفس الشحنة.**

Buttons:

- **أضيفيها للطلب**
- **لا شكرا، تأكيد طلبي**

Upsell choice logic:

- If order contains warming mat, upsell rice dispenser.
- If order contains rice dispenser, upsell vegetable cutting set.
- If order contains vegetable cutting set, upsell rice dispenser.
- If order contains all three, show no upsell.

## Thank-You Page

Must include:

- Confirmation headline.
- Order number.
- Summary.
- COD amount.
- WhatsApp confirmation note.
- Delivery expectation.
- Reminder to answer call/WhatsApp.

Arabic copy:

**تم استلام طلبك بنجاح**

**فريق مطبخ دفا بيتواصل معك لتأكيد الطلب قبل الشحن. الرجاء الرد على الاتصال أو رسالة واتساب حتى ما يتأخر طلبك.**

## COD Confirmation CRO

The website must set expectations before order:

- "سيتم تأكيد الطلب قبل الشحن."
- "الطلبات غير المؤكدة قد لا يتم شحنها."
- "الدفع عند الاستلام."

This increases serious orders and reduces RTO.

