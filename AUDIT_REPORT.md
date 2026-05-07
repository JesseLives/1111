# 🛍️ SHELLIFY STORE AUDIT & REDESIGN REPORT
## Target: getcasely.com aesthetic · Theme: Sense (Free, v15.x) · Date: Generated from theme export

---

## PHASE 1 — COMPLETE STORE GAP AUDIT

### 1.1 Sense-Specific Overrides Needed (CRITICAL - Must Fix First)

| Issue | Sense Default (Current) | Casely Target | Status |
|---|---|---|---|
| Background gradients | `linear-gradient(180deg, rgba(240, 244, 236, 1), rgba(241, 235, 226, 1) 100%)` | Flat solid white `#FFFFFF` | ❌ NEEDS FIX |
| Button border-radius | `10px` (pill-ish shape) | `4px` (slightly rounded) | ❌ NEEDS FIX |
| Card border-radius | `12px` (very rounded) | `6px` (subtly rounded) | ❌ NEEDS FIX |
| Badge border-radius | `20px` (pill) | `3-4px` (sharp) | ❌ NEEDS FIX |
| Color palette | Pastel health/beauty tones (sage, cream, mauve) | Bold, high-contrast brand colors | ❌ NEEDS FIX |
| Typography weight | Poppins (medium weight, soft) | Barlow Condensed/Oswald Bold (700-900) | ❌ NEEDS FIX |
| Section background | Gradient blends between sections | Clean solid color blocks | ❌ NEEDS FIX |
| Page width | `1200px` | `1400px` | ❌ NEEDS FIX |
| Spacing sections | `36px` | `60-80px` desktop | ❌ NEEDS FIX |

### 1.2 Global Settings Audit

- ❌ **Color schemes**: Currently using gradient backgrounds (scheme-1 has `background_gradient`)
- ❌ **Heading font**: Using Poppins (too soft) — need Barlow Condensed or Oswald
- ❌ **Button border-radius**: Set to 10px — needs to be 4px
- ❌ **Card corner radius**: Set to 12px — needs to be 6px
- ❌ **Page width**: Set to 1200px — needs to be 1400px
- ❌ **Section spacing**: Set to 36px — needs to be 60px minimum

### 1.3 Header Audit

Based on current `sections/header.liquid`:
- ✅ Sticky header: Supported natively
- ✅ Transparent header: Supported natively
- ✅ Cart icon with count: Present
- ✅ Search icon: Present
- ⚠️ **Announcement bar**: Need to verify content/messaging
- ⚠️ **Navigation**: Need to build proper menu structure

### 1.4 Homepage Sections Audit (Current: index.json)

| # | Section | Type Needed | Current Status |
|---|---|---|---|
| 1 | Announcement bar | Announcement bar | ❌ MISSING |
| 2 | Hero | Image banner | ❌ MISSING (has image-with-text instead) |
| 3 | USP bar | Multicolumn (4-col) | ❌ MISSING |
| 4 | Collections grid | Collection list | ❌ MISSING |
| 5 | Best sellers | Featured collection | ⚠️ EXISTS but only shows 4 products |
| 6 | Brand story | Image with text | ⚠️ EXISTS but wrong layout |
| 7 | Testimonials | Multicolumn | ❌ MISSING |
| 8 | Press logos | Logo list | ❌ MISSING |
| 9 | Newsletter | Email signup | ❌ MISSING |
| 10 | Footer | Footer | ✅ EXISTS |

**Current homepage only has 2 sections** — needs complete rebuild with 10+ sections.

### 1.5 Collection Page Audit (collection.json)

- ✅ 4-column grid desktop: YES (`columns_desktop: 4`)
- ✅ 2-column mobile: YES (`columns_mobile: "2"`)
- ✅ Second image on hover: YES (`show_secondary_image: true`)
- ❌ Color swatches under card: NO (needs custom CSS/Liquid)
- ❌ Quick Add button: NO (`quick_add: "none"`)
- ✅ Filter sidebar present: YES (`enable_filtering: true`) but uses horizontal layout
- ✅ Sort dropdown present: YES (`enable_sorting: true`)

