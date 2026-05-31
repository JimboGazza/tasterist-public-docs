# Cloudflare Email Setup

Current target behavior:

- support / suggestion emails land in `james@tasterist.com`
- weekly emails can go to opted-in staff on `@penninegymnastics.com`
- app-side owner routing and worker-side recipient rules stay aligned

## Current model

There are two separate controls:

1. Render app settings decide who the app tries to email.
2. The Cloudflare worker decides which recipients are actually allowed.

The worker in this repo supports:

- exact owner inbox via `OWNER_EMAIL`
- extra owner aliases via `OWNER_EMAIL_ALIASES`
- domain-wide allow via `ALLOWED_TO_DOMAIN` or `ALLOWED_TO_DOMAINS`

Important:

- `TASTERIST_EMAIL_OWNER_ONLY=1` on Render forces owner-only sending from the app.
- `TASTERIST_EMAIL_OWNER_ONLY=0` allows weekly staff emails, subject to the worker allow rules.
- domain-wide recipient rules are enforced in worker code, not in the `send_email` binding block

## Recommended current setup

Use these values.

### Render web service

- `TASTERIST_EMAIL_ENABLED=1`
- `TASTERIST_EMAIL_OWNER_ONLY=0`
- `TASTERIST_OWNER_EMAIL=james@tasterist.com`
- `TASTERIST_EMAIL_FROM=Tasterist <noreply@tasterist.com>`
- `TASTERIST_EMAIL_WEBHOOK_URL=https://<your-worker-subdomain>.workers.dev`
- `TASTERIST_EMAIL_WEBHOOK_TOKEN=<same as WEBHOOK_TOKEN>`
- `TASTERIST_CRON_TOKEN=<same as RENDER_CRON_TOKEN>`

If you ever want to force owner-only mode again:

- set `TASTERIST_EMAIL_OWNER_ONLY=1`

### Cloudflare worker vars / secrets

Secrets:

- `WEBHOOK_TOKEN`
- `RENDER_CRON_TOKEN`
- `RESEND_API_KEY` if you want the worker to use Resend

Vars:

- `OWNER_EMAIL=james@tasterist.com`
- `OWNER_EMAIL_ALIASES=james@penninegymnastics.com`
- `ALLOWED_TO_DOMAINS=penninegymnastics.com,tasterist.com`
- `DEFAULT_FROM=noreply@tasterist.com`
- `RENDER_CRON_URL=https://tasterist.com/cron/weekly-admin-report`

## Backend choice

The worker supports two backends:

1. Resend
2. Cloudflare `send_email` binding

Recommended for domain-wide staff email:

- use `RESEND_API_KEY`

Why:

- the worker can then allow `@penninegymnastics.com` through its own domain checks
- the `send_email` binding is better suited to tightly fixed destinations

If you do not set `RESEND_API_KEY`, the worker falls back to the `send_email` binding.

## Repo files

Current repo config lives in:

- [cloudflare/email-worker/wrangler.toml](/Users/jamesgardner/Documents/Tasterist/cloudflare/email-worker/wrangler.toml)
- [cloudflare/email-worker/src/index.js](/Users/jamesgardner/Documents/Tasterist/cloudflare/email-worker/src/index.js)

Current repo defaults:

- `OWNER_EMAIL=james@tasterist.com`
- `OWNER_EMAIL_ALIASES=james@penninegymnastics.com`
- `ALLOWED_TO_DOMAINS=penninegymnastics.com,tasterist.com`

## Deploy command

From repo root:

```bash
npx wrangler deploy --config cloudflare/email-worker/wrangler.toml
```

Do not use bare `npx wrangler deploy` at repo root.

## Token generation

Generate secure values locally:

```bash
openssl rand -base64 48
```

Run twice:

- first output -> `WEBHOOK_TOKEN` and `TASTERIST_EMAIL_WEBHOOK_TOKEN`
- second output -> `RENDER_CRON_TOKEN` and `TASTERIST_CRON_TOKEN`

## Test checklist

### 1. Worker health

Open the worker URL in a browser:

```text
https://<your-worker-subdomain>.workers.dev
```

Expected:

- JSON response
- shows worker diagnostic flags

### 2. Support / suggestion email

In the app:

1. submit a suggestion report
2. confirm it lands in `james@tasterist.com`

If it says success but no email arrives:

- check spam / junk
- verify `TASTERIST_OWNER_EMAIL` on Render is `james@tasterist.com`
- verify worker `OWNER_EMAIL` is also `james@tasterist.com`

### 3. Weekly report test

In the app:

1. go to `Settings -> Admin Console`
2. use the test / send weekly email action
3. confirm opted-in staff on `@penninegymnastics.com` receive mail

### 4. Cron endpoint

```bash
curl -sS -X POST "https://tasterist.com/cron/weekly-admin-report" \
  -H "X-Tasterist-Cron-Token: <RENDER_CRON_TOKEN>"
```

Expected:

- JSON with status output

## Failure modes

### `403 recipient not allowed`

Cause:

- worker recipient rules blocked the target address

Checks:

- `OWNER_EMAIL`
- `OWNER_EMAIL_ALIASES`
- `ALLOWED_TO_DOMAIN` / `ALLOWED_TO_DOMAINS`
- whether the target really ends with `@penninegymnastics.com`

### App says success but inbox is empty

Cause is usually one of:

- Render owner email points at the wrong inbox
- worker owner email points at the wrong inbox
- message accepted by provider but landed in spam
- provider-side delivery issue after webhook acceptance

### Weekly emails only go to owner

Cause:

- `TASTERIST_EMAIL_OWNER_ONLY=1`

Fix:

- set `TASTERIST_EMAIL_OWNER_ONLY=0`
- redeploy Render

## Summary

For the current intended behavior, keep these aligned:

- Render `TASTERIST_OWNER_EMAIL=james@tasterist.com`
- Render `TASTERIST_EMAIL_OWNER_ONLY=0`
- Worker `OWNER_EMAIL=james@tasterist.com`
- Worker `OWNER_EMAIL_ALIASES=james@penninegymnastics.com`
- Worker `ALLOWED_TO_DOMAINS=penninegymnastics.com,tasterist.com`
