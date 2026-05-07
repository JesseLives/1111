# 🛠️ LIQUID SNIPPETS FOR CASELY REDESIGN
## Sense Theme (v15.x) - shellify-5.myshopify.com

---

## 8.1 "NEW" BADGE FOR RECENT PRODUCTS

**File:** `snippets/card-product.liquid`

### Location 1: Lines 127-143 (No-media card badge section)

Find this block:
```liquid
<div class="card__badge {{ settings.badge_position }}">
  {%- if card_product.available == false -%}
    <span
      id="NoMediaStandardBadge-{{ section_id }}-{{ card_product.id }}"
      class="badge badge--bottom-left color-{{ settings.sold_out_badge_color_scheme }}"
    >
      {{- 'products.product.sold_out' | t -}}
    </span>
  {%- elsif card_product.compare_at_price > card_product.price and card_product.available -%}
    <span
      id="NoMediaStandardBadge-{{ section_id }}-{{ card_product.id }}"
      class="badge badge--bottom-left color-{{ settings.sale_badge_color_scheme }}"
    >
      {{- 'products.product.on_sale' | t -}}
    </span>
  {%- endif -%}
</div>
```

Replace with:
```liquid
<div class="card__badge {{ settings.badge_position }}">
  {%- liquid
    assign thirty_days_ago = 'now' | date: '%s' | minus: 2592000
    assign product_date = card_product.created_at | date: '%s'
    assign is_new = false
    if product_date > thirty_days_ago
      assign is_new = true
    endif
  -%}
  
  {%- if card_product.available == false -%}
    <span
      id="NoMediaStandardBadge-{{ section_id }}-{{ card_product.id }}"
      class="badge badge--bottom-left color-{{ settings.sold_out_badge_color_scheme }}"
    >
      {{- 'products.product.sold_out' | t -}}
    </span>
  {%- elsif card_product.compare_at_price > card_product.price and card_product.available -%}
    <span
      id="NoMediaStandardBadge-{{ section_id }}-{{ card_product.id }}"
      class="badge badge--bottom-left color-{{ settings.sale_badge_color_scheme }}"
    >
      {{- 'products.product.on_sale' | t -}}
    </span>
  {%- elsif is_new -%}
    <span
      id="NoMediaStandardBadge-{{ section_id }}-{{ card_product.id }}"
      class="badge badge--new"
    >
      NEW
    </span>
  {%- endif -%}
</div>
```

### Location 2: Lines 551-567 (Standard card badge section)

Find this block:
```liquid
<div class="card__badge {{ settings.badge_position }}">
  {%- if card_product.available == false -%}
    <span
      id="Badge-{{ section_id }}-{{ card_product.id }}"
      class="badge badge--bottom-left color-{{ settings.sold_out_badge_color_scheme }}"
    >
      {{- 'products.product.sold_out' | t -}}
    </span>
  {%- elsif card_product.compare_at_price > card_product.price and card_product.available -%}
    <span
      id="Badge-{{ section_id }}-{{ card_product.id }}"
      class="badge badge--bottom-left color-{{ settings.sale_badge_color_scheme }}"
    >
      {{- 'products.product.on_sale' | t -}}
    </span>
  {%- endif -%}
</div>
```

Replace with:
```liquid
<div class="card__badge {{ settings.badge_position }}">
  {%- liquid
    assign thirty_days_ago = 'now' | date: '%s' | minus: 2592000
    assign product_date = card_product.created_at | date: '%s'
    assign is_new = false
    if product_date > thirty_days_ago
      assign is_new = true
    endif
  -%}
  
  {%- if card_product.available == false -%}
    <span
      id="Badge-{{ section_id }}-{{ card_product.id }}"
      class="badge badge--bottom-left color-{{ settings.sold_out_badge_color_scheme }}"
    >
      {{- 'products.product.sold_out' | t -}}
    </span>
  {%- elsif card_product.compare_at_price > card_product.price and card_product.available -%}
    <span
      id="Badge-{{ section_id }}-{{ card_product.id }}"
      class="badge badge--bottom-left color-{{ settings.sale_badge_color_scheme }}"
    >
      {{- 'products.product.on_sale' | t -}}
    </span>
  {%- elsif is_new -%}
    <span
      id="Badge-{{ section_id }}-{{ card_product.id }}"
      class="badge badge--new"
    >
      NEW
    </span>
  {%- endif -%}
</div>
```