**Issues:**
- Filter type is `horizontal` — should be `sidebar` for better UX
- Quick add is disabled — should enable for conversion
- Image ratio is `adapt` — should be `square` for consistency

### 1.6 Product Page (PDP) Audit (product.json)

Current blocks order: vendor → title → price → variant_picker → quantity_selector → buy_buttons → description → share

- ✅ Image left, info right: YES (`media_position: "left"`)
- ✅ Image zoom enabled: YES (`image_zoom: "lightbox"`)
- ⚠️ Variant picker: Uses button style but NOT pills — needs CSS override
- ❌ ATC button: Not full-width, not bold brand color
- ❌ Trust badges below ATC: MISSING
- ❌ Accordion tabs for product info: MISSING (description is plain text)
- ❌ Reviews section: MISSING (needs Judge.me app)
- ✅ Related products carousel: YES (`related-products` section exists)

**Missing blocks that should be added:**
- Rating block (requires reviews app)
- Collapsible rows for product details
- Custom Liquid for trust badges
- Inventory urgency warning

### 1.7 Cart Audit (cart.json)

Current cart type in settings: `"cart_type": "notification"`

- ❌ Slide-out drawer: NOT ENABLED (uses notification instead)
- ❌ Free shipping progress bar: MISSING
- ❌ Trust badges in cart: MISSING

**Note:** The `snippets/cart-drawer.liquid` file exists but isn't being used. Need to change `cart_type` to `drawer`.

---

## PHASE 2 — SENSE THEME SETTINGS CONFIGURATION

### 2.1 Colors — Override All Gradient Defaults

**CRITICAL:** Current settings use gradients. Must change to solid colors.

**Recommended Color Scheme Updates** (edit `config/settings_data.json`):

```json
"color_schemes": {
  "scheme-1": {
    "settings": {
      "background": "#FFFFFF",
      "background_gradient": "",
      "text": "#111111",
      "button": "#E8303A",
      "button_label": "#FFFFFF",
      "secondary_button_label": "#111111",
      "shadow": "#111111"
    }
  },
  "scheme-2": {
    "settings": {
      "background": "#111111",
      "background_gradient": "",
      "text": "#FFFFFF",
      "button": "#FFFFFF",
      "button_label": "#111111",
      "secondary_button_label": "#FFFFFF",
      "shadow": "#333333"
    }
  },
  "scheme-3": {
    "settings": {
      "background": "#F5F5F5",
      "background_gradient": "",
      "text": "#111111",
      "button": "#111111",
      "button_label": "#FFFFFF",
      "secondary_button_label": "#111111",
      "shadow": "#CCCCCC"
    }
  }
}
```

### 2.2 Typography

**Current:** Poppins (too soft/delicate for Casely aesthetic)

**Recommended changes in Theme Settings:**
```
Heading font:   Barlow Condensed Bold (700) or Oswald Bold
Body font:      DM Sans or Jost
Heading scale:  130-140% (increase from current 120%)
Body size:      16px (current appears to be 100% = ~16px, OK)
```

### 2.3 Buttons

**Current:**
```json
"buttons_border_thickness": 1,
"buttons_border_opacity": 55,
"buttons_radius": 10,
```

**Needed:**
```json
"buttons_radius": 4,
"buttons_border_thickness": 2,
```

### 2.4 Cards

**Current:**
```json
"card_style": "card",
"card_image_padding": 0,
"card_text_alignment": "center",
"card_color_scheme": "scheme-1",
"card_corner_radius": 12,
```

**Needed:**
```json
"card_corner_radius": 6,
"card_text_alignment": "left",
```

### 2.5 Layout

**Current:**
```json
"page_width": 1200,
"spacing_sections": 36,
```

**Needed:**
```json
"page_width": 1400,
"spacing_sections": 60,
```

---

## PHASE 3 — HOMEPAGE REBUILD PRIORITY

### Current State:
Only 2 sections exist in `templates/index.json`:
1. image-with-text (basic)
2. featured-collection (4 products)

### Required Sections (in order):

