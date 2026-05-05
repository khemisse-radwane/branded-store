# UGC, Proof, Materials, and Skills

## Purpose

This doc tells the coder what content and proof systems the website needs so the brand can sell common dropshipping products at premium prices without looking generic.

## Required Skills For The Build

The coder should behave like a combined:

- Senior Next.js frontend engineer.
- FastAPI backend engineer.
- Arabic RTL ecommerce designer.
- DTC CRO specialist.
- Paid social landing page builder.
- Tracking/CAPI implementation engineer.
- Shopify-style cart and checkout UX builder, even though this is custom code.

## Product Materials / "Ingredients" Matrix

Use "materials" or "المواد" in the UI, not "ingredients", because these are kitchen tools, not consumables.

### دفاية الطعام الكهربائية

Potential material/spec proof blocks:

- سطح سيليكون أو نانو سيليكون قابل للمسح, if supplier confirms.
- تحكم بدرجات الحرارة, if product supports it.
- مؤقت إيقاف, if product supports it.
- إيقاف تلقائي, if product supports it.
- تصميم قابل للطي أو اللف, if product supports it.
- مناسب للأطباق, القدور, والصواني المقاومة للحرارة.

Do not claim:

- Child-safe unless certified/true.
- Fireproof unless documented.
- GCC certified unless documented.

### حافظة الأرز الذكية مع كوب قياس

Potential material/spec proof blocks:

- بلاستيك غذائي PP/PET, if supplier confirms.
- غطاء محكم.
- كوب قياس.
- نافذة شفافة لمتابعة الكمية.
- مناسبة للأرز, الحبوب, العدس, الدقيق, أو طعام الحيوانات.

Do not claim:

- 100% insect-proof unless tested.
- Medical/health protection.

### مجموعة تقطيع الخضار الاحترافية

Potential material/spec proof blocks:

- شفرات ستانلس ستيل, if supplier confirms.
- حافظة/وعاء للتقطيع.
- قطع متعددة للتقطيع والبشر.
- أداة تنظيف.
- واقي لليد, if included.

Do not claim:

- Cannot cut hands.
- Dishwasher safe unless supplier confirms.
- Professional restaurant grade unless documented.

## Social Proof System

Create social proof components that can later be wired to real reviews.

Review fields:

- name.
- city.
- rating.
- product_id.
- review_ar.
- optional image/video placeholder.
- verified flag.

Default review display:

- "عميلة موثقة" only if actually verified.
- Otherwise use "تجربة عميلة" or hide verification badge.

Suggested review card labels:

- الرياض
- جدة
- الدمام
- مكة
- المدينة

## Social Proof Copy Examples

Food warmer:

- "ممتازة للعشاء والعزايم، الأكل بقي دافئ فترة أطول."
- "حبيتها لأنها تنطوي وما تأخذ مساحة."

Rice dispenser:

- "رتبت دولاب المطبخ وخلصت من أكياس الأرز المفتوحة."
- "الكوب سهل علي قياس الأرز بدون فوضى."

Vegetable cutter:

- "وفرت علي وقت السلطة والتجهيز اليومي."
- "أكثر شيء عجبني أن الخضار ينزل مباشرة في الحافظة."

## UGC Requirements

The website must be ready for AI video, UGC, Snapchat, and TikTok traffic.

Each product needs:

- 1 hero demo video slot.
- 3 image slots.
- Before/after visual.
- Short testimonial block.
- FAQ based on objections.

UGC video structure:

1. Hook in first 2 seconds.
2. Problem shot.
3. Product demo.
4. Result shot.
5. COD/trust line.
6. CTA.

## UGC Hooks

Food warmer:

- "إذا الأكل يبرد قبل ما تجتمع العائلة، هذا الحل."
- "للعزايم والرمضان، خلي سفرتك دافئة."

Rice dispenser:

- "أكياس الأرز كانت تخرب ترتيب المطبخ؟"
- "شوفي كيف صار دولاب الأرز أنظف."

Vegetable cutter:

- "تقطيع السلطة كان يأخذ مني وقت، لين جربت هذي."
- "خضار جاهزة في ثواني وبدون فوضى."

## Authority Blocks

Use these sections on product pages:

- **تفاصيل عملية**: dimensions, capacity, included parts.
- **لماذا اخترناه؟**: practical reason Dafa Kitchen selected it.
- **طريقة الاستخدام**: 3-step usage.
- **العناية والتنظيف**: short cleaning instructions.
- **ملاحظات مهمة قبل الطلب**: honest limitations.

Honest limitations increase COD confirmation and reduce refusals.

Examples:

- For warming mat: "يستخدم مع أواني مناسبة للحرارة. لا يترك بدون متابعة لفترات طويلة."
- For rice dispenser: "تأكدي من اختيار مكان مناسب لحجم الحافظة."
- For cutter: "الشفرات حادة، تحفظ بعيدا عن الأطفال."

## Trust Assets

Required trust badges:

- دفع عند الاستلام.
- تأكيد الطلب عبر واتساب.
- دعم عربي.
- استبدال في حال وجود عيب.
- تجهيز الطلبات المؤكدة فقط.

Do not overdo badges. Keep them premium and minimal.

## Media File Naming

Use:

```text
public/images/products/food-warmer/hero.jpg
public/images/products/food-warmer/detail-1.jpg
public/images/products/food-warmer/detail-2.jpg
public/images/products/rice-dispenser/hero.jpg
public/images/products/vegetable-cutter/hero.jpg
```

Alt text must be Arabic and descriptive.

Example:

```text
دفاية الطعام الكهربائية على سفرة عائلية
```
