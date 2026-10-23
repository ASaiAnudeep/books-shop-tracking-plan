# books-shop-tracking-plan

Rudder CLI tracking plan + data catalog for **Leaf & Lantern Books**.

This repo is the declared analytics contract for:

| Repo | Role |
|------|------|
| [`ASaiAnudeep/books-shop`](https://github.com/ASaiAnudeep/books-shop) | Next.js storefront (`analytics/schemas/*.json`, `analytics/track.ts`) |
| [`ASaiAnudeep/books-shop-dbt`](https://github.com/ASaiAnudeep/books-shop-dbt) | Warehouse models over the same funnel events |
| this repo | rudder-cli YAML for Lookout Context Hub / tracking-plan tooling |

## Funnel events

| Event | Type | Category | Required properties |
|-------|------|----------|---------------------|
| `page_view` | page | browsing | `path`, `source`, `pageName` |
| `product_viewed` | track | browsing | `path`, `source`, `productId`, `title`, `genre`, `price` |
| `cart_item_added` | track | cart | `path`, `source`, `productId`, `title`, `quantity`, `price` |
| `checkout_started` | track | checkout | `path`, `source`, `itemCount`, `cartValue` (`email` optional) |
| `order_completed` | track | checkout | `path`, `source`, `orderId`, `itemCount`, `total`, `email` |
| identify | identify | — | `email` (traits; fired when checkout provides email) |

Custom types: `Event_source_type` (`web`), `Book_genre_type` (Fiction, Poetry, History, Science, Design, Business — same as dbt `books` seed).

## Layout

```
catalog/
  categories.yaml
  custom-types.yaml
  events.yaml
  properties.yaml
tracking-plans/
  webapp.yaml          # plan id: books_shop
```

## Link in Lookout

1. Settings → same warehouse account as `books-shop-dbt` → **Link project**
2. Repo: `ASaiAnudeep/books-shop-tracking-plan`, path: `/`
3. **Refresh warehouse documentation**
4. Context Hub → Tracking plans; chat e.g. “What properties does `order_completed` require?”
