# Troubleshooting

## Authentication

- `Session expired.` → `higgsfield auth login`
- `Stored credentials are for ... but current environment ...` → `higgsfield auth login` for the current API URL.
- `Not authenticated.` → first `higgsfield auth login`.

## Validation

- `Missing required params: prompt` — user gave no prompt. Ask.
- `Missing required params: medias` on Virality Predictor (`brain_activity`) — pass exactly one video via `--video <path-or-id>`. Virality Predictor does not need `--prompt`.
- `Invalid values: <param>=<v> (allowed: ...)` — pick from allowed enum.
- `Unknown params: <name>` — schema doesn't accept this flag. Run `higgsfield model get <jst>` and check.

## Job lifecycle

- `Job ended with status "failed"` — server-side failure. Often prompt content / safety. Try rephrasing.
- `nsfw` / `ip_detected` — content policy. Rephrase.
- `Timeout after 10m` — the job is still running; rejoin with `higgsfield generate wait <job_id> --timeout 30m` instead of resubmitting. For future long runs pass `--wait-timeout 30m` to `generate create`.

## Rate limits

`Higgsfield API error (HTTP 429)` — too many requests. Back off.

## CloudFlare / DataDome

If `Failed to decode response. Body: <html>...captcha-delivery...` appears, the server's anti-bot fired. Wait 30s and retry. If persistent, ping the team.

## Cost

`higgsfield generate cost <jst> ...` returns credit estimate without submitting. Useful when the user asks "how much will this cost?".

For workflows, use `higgsfield generate cost workflow <workflow_name> ...`, for example `higgsfield generate cost workflow reframe --duration 7.1 --resolution 1080p`. Do not use `higgsfield generate workflow cost ...`.
