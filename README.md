# Daisycon Conversion Tag

This is Daisycon's official conversion tag template for Google Tag Manager (GTM). It runs in a web container and sends conversion data from an advertiser's order confirmation page to Daisycon.

The tag supports both basket-level and product-level conversion tracking.

## What it does

- **Tracks conversions:** Sends campaign and transaction data from the browser to Daisycon.
- **Supports product-level tracking:** Builds Daisycon product lines from GA4 ecommerce data, UA Enhanced Ecommerce data, or a Daisycon product array.
- **Supports flexible commission codes:** Uses product categories, another product property, or a tag-level commission code.
- **Handles incomplete product data:** Either fails the tag or sends a basket-level fallback conversion, depending on the selected fallback behavior.

## Tracking modes

### Standard transaction tracking

Sends one basket-level conversion using the transaction values configured in the tag.

### Product-level tracking

Product-level tracking supports three data sources:

- **GA4 ecommerce data:** Reads products from `ecommerce.items` in the data layer.
- **UA Enhanced Ecommerce data:** Reads products from `ecommerce.purchase.products` in the data layer.
- **Daisycon product array:** Reads a GTM variable containing product objects with the supported keys `a`, `r`, `qty`, `sku`, `cc`, `pn`, `iv`, and `e1` through `e5`.

For GA4 and UA data, the tag reads the transaction ID, currency, and coupon from the data layer when available and falls back to the corresponding configured tag values.

Each valid product is sent as a Daisycon `p[]` entry. A product-level conversion requires a campaign ID, transaction ID, and at least one valid product.

### GA4 price and tax

For GA4 products, `items[].price` is treated as the unit product price and multiplied by `quantity`. Supply the price excluding tax. GA4's transaction-level `tax` field is not allocated across products or used to calculate Daisycon product amounts.

If you need explicit control over the Daisycon values, you can add `daisycon_a` or `a` for the product amount and `daisycon_r` or `r` for the product revenue. These values take precedence over the calculated `price × quantity` amount.

### Product commission codes

Commission codes can come from each product's standard category or commission-code field, from another product property selected in the tag, or from the tag-level commission code for every product. The tag-level value is also used as a fallback when a product-level value is empty.

### Fallback behavior

When product-level tracking cannot build any valid products, choose whether the tag should:

- fail without sending a conversion; or
- send a standard basket-level conversion instead.

Fallback conversions include a marker in the advertiser description so they can be identified downstream.

## Setup and configuration

Use the following guide to configure the Daisycon Conversion Tag:

- [How to implement the Daisycon conversion pixel in Google Tag Manager](https://faq-advertiser.daisycon.com/hc/en-us/articles/360019494034)
