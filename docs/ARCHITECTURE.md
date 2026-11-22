# Architecture — PublicAuction

## Deployment
- apps/web: Next.js App Router, TypeScript, CSS, server-side source attribution.
- services/collector: Python HTTP client, Pydantic schema validation, APScheduler/cron.
- services/api: read-only versioned REST (optional; start with prebuilt JSON snapshots).
- DB: PostgreSQL with partitioning only after measurements.
- staging: Vercel preview, no secret exposed. Free plan conditions must be checked before commercial use.
- production candidate: home Ubuntu + Cloudflare Tunnel + HTTPS + DNS if verified. Avoid direct public DB access.

## Contract boundaries
`GET /v1/categories/stats?period=...`, `GET /v1/assets?...&cursor=...`, `GET /v1/assets/{id}`, `GET /v1/health/data`.
Pagination, bounded limits, explicit timestamps, source links, null semantics, errors. GET idempotent; ETag for cached reads.

## Frontend
App Router routes /, /categories/[slug], /region/[slug], /assets/[id], /statistics, /guides/[slug]. SSR for discoverability. No fabricated images: only licensed source images or generic category art clearly labeled.

## Security / Operations
API keys in env/secrets, HTTPS, same-site cookies if accounts, strict CORS, CSP, server-only database credentials, least-privilege read DB user, rate limits, audit logs sans sensitive payloads, validated URLs to avoid SSRF, SQL parameterization, encrypted backups + restore test. Health checks, logs, alert on ingest drift. Admin surface requires auth/MFA; not publicly indexed.

## Hosting economics
Heavy ETL at home, public static artifacts on Vercel / CDN. Avoid making every page request depend on an intermittently available home PC. Estimate electricity and API quotas; keep custom-domain purchase optional until validated.

## Definition of Done
Tested API contract + provenance + fallback + accessible mobile UI + page metadata + security review + data freshness label.
