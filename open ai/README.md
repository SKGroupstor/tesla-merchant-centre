# ChatGPT (OpenAI) Ads product feed — teslafiresystems.com

Product feed for OpenAI Ads Manager (Canada-only ad account), built from the live WooCommerce catalog at https://teslafiresystems.com.

| File | Purpose |
|---|---|
| `teslafiresystems_openai_feed.jsonl` | Upload file (OpenAI product feed spec, one product per line) |
| `teslafiresystems_openai_feed.csv` | Same feed as a spreadsheet for review |

## Feed settings
- Seller: Tesla Fire Systems Inc
- Market: `target_countries: ["CA"]`, `store_country: "CA"`, prices in CAD
- `is_eligible_search: true`, `is_ads_eligible: true` on every row
- `item_id` = WooCommerce product ID (stable)
- Products with no brand on the site use `Tesla Fire Systems` as the brand
- Excluded: $0 free promo items (Free Maintenance Tags, Free safety tie)

## Upload
Ads Manager → Tools → Feeds → Create Feed → connect via SFTP → upload the `.jsonl` file.
OpenAI treats each upload as the full catalog; refresh at least daily.

Spec: https://developers.openai.com/commerce/specs/file-upload/products
