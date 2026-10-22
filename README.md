# forge-cats

The tokens listed on [The Forge](https://forge.awizard.dev): every pooled CAT and every Forge LP token, with its name, ticker, icon, decimals and, where the issuer published them, a description and website.

The lists change only when a token is deployed on the Forge. They carry no prices.

| Network | List |
|---|---|
| mainnet | `https://raw.githubusercontent.com/awizardxch/forge-cats/main/v1/mainnet.json` |
| testnet11 | `https://raw.githubusercontent.com/awizardxch/forge-cats/main/v1/testnet11.json` |

The same lists are served at `https://forge.awizard.dev/token-list/v1/<network>.json`. This repo is the copy that needs no Forge server: every icon in it is an image in this repo (`logos/`, `branding/`), linked on raw.githubusercontent.com.

## Format

Each file is a token index v1 document with no prices: `xch_usd` is `null`, entries carry no `price_xch`, `price_usd`, `liquidity_xch` or `source`, and `ttl_seconds` is a day. A wallet that reads the Forge's live token index reads these the same way.

```json
{
  "version": 1,
  "provider": { "name": "The Forge", "url": "https://github.com/awizardxch/forge-cats" },
  "network": "mainnet",
  "generated_at": "2026-10-21T00:00:00.000Z",
  "ttl_seconds": 86400,
  "xch_usd": null,
  "tokens": [
    { "asset_id": "<64 hex>", "name": "SPROUT", "ticker": "🌱", "icon_url": "https://raw.githubusercontent.com/awizardxch/forge-cats/main/logos/<sha256>.webp", "decimals": 3 }
  ]
}
```

| Field | Rule |
|---|---|
| `version` | `1`. Refuse any other value. |
| `network` | `mainnet` or `testnet11`. |
| `asset_id` | 64 lowercase hex characters, the CAT's asset id (TAIL hash). The only required field of an entry; one entry per id, sorted. |
| `name` | 1 to 64 UTF-16 units. |
| `ticker` | 1 to 16 UTF-16 units (an emoji counts as two or more). |
| `icon_url` | https, at most 512 characters. |
| `description` | At most 280 UTF-16 units. |
| `website` | https, at most 512 characters. |
| `decimals` | 0 to 18; 3 for every CAT today. |

The text bounds are the ones Sage applies to a CAT label, so any entry is one Sage would accept. A reader drops a single bad field or entry and keeps the rest, ignores fields it does not know, and uses a name only for a CAT it has no name for: never over one the user set.

Live prices are a separate feed: `https://forge.awizard.dev/token-index/v1/<network>.json`.

## How it is updated

Generated, not edited by hand. After a pool brings a new token, the Forge's `scripts/build-token-list.mjs --cats <this clone>` rewrites `v1/<network>.json` and copies any new images; the change is committed here and in the Forge's site in the same step.
