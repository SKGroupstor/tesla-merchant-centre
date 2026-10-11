# tesla-merchant-centre — what this repo is

*Reviewed live on GitHub 2026-09-27; this file was pushed the same day.*

## One repo, not two

Right now there is **one** GitHub repo for this — `github.com/SKGroupstor/tesla-merchant-centre` (Public). There is no separate repo for the LLC side and no separate repo for the Inc side. Both entities' feeds live in this same repo, side by side, told apart only by filename:

| File | Entity | What it is |
|---|---|---|
| `GMC-local-inventory-feed-inc.csv` | **Tesla Fire Systems Inc** (Canada, teslafiresystems.com) | A **local inventory feed** — the add-on file that tells Google which store carries an item, for store-pickup/local-ads badges. It is *not* the primary product listing for Inc. |
| `google-merchant-feed-llc.csv` | **Tesla Fire Systems LLC** (US, teslafire.com) | The **primary product feed** — full listings (title, description, price, GTIN, shipping weight, etc.) for Merchant Center's main Shopping/Free-listings feed. |

That split isn't a mistake — it reflects how each entity actually submits to Google:

- **Inc (Canada):** its main product feed is generated automatically by the WooCommerce "Google Listings & Ads" plugin, which is why `GMC-local-inventory-feed-inc.csv` uses ids like `gla_17262` — that plugin's own Merchant Center IDs. This repo only supplies the *local inventory* supplement Google needs on top of that.
- **LLC (US):** there's no such plugin feed, so the entire product feed is built and hosted here manually, refreshed by pushing an updated CSV.

(Repo's "About" line still just says "...for Tesla Fire Systems Inc." — a little out of date now that it also carries the LLC feed. Worth a one-line edit if you want it to read accurately.)

## Current file layout

```
tesla-merchant-centre/
├── README.md                             this file
├── OFFICIAL-SHARING-URLS.md              the public feed URLs to give Google/anyone who asks
├── GMC-local-inventory-feed-inc.csv     258 rows (+ header) — id, store code, quantity, price, sale price,
│                                                                availability (about to be redone — see below)
├── google-merchant-feed-llc.csv         120 rows (+ header) — id, title, description, link, image_link, availability,
│                                                                price, sale_price, brand, condition, gtin, mpn,
│                                                                identifier_exists, shipping_weight
├── .github/workflows/backup-on-update.yml
└── backups/                              automated pre-push snapshots + a couple of manual historical uploads
```

## How it's hosted

Served as static files via **GitHub Pages**, building from the `main` branch root, at `https://skgroupstor.github.io/tesla-merchant-centre/`. No custom domain is configured. Anything pushed to `main` goes live at that URL immediately — there's no separate "publish" step. (Full public URLs for each feed are in the companion file.)

## Automatic backups

`.github/workflows/backup-on-update.yml` runs on every push to `main`. If a push **modifies** a file that already existed, the Action saves the file's *pre-push* version into `backups/` with a UTC timestamp in the name, then commits that backup back to `main` automatically. Brand-new files and deletions aren't backed up (nothing to preserve). This is why `backups/` has two dated snapshots of `google-merchant-feed-llc.csv` from earlier pushes, plus some older manual uploads from before this workflow existed (e.g. `merchant_export_2026-06-07.tsv`, `tesla_fire_systems_merchant_centre_feed.csv`).

## Local inventory feed — being redone

`GMC-local-inventory-feed-inc.csv` still has two known problems as of this push (not fixed yet — Ash is providing updated data before this file gets rebuilt):

1. **Header names don't match Google's spec.** It uses `store code` and `sale price` (spaces) instead of the required `store_code` and `sale_price` (underscores). A local inventory feed is matched by exact attribute name, so these columns are very likely being silently ignored by Google rather than causing a visible error.
2. **No `pickup_method` / `pickup_sla` columns at all** — the two attributes that actually drive same-day/next-day pickup messaging on a listing.

Note that neither problem involves the `gla_` ids themselves — those are already correct, straight from the WooCommerce plugin. This file will be replaced with a corrected version once the new source data is in hand.