1. **Announcement Bar** — NEW
2. **Header** — Edit existing
3. **Image Banner (Hero)** — NEW (replace image-with-text)
4. **Multicolumn (USP Bar)** — NEW
5. **Collection List** — NEW
6. **Featured Collection (Best Sellers)** — EXPAND (show 8 products)
7. **Image with Text (Brand Story)** — REORDER/EDIT
8. **Multicolumn (Testimonials)** — NEW
9. **Logo List (Press)** — NEW
10. **Email Signup** — NEW
11. **Footer** — Edit existing

---

## PHASE 4 — NAVIGATION BUILD

**Current state:** No menu structure defined in theme files (managed in Shopify admin).

**Required menu structure:**
```
Main Menu:
├── NEW ✨             →  /collections/new-arrivals
├── SHOP BY DEVICE    →  (no URL — parent only)
│   ├── iPhone 16     →  /collections/iphone-16
│   ├── iPhone 15     →  /collections/iphone-15
│   ├── Samsung S24   →  /collections/samsung-s24
│   └── View All      →  /collections/all
├── COLLECTIONS       →  (no URL — parent only)
│   ├── Best Sellers  →  /collections/best-sellers
│   ├── Trending      →  /collections/trending
│   └── Limited Ed.   →  /collections/limited-edition
├── SALE 🔥           →  /collections/sale
└── ABOUT             →  /pages/about
```

---

## PHASE 5 — PRODUCT PAGE REBUILD

### Current Block Order:
vendor → title → price → variant_picker → quantity_selector → buy_buttons → description → share

### Required Block Order:
1. Title
2. Price
3. Star rating (NEW — requires Judge.me)
4. Variant picker (change to pills style via CSS)
5. Quantity selector
6. Buy buttons (make full-width, bold via CSS)
7. Custom Liquid (NEW — trust badges)
8. Collapsible row (NEW — "Product Details")
9. Collapsible row (NEW — "Shipping & Returns")
10. Collapsible row (NEW — "Materials & Care")
11. Description (move to collapsible)
12. Share (optional, move to bottom)

---

## PHASE 6 — COLLECTION PAGE SETUP

### Current Settings:
```json
"filter_type": "horizontal",
"quick_add": "none",
"image_ratio": "adapt",
```

### Required Changes:
```json
"filter_type": "sidebar",
"quick_add": "button",
"image_ratio": "square",
```

---

## PHASE 7 — CUSTOM CSS REQUIREMENTS

The following CSS overrides are CRITICAL and must be added to `assets/base.css`:

### Priority 🔴 HIGH (Do First):
1. Kill Sense's default gradients → solid colors
2. Override button border-radius (10px → 4px)
3. Override card corner-radius (12px → 6px)
4. Override badge border-radius (20px → 3px)
5. Bold typography overrides (headings uppercase, heavy weight)
6. Primary CTA button styling (brand color)
7. ATC button full-width styling

### Priority 🟡 MEDIUM (Do Second):
8. Product card hover effects
9. Collection card gradient overlays
10. Navigation link styling (uppercase, bold)
11. Sale badge styling
12. Quick add button styling
13. Trust badges styling
14. Free shipping bar styling

### Priority 🟢 LOW (Do Last):
15. Press logo grayscale hover effects
16. Rating star colors
17. Inventory warning styling
18. Footer heading styling
19. Mobile-specific overrides

---

## PHASE 8 — LIQUID TEMPLATE TWEAKS REQUIRED

### 8.1 "NEW" Badge for Recent Products
**File:** `snippets/card-product.liquid`
**Lines:** 127-143 and 551-567
**Action:** Add logic to check if product was created in last 30 days

### 8.2 Low Stock Urgency on PDP
**File:** `sections/main-product.liquid`
**Location:** Before buy_buttons block
**Action:** Add inventory quantity check and warning message

### 8.3 Free Shipping Bar in Cart Drawer
**File:** `snippets/cart-drawer.liquid`
**Lines:** 560-564 (before `.cart__ctas` div)
**Action:** Add free shipping progress bar logic

### 8.4 Trust Badges in Cart Drawer
**File:** `snippets/cart-drawer.liquid`
**Lines:** 576-577 (inside `.cart__ctas`, before checkout button)
**Action:** Add trust badge icons/text

---

