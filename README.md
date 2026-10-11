# tesla-merchant-centre — what this repo is

*Reviewed live on GitHub 2026-10-11; this file was pushed the same day.*

## One repo, not two

Right now there is **one** GitHub repo for this — `github.com/SKGroupstor/tesla-merchant-centre` (Public). There is no separate repo for the LLC side and no separate repo for the Inc side. Both entities' feeds live in this same repo, side by side, told apart only by filename:

| File | Entity | What it is |
|---|---|---|
| `GMC-local-inventory-feed-inc.csv` | **Tesla Fire Systems Inc** (Canada, teslafiresystems.com) | A **local inventory feed** — the add-on file that tells Google which store carries an item, for store-pickup/local-ads badges. It is *not* the primary product listing for Inc. |
| `google-merchant-feed-llc.csv` | **Tesla Fire Systems LLC** (US, teslafire.com) | The **primary product feed** — full listings (title, description, price, GTIN, shipping weight, etc.) for Merchant Center's main Shopping/Free-listings feed. |

That split isn't a mistake — it reflects how each entity actually submits to Google:

- **Inc (Canada):** its main product feed is generated automatically by the WooCommerce "Google Listings & Ads" plugin, which is why `GMC-local-inventory-feed-inc.csv` uses ids like `gla_17262` for most rows — that plugin's own Merchant Center IDs. (A handful of rows use plain numeric ids with no `gla_` prefix; that's expected and consistent with how the plugin assigns ids.) This repo only supplies the *local inventory* supplement Google needs on top of that.
- **LLC (US):** there's no such plugin feed, so the entire product feed is built and hosted here manually, refreshed by pushing an updated CSV.

(Repo's "About" line still just says "...for Tesla Fire Systems Inc." — a little out of date now that it also carries the LLC feed. Worth a one-line edit if you want it to read accurately.)

## Current file layout

```
tesla-merchant-centre/
├── README.md                             this file
├── OFFICIAL-SHARING-URLS.md              the public feed URLs to give Google/anyone who asks
├── GMC-local-inventory-feed-inc.csv     252 rows (+ header) — id, store_code, quantity, availability,
│                                                                pickup_method, pickup_sla, price, sale_price,
│                                                                sku_reference, product_name_reference,
│                                                                product_url_reference
├── google-merchant-feed-llc.csv         120 rows (+ header) — id, title, description, link, image_link, availability,
│                                                                price, sale_price, brand, condition, gtin, mpn,
│                                                                identifier_exists, shipping_weight
├── .github/workflows/backup-on-update.yml
└── backups/                              automated pre-push snapshots + a couple of manual historical uploads
```

## How it's hosted

Served as static files via **GitHub Pages**, building from the `main` branch root, at `https://skgroupstor.github.io/tesla-merchant-centre/`. No custom domain is configured. Anything pushed to `main` goes live at that URL within moments — there's no separate "publish" step. (Full public URLs for each feed are in the companion file.)

## Automatic backups

`.github/workflows/backup-on-update.yml` runs on every push to `main`. If a push **modifies** a file that already existed, the Action saves the file's *pre-push* version into `backups/` with a UTC timestamp in the name, then commits that backup back to `main` automatically. Brand-new files and deletions aren't backed up (nothing to preserve). This is why `backups/` has several dated snapshots plus some older manual uploads from before this workflow existed (e.g. `merchant_export_2026-06-07.tsv`, `tesla_fire_systems_merchant_centre_feed.csv`).

## Local inventory feed — fixed

`GMC-local-inventory-feed-inc.csv` previously had two problems that have since been corrected:

1. **Header names now match Google's spec.** The file uses `store_code` and `sale_price` (underscores), the exact attribute names Google's local inventory feed spec requires. (Earlier versions used `store code` and `sale price` with spaces, which a local-inventory feed — matched by exact attribute name — would have silently ignored rather than flagged as an error.)
2. **`pickup_method` and `pickup_sla` columns are present and populated** for every row — these are the two attributes that drive same-day/next-day pickup messaging on a listing.

The `gla_` ids were never the issue — those come straight from the WooCommerce plugin and have always been correct. The file now carries 252 data rows, all in the corrected format, and is live at the GitHub Pages URL in the companion file.
