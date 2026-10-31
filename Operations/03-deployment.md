# 03 — Deployment

This summarises `docs/DEPLOYMENT.md`, which holds the exact steps.

## Layout

| Part | Where | How |
|---|---|---|
| Core | VPS | Dokploy, Docker Compose mode, `deploy/vps/compose.yml` |
| AI | NVIDIA Brev | `deploy/gpu/compose.yml`, outbound connections only |
| Images | GHCR | `ghcr.io/nadjeet/nadjet-{api,web,vision}`, built by CI |

## Open ports on the VPS

`22/tcp`, `80/tcp`, `443/tcp`, `443/udp`, `8554/tcp` (RTSP), `8189/udp` and `8189/tcp` (WebRTC), and `3000/tcp` for first Dokploy setup only.

`stream.` must be DNS-only, because WebRTC and RTSP aren't plain HTTP.

## Release

1. Merge to `main`.
2. Tag: `git tag demo-v2 && git push origin demo-v2`. CI builds the images.
3. In Dokploy, set `NADJET_TAG=demo-v2` and deploy. `migrate` runs first, and the API starts only if it succeeds.

## Rollback

Set `NADJET_TAG` back and deploy. Migrations go forward only, so take a `pg_dump` before a schema-changing release.

## Health

| Check | Purpose |
|---|---|
| `GET /health` | Process is up |
| `GET /ready` | Redis and PostGIS respond |
| `https://app.<domain>/healthz` | Web is up |

## Seeding

`nadjet-api seed-demo` loads users, cameras, units and facilities. It **never** creates incidents; those come only from the live pipeline (FR-VIS-020).
