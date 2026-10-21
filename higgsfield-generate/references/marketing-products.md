# Products

Two ways to register a product: URL fetch (auto-imports title, description, images) or manual (provide your own).

## URL fetch (default)

```bash
ID=$(higgsfield marketing-studio products fetch --url https://shop.example.com/sneakers --wait --json | jq -r .id)
```

`--wait` polls until `status` is `completed` or `failed`. Default timeout 90s.

If `failed`, check `fail_reason` — usually invalid URL or scrape blocked.

App Store URLs auto-route to `webproducts` (different endpoint). Use:

```bash
higgsfield marketing-studio webproducts fetch --url https://apps.apple.com/... --wait
```

## Manual

When the user has product photos and details:

```bash
A=$(higgsfield upload create shoe1.png)
B=$(higgsfield upload create shoe2.png)
higgsfield marketing-studio products create \
  --title "AeroRun Pro" \
  --description "Lightweight running shoe" \
  --image $A --image $B
```

Returns the product entity directly (no polling needed).

## Manual webproduct

For App Store / web pages without URL fetch:

```bash
higgsfield marketing-studio webproducts create \
  --url "https://example.com" \
  --title "MyApp" \
  --description "..."
```

`--subtitle`, `--favicon-url`, `--desktop` and `--mobile` are deprecated: the CLI still parses them but no longer sends them.

## Listing

```bash
higgsfield marketing-studio products list
higgsfield marketing-studio products list --json
higgsfield marketing-studio webproducts list
```