## PHASE 9 — CONVERSION FEATURES (Apps Required)

### Required Apps:
1. **Judge.me Product Reviews** (FREE) — For star ratings on PDP and collection cards
2. **Klaviyo** (FREE up to 250 contacts) — Email marketing, welcome flows, abandoned cart
3. **Privy** (FREE tier) — Exit intent popup for email capture

### Optional Apps:
4. **Free Shipping Bar by Hextom** (FREE) — Alternative to Liquid snippet for shipping bar
5. **Nudgify/Fomo** (FREEMIUM) — Social proof popups ("X people viewing this")

---

## PHASE 10 — MOBILE-FIRST QA CHECKLIST

Test on real devices at these breakpoints:
- iPhone Safari: 375px
- Android Chrome: 360px
- iPad: 768px
- Desktop: 1440px

### Critical Mobile Tests:
- [ ] Hero headline legible, not clipped
- [ ] Hamburger menu opens smooth full-screen overlay
- [ ] Product grid: 2-up (NOT 1-up)
- [ ] PDP images: full-width, swipe works
- [ ] ATC button: sticky at bottom on scroll
- [ ] Cart: slides in from right without shifting page
- [ ] Apple Pay / Google Pay: visible below ATC
- [ ] All body text: minimum 14px
- [ ] All tap targets: minimum 44×44px
- [ ] Form inputs: don't zoom on focus (need 16px font-size)

---

## PHASE 11 — SEO & PERFORMANCE

### SEO Checklist:
- [ ] Homepage `<title>`: Include primary keyword + brand
- [ ] Homepage meta description: 140-160 chars
- [ ] All product image `alt` text: descriptive
- [ ] Collections: written SEO descriptions
- [ ] Submit sitemap.xml to Google Search Console

### Performance Checklist:
- [ ] All images: WebP format, compressed
- [ ] Hero image: max 1800×900px, < 250KB
- [ ] Product card images: max 800×800px, < 100KB
- [ ] Run Google PageSpeed Insights — target 90+ desktop, 70+ mobile

---

## LIMITATIONS FLAG — What Sense Can't Do Without Paid Theme/App

| Feature | Casely Has | Sense Limitation | Workaround |
|---|---|---|---|
| Advanced mega menu | Multi-column with images | Basic dropdown only | Use native mega menu, keep simple |
| Advanced filtering | Filter by case style, color, material | Only basic filters | Use Shopify's native filters (adequate) |
| Bundle builder | Build-your-own bundles | Not native | Use app like Bundler (paid) |
| Quiz funnel | "Find your case" quiz | Not native | Use Octane AI or similar (paid) |
| Advanced PDP tabs | Custom tabbed layout | Basic accordions only | Use collapsible rows (acceptable) |
| Sticky ATC on mobile | Always visible | Requires custom code | CSS sticky positioning (included in Phase 7) |
| Advanced urgency | Live visitor counters | Not native | Use app like Nudgify (paid) |

**Conclusion:** Sense can replicate 85-90% of Casely's core experience with custom CSS + minor Liquid edits. The remaining 10-15% requires paid apps or is non-essential.

---

## PRIORITY STACK — Implementation Order

### 🔴 HIGH PRIORITY (Day 1-2):
1. Update `config/settings_data.json` with solid colors, correct radii, page width
2. Add ALL custom CSS to `assets/base.css` (Phase 7 styles)
3. Rebuild homepage sections in correct order
4. Change cart type from "notification" to "drawer"
5. Enable quick add on collection pages
6. Change filter type to sidebar

### 🟡 MEDIUM PRIORITY (Day 3-4):
7. Add Liquid snippets for NEW badge, low stock warning
8. Add free shipping bar to cart drawer
9. Add trust badges to PDP and cart
10. Install Judge.me for reviews
11. Build navigation menu structure
12. Add collapsible rows to PDP

### 🟢 LOW PRIORITY (Day 5+):
13. Install Klaviyo for email marketing
14. Install Privy for exit intent popup
15. Fine-tune mobile CSS
16. Optimize all images for performance
17. Write SEO meta descriptions
18. Submit sitemap to Google

---

*End of Audit Report*
