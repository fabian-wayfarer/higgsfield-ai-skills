# Marketing Studio Modes

Current mode values for `marketing_studio_video` `--mode`. The live schema is the source of truth, so run `higgsfield model get marketing_studio_video` if validation fails.

| `--mode` slug | Human-readable label | Hook/setting | Best for |
|---|---|---|---|
| `ugc` | UGC | ✅ | Default. Casual, organic-feel content from a presenter. |
| `ugc_how_to` | Tutorial | ✅ | "Here's how to use this." Tutorial / explainer. |
| `ugc_unboxing` | Unboxing | ✅ | "Just got this in the mail." Unboxing reveal. |
| `product_showcase` | Product Showcase | ❌ | Clean product highlight, polished. |
| `product_review` | Product Review | ✅ | Presenter giving an opinion on the product. |
| `tv_spot` | TV Spot | ❌ | Broadcast-style commercial. Higher production. |
| `wild_card` | Wild Card | ❌ | Experimental, model picks the vibe. |
| `ugc_virtual_try_on` | UGC Virtual Try On | ✅ | Person trying on clothing/accessories — UGC vibe. |
| `virtual_try_on` | Pro Virtual Try On | ❌ | Same but more polished, model-driven. |

The "Hook/setting" column shows whether `--hook_id` and `--setting_id` are valid for that mode. Modes marked ❌ ignore or reject setup items.

**Default when the user doesn't specify:** `ugc`.

## Picking flow

- "Looks like a real person filmed on phone" → `ugc` family (`ugc`, `ugc_unboxing`, `ugc_virtual_try_on`, `ugc_how_to`)
- "Polished broadcast commercial" → `tv_spot`
- "Show the product itself, less presenter" → `product_showcase`
- "Presenter giving an opinion" → `product_review`
- "Try clothing on someone" → `virtual_try_on` (polished) or `ugc_virtual_try_on` (organic feel)
- "Surprise me / something different" → `wild_card`

## Other parameters

For `marketing_studio_video`, the API accepts:

- `aspect_ratio`: `auto`, `21:9`, `16:9`, `4:3`, `1:1`, `3:4`, `9:16` (default `9:16`).
- `duration`: integer ≥ 4 seconds. No fixed cap; check `higgsfield model get marketing_studio_video` for the current upper bound.
- `resolution`: `480p`, `720p`, or `1080p` (default `720p`).
- `generate_audio`: boolean (default `false`). Generates audio for the video.
- `avatars`: array of `{id, type}` where `type` is `preset` or `custom`. See `marketing-avatars.md`.
- `product_ids`: array of product UUIDs (from `higgsfield marketing-studio products fetch` or `create`). See `marketing-products.md`.
- `hook_id`: optional Marketing Studio setup hook UUID. See `marketing-setup-items.md`.
- `setting_id`: optional Marketing Studio setup setting UUID. See `marketing-setup-items.md`.
- `image_references` (`--image`), `start_image` (`--start-image`), `end_image` (`--end-image`): optional reference images.
- `ad_reference_id`: a completed ad reference UUID. See `marketing-ad-references.md`. Cannot be combined with `hook_id`/`setting_id`.
- `web_product_ids` with `specific_mode web_product`: webproducts from `higgsfield marketing-studio webproducts fetch`. Cannot be combined with `product_ids`.

## URL-driven Click-to-Ad shortcut

For `marketing_studio_video` driven by a product URL (no manual product create), the MCP-side flow is:

1. Call `higgsfield marketing-studio products fetch --url <url> --wait --json` and capture the returned `id` (or use `show_marketing_studio` widget action `fetch`). Stop if the id is empty or `null`.
2. Call `higgsfield generate create marketing_studio_video --prompt "..." --product_ids @<file>`, where the file holds `["<id>"]`. For App Store listings or a site/landing page (not a single product page), use `webproducts fetch` and pass `--web_product_ids @<file> --specific_mode web_product` instead.

Repeated fetches for the same URL dedupe — the backend reuses any existing non-failed entity.