---

## 8.2 LOW STOCK URGENCY ON PDP

**File:** `sections/main-product.liquid`

Find the buy_buttons block render (around line 480):
```liquid
{%- when 'buy_buttons' -%}
  {%- render 'buy-buttons',
    block: block,
    product: product,
    product_form_id: product_form_id,
    section_id: section.id,
    show_pickup_availability: true
  -%}
```

Add this code IMMEDIATELY BEFORE the buy_buttons block (after the variant_picker block ends):

```liquid
{%- when 'inventory_warning' -%}
  {%- liquid
    assign inv = product.selected_or_first_available_variant
    if inv.inventory_management == 'shopify' and inv.inventory_quantity > 0 and inv.inventory_quantity <= 5
      echo '<p class="inventory-warning">'
      echo '⚠️ Only '
      echo inv.inventory_quantity
      echo ' left in stock — order soon!'
      echo '</p>'
    endif
  -%}
```

Then add this to the schema blocks list (near end of file, around line 860+):

```liquid
{
  "type": "inventory_warning",
  "name": "Inventory Warning",
  "limit": 1
},
```

**Alternative inline approach** (if you don't want to add a new block type):

Find where quantity_selector is rendered and add after it:

```liquid
{%- liquid
  assign inv = product.selected_or_first_available_variant
  if inv.inventory_management == 'shopify' and inv.inventory_quantity > 0 and inv.inventory_quantity <= 5
    echo '<p class="inventory-warning">⚠️ Only '
    echo inv.inventory_quantity
    echo ' left in stock — order soon!</p>'
  endif
-%}
```

---

## 8.3 FREE SHIPPING BAR IN CART DRAWER

**File:** `snippets/cart-drawer.liquid`

Find line 560 (before the `<!-- CTAs -->` comment):

```liquid
</div>

<!-- CTAs -->
```

Insert this code:

```liquid
<!-- Free Shipping Progress Bar -->
{%- assign threshold = 3500 -%}
{%- assign remaining = threshold | minus: cart.total_price -%}
{%- assign percent_complete = cart.total_price | times: 100.0 | divided_by: threshold | round -%}

{%- if cart.item_count > 0 -%}
  <div class="free-shipping-bar">
    {%- if remaining > 0 -%}
      <p>Add <strong>{{ remaining | money }}</strong> more for <strong>FREE SHIPPING</strong> 🚚</p>
      <div class="free-shipping-bar__track">
        <div class="free-shipping-bar__fill" style="width: {{ percent_complete }}%"></div>
      </div>
    {%- else -%}
      <p class="free-shipping-bar--achieved">🎉 You've unlocked FREE SHIPPING!</p>
    {%- endif -%}
  </div>
{%- endif -%}
<!-- End Free Shipping Progress Bar -->
```

---

## 8.4 TRUST BADGES IN CART DRAWER

**File:** `snippets/cart-drawer.liquid`

Find the checkout button section (lines 564-577):

```liquid
<div class="cart__ctas" {{ block.shopify_attributes }}>
  <button
    type="submit"
    id="CartDrawer-Checkout"
    class="cart__checkout-button button"
    name="checkout"
    form="CartDrawer-Form"
    {% if cart == empty %}
      disabled
    {% endif %}
  >
    {{ 'sections.cart.checkout' | t }}
  </button>
</div>
```

Insert this code INSIDE the `.cart__ctas` div, BEFORE the checkout button:

```liquid
<div style="display:flex;gap:10px;justify-content:center;padding:10px 0 14px;font-size:11px;color:#aaa;letter-spacing:0.04em;text-transform:uppercase;">
  <span>🔒 Secure</span>
  <span>·</span>
  <span>🚚 Free $35+</span>
  <span>·</span>
  <span>🔄 Easy Returns</span>
</div>
```

Full result:
```liquid
<div class="cart__ctas" {{ block.shopify_attributes }}>
  <div style="display:flex;gap:10px;justify-content:center;padding:10px 0 14px;font-size:11px;color:#aaa;letter-spacing:0.04em;text-transform:uppercase;">
    <span>🔒 Secure</span>
    <span>·</span>
    <span>🚚 Free $35+</span>
    <span>·</span>
    <span>🔄 Easy Returns</span>
  </div>
  <button
    type="submit"
    id="CartDrawer-Checkout"
    class="cart__checkout-button button"
    name="checkout"
    form="CartDrawer-Form"
    {% if cart == empty %}
      disabled
    {% endif %}
  >
    {{ 'sections.cart.checkout' | t }}
  </button>
</div>
```

---

## 8.5 TRUST BADGES ON PRODUCT PAGE

**File:** `sections/main-product.liquid`

Add a new Custom Liquid block type or insert after buy_buttons.

Find the buy_buttons block (line 480) and add this immediately after it:

```liquid
{%- when '@app' -%}
  {% render block %}
{%- when 'trust_badges' -%}
  <div class="trust-badges">
    <span>🔒 Secure Checkout</span>
    <span>🚚 Free Shipping $35+</span>
    <span>🔄 Easy 30-Day Returns</span>
    <span>⭐ 1-Year Warranty</span>
  </div>
```

Then add to schema blocks list:

```liquid
{
  "type": "trust_badges",
  "name": "Trust Badges",
  "limit": 1
},
```

---

## 8.6 UPDATED HOMEPAGE TEMPLATE (index.json)

**File:** `templates/index.json`

Replace entire contents with:

```json
{
  "sections": {
    "announcement-bar": {
      "type": "announcement-bar",
      "settings": {
        "color_scheme": "scheme-2",
        "text": "🎉 FREE SHIPPING ON ORDERS OVER $35  |  SHOP NEW ARRIVALS →",
        "link": "/collections/new-arrivals",
        "auto_rotate": false
      }
    },
    "image-banner": {
      "type": "image-banner",
      "blocks": {
        "heading": {
          "type": "heading",
          "settings": {
            "heading": "PROTECT YOUR STYLE",
            "heading_size": "h1"
          }
        },
        "text": {
          "type": "text",
          "settings": {
            "text": "Premium phone cases designed for the bold."
          }
        },
        "buttons": {
          "type": "buttons",
          "settings": {
            "button_label_1": "SHOP NOW",
            "button_link_1": "/collections/all",
            "button_style_1": "primary",
            "button_label_2": "NEW ARRIVALS",
            "button_link_2": "/collections/new-arrivals",
            "button_style_2": "secondary"
          }
        }
      },
      "block_order": ["heading", "text", "buttons"],
      "settings": {
        "image_height": "large",
        "desktop_image_width": "wide",
        "desktop_content_position": "middle-left",
        "color_scheme": "scheme-2",
        "full_width": true
      }
    },
    "multicolumn-usp": {
      "type": "multicolumn",
      "blocks": {
        "column-1": {
          "type": "column",
          "settings": {
            "image": "icon-truck.svg",
            "title": "Free Shipping Over $35"
          }
        },
        "column-2": {
          "type": "column",
          "settings": {
            "image": "icon-return.svg",
            "title": "30-Day Easy Returns"
          }
        },
        "column-3": {
          "type": "column",
          "settings": {
            "image": "icon-padlock.svg",
            "title": "1-Year Warranty"
          }
        },
        "column-4": {
          "type": "column",
          "settings": {
            "image": "icon-star.svg",
            "title": "50,000+ Happy Customers"
          }
        }
      },
      "block_order": ["column-1", "column-2", "column-3", "column-4"],
      "settings": {
        "color_scheme": "scheme-3",
        "columns_desktop": 4,
        "columns_mobile": 2,
        "image_width": "small"
      }
    },
    "collection-list": {
      "type": "collection-list",
      "settings": {
        "title": "SHOP BY COLLECTION",
        "color_scheme": "scheme-1",
        "columns_desktop": 3
      }
    },
    "featured-collection": {
      "type": "featured-collection",
      "settings": {
        "title": "BEST SELLERS",
        "description": "The styles everyone is loving right now.",
        "collection": "all",
        "products_to_show": 8,
        "columns_desktop": 4,
        "color_scheme": "scheme-1",
        "image_ratio": "square",
        "show_secondary_image": true,
        "quick_add": "button",
        "show_rating": true
      }
    },
    "image-with-text": {
      "type": "image-with-text",
      "blocks": {
        "heading": {
          "type": "heading",
          "settings": {
            "heading": "MADE FOR THE BOLD",
            "heading_size": "h2"
          }
        },
        "text": {
          "type": "text",
          "settings": {
            "text": "<p>We design premium phone cases that combine protection with style. Every case is crafted with high-quality materials and tested to withstand daily wear while keeping your device looking fresh.</p>"
          }
        },
        "button": {
          "type": "button",
          "settings": {
            "button_label": "Our Story",
            "button_link": "/pages/about",
            "button_style_secondary": true
          }
        }
      },
      "block_order": ["heading", "text", "button"],
      "settings": {
        "layout": "image_first",
        "color_scheme": "scheme-1",
        "image_height": "medium"
      }
    },
    "multicolumn-testimonials": {
      "type": "multicolumn",
      "blocks": {
        "column-1": {
          "type": "column",
          "settings": {
            "title": "⭐⭐⭐⭐⭐",
            "text": "\"Best case I've ever owned. Dropped my phone countless times and not a scratch!\"",
            "link_label": "— Sarah M., NY"
          }
        },
        "column-2": {
          "type": "column",
          "settings": {
            "title": "⭐⭐⭐⭐⭐",
            "text": "\"The quality is insane. Looks exactly like the photos and feels premium.\"",
            "link_label": "— Jake T., CA"
          }
        },
        "column-3": {
          "type": "column",
          "settings": {
            "title": "⭐⭐⭐⭐⭐",
            "text": "\"Fast shipping and amazing customer service. Will definitely buy again!\"",
            "link_label": "— Emily R., TX"
          }
        }
      },
      "block_order": ["column-1", "column-2", "column-3"],
      "settings": {
        "color_scheme": "scheme-3",
        "columns_desktop": 3,
        "columns_mobile": 1
      }
    },
    "logo-list": {
      "type": "logo-list",
      "settings": {
        "title": "AS SEEN IN",
        "color_scheme": "scheme-1"
      }
    },
    "email-signup-banner": {
      "type": "email-signup-banner",
      "blocks": {
        "heading": {
          "type": "heading",
          "settings": {
            "heading": "JOIN THE COMMUNITY"
          }
        },
        "paragraph": {
          "type": "paragraph",
          "settings": {
            "text": "Get 15% off your first order + early access to new drops."
          }
        }
      },
      "block_order": ["heading", "paragraph"],
      "settings": {
        "color_scheme": "scheme-2",
        "button_label": "GET 15% OFF"
      }
    }
  },
  "order": [
    "announcement-bar",
    "image-banner",
    "multicolumn-usp",
    "collection-list",
    "featured-collection",
    "image-with-text",
    "multicolumn-testimonials",
    "logo-list",
    "email-signup-banner"
  ]
}
```

---

## 8.7 UPDATED COLLECTION TEMPLATE

**File:** `templates/collection.json`

Replace contents with:

```json
{
  "sections": {
    "main-collection-banner": {
      "type": "main-collection-banner",
      "settings": {
        "show_collection_description": true,
        "show_collection_image": true,
        "color_scheme": "scheme-1"
      }
    },
    "main-collection-product-grid": {
      "type": "main-collection-product-grid",
      "settings": {
        "products_per_page": 20,
        "columns_desktop": 4,
        "columns_mobile": "2",
        "color_scheme": "scheme-1",
        "image_ratio": "square",
        "show_secondary_image": true,
        "show_vendor": false,
        "show_rating": true,
        "quick_add": "button",
        "enable_filtering": true,
        "filter_type": "sidebar",
        "enable_sorting": true,
        "padding_top": 0,
        "padding_bottom": 68
      }
    }
  },
  "order": [
    "main-collection-banner",
    "main-collection-product-grid"
  ]
}
```

---

## 8.8 UPDATED PRODUCT TEMPLATE

**File:** `templates/product.json`

Replace contents with:

```json
{
  "sections": {
    "main": {
      "type": "main-product",
      "blocks": {
        "title": {
          "type": "title",
          "settings": {}
        },
        "price": {
          "type": "price",
          "settings": {}
        },
        "rating": {
          "type": "rating",
          "settings": {}
        },
        "variant_picker": {
          "type": "variant_picker",
          "settings": {
            "picker_type": "button",
            "swatch_shape": "circle"
          }
        },
        "quantity_selector": {
          "type": "quantity_selector",
          "settings": {}
        },
        "buy_buttons": {
          "type": "buy_buttons",
          "settings": {
            "show_dynamic_checkout": true,
            "show_gift_card_recipient": true
          }
        },
        "trust_badges": {
          "type": "custom_liquid",
          "settings": {
            "code": "<div class=\"trust-badges\"><span>🔒 Secure Checkout</span><span>🚚 Free Shipping $35+</span><span>🔄 Easy 30-Day Returns</span><span>⭐ 1-Year Warranty</span></div>"
          }
        },
        "collapsible_details": {
          "type": "collapsible_content",
          "settings": {
            "heading": "Product Details",
            "content": "<p>Premium materials meet precision engineering. Our cases are designed to provide maximum protection without the bulk.</p>",
            "layout": "accordion"
          }
        },
        "collapsible_shipping": {
          "type": "collapsible_content",
          "settings": {
            "heading": "Shipping & Returns",
            "content": "<p>Free shipping on orders over $35. 30-day hassle-free returns.</p>",
            "layout": "accordion"
          }
        },
        "collapsible_materials": {
          "type": "collapsible_content",
          "settings": {
            "heading": "Materials & Care",
            "content": "<p>Made from high-quality polycarbonate and TPU. Clean with mild soap and water.</p>",
            "layout": "accordion"
          }
        }
      },
      "block_order": [
        "title",
        "price",
        "rating",
        "variant_picker",
        "quantity_selector",
        "buy_buttons",
        "trust_badges",
        "collapsible_details",
        "collapsible_shipping",
        "collapsible_materials"
      ],
      "settings": {
        "enable_sticky_info": true,
        "color_scheme": "scheme-1",
        "media_size": "medium",
        "constrain_to_viewport": true,
        "media_fit": "contain",
        "gallery_layout": "stacked",
        "mobile_thumbnails": "hide",
        "media_position": "left",
        "image_zoom": "lightbox",
        "hide_variants": false,
        "enable_video_looping": false,
        "padding_top": 24,
        "padding_bottom": 24
      }
    },
    "related-products": {
      "type": "related-products",
      "settings": {
        "heading": "YOU MAY ALSO LIKE",
        "heading_size": "h2",
        "products_to_show": 4,
        "columns_desktop": 4,
        "columns_mobile": "2",
        "color_scheme": "scheme-1",
        "image_ratio": "square",
        "show_secondary_image": true,
        "show_vendor": false,
        "show_rating": true,
        "quick_add": "button",
        "padding_top": 36,
        "padding_bottom": 68
      }
    }
  },
  "order": [
    "main",
    "related-products"
  ]
}
```

---

*End of Liquid Snippets*
