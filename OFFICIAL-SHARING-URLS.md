# tesla-merchant-centre — official feed sharing URLs

*Verified live 2026-09-27 (GitHub Pages settings confirmed: source = `main` branch, root, no custom domain).*

These are the public URLs to hand to Google Merchant Center's **scheduled fetch** for each feed. Anything pushed to `main` in the repo is live at these URLs within moments — no separate publish/deploy step.

## Primary (GitHub Pages) — use these

| Feed | Entity | URL |
|---|---|---|
| Local Inventory Feed | Tesla Fire Systems Inc (Canada) | `https://skgroupstor.github.io/tesla-merchant-centre/GMC-local-inventory-feed-inc.csv` |
| Primary Product Feed | Tesla Fire Systems LLC (US) | `https://skgroupstor.github.io/tesla-merchant-centre/google-merchant-feed-llc.csv` |
| ChatGPT (OpenAI) Ads Feed | Tesla Fire Systems Inc (Canada) | `https://skgroupstor.github.io/tesla-merchant-centre/open%20ai/teslafiresystems_openai_feed.jsonl` |
| ChatGPT (OpenAI) Ads Feed — CSV copy | Tesla Fire Systems Inc (Canada) | `https://skgroupstor.github.io/tesla-merchant-centre/open%20ai/teslafiresystems_openai_feed.csv` |

Base site: `https://skgroupstor.github.io/tesla-merchant-centre/`

## Alternate (raw GitHub) — fallback only

If GitHub Pages is ever down or you need a link Google can fetch straight from the git history rather than the built site, the raw file URLs work the same way:

| Feed | URL |
|---|---|
| Inc — local inventory | `https://raw.githubusercontent.com/SKGroupstor/tesla-merchant-centre/main/GMC-local-inventory-feed-inc.csv` |
| LLC — primary product feed | `https://raw.githubusercontent.com/SKGroupstor/tesla-merchant-centre/main/google-merchant-feed-llc.csv` |
| Inc — ChatGPT (OpenAI) Ads feed | `https://raw.githubusercontent.com/SKGroupstor/tesla-merchant-centre/main/open%20ai/teslafiresystems_openai_feed.jsonl` |
| Inc — ChatGPT (OpenAI) Ads feed (CSV) | `https://raw.githubusercontent.com/SKGroupstor/tesla-merchant-centre/main/open%20ai/teslafiresystems_openai_feed.csv` |

Stick with the GitHub Pages links above as the ones actually registered in Merchant Center unless you have a specific reason to switch — no need to update anything in Google if you're not changing which URL is on file there.

## Notes

- The repo is **Public** on purpose — Merchant Center's scheduled fetch needs a URL anyone (including Google's fetch bot) can reach without logging in. That's expected and required, not an oversight.
- No custom domain is set up for the Pages site. If that ever changes, both URLs above change with it and Merchant Center's scheduled-fetch settings would need updating to match.
- The two Google feeds above are the full list for Merchant Center — there is no third Google feed URL for a separate LLC/Inc split (see the companion `README.md` for why the two files differ in structure).
- The ChatGPT (OpenAI) Ads feed is uploaded to OpenAI Ads Manager by SFTP rather than fetched by URL; its links are listed for download and reference. The folder name contains a space, so it appears as `open%20ai` in URLs.
