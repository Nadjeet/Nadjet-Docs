# 01 — Decision log

Index of the 55 technical decisions in [`docs/11-technical-decisions.md`](https://github.com/Nadjeet/nadjet/blob/dev/docs/11-technical-decisions.md). It is generated from that file, which holds the full context, the rejected options and the reasoning. Rows are never renumbered, and later decisions can supersede earlier ones, so check the full row before relying on one.

| ID | Date | Topic | Summary |
|---|---|---|---|
| TD-01 | 2026-10-30 | AI verifier | pretrained Qwen3-VL-8B-Instruct-FP8 (Apache-2.0) on vLLM, used for both the verifier and the sweep |
| TD-02 | 2026-10-30 | Detector | RF-DETR (Apache-2.0), COCO classes |
| TD-03 | 2026-10-30 | Tracking, zones, speed | supervision (MIT): ByteTrack + PolygonZone |
| TD-04 | 2026-10-30 | Optional modules | smoke/fire detector (weslleyskah/fire_smoke_box YOLO26n or D-Fire YOLOv5s; both AGPL-derived), run as a separate process, off by d |
| TD-05 | 2026-10-30 | One monorepo | Nadjeet/nadjet, plus Nadjeet/.github for the org profile |
| TD-06 | 2026-10-30 | Video server | MediaMTX (MIT) |
| TD-07 | 2026-10-30 | Annotated stream | re-encoded on the GPU (NVENC) and republished to the video server |
| TD-08 | 2026-10-30 | Backend | Python 3.12, FastAPI, Pydantic v2, SQLAlchemy 2 + Alembic, arq jobs |
| TD-09 | 2026-10-30 | Data | PostgreSQL + PostGIS; Redis Streams for candidates and jobs |
| TD-10 | 2026-10-30 | Routing | self-hosted OSRM on the Geofabrik Algeria extract |
| TD-11 | 2026-10-30 | Frontend | one Vite + React 19 + TypeScript SPA with 4 surfaces; TanStack Router/Query; Tailwind + shadcn/ui; MapLibre GL + self-hosted Proto |
| TD-12 | 2026-10-30 | Real-time to browsers | SSE for incident updates; WebSocket where interactive |
| TD-13 | 2026-10-30 | SMS | SMS Gateway for Android (capcom6, Apache-2.0) on our own phone and SIM, in private-server mode (gateway server self-hosted on the  |
| TD-14 | 2026-10-30 | Other channels | Web Push (VAPID, no account); Telegram Bot API |
| TD-15 | 2026-10-30 | Alert format | OASIS CAP 1.2 XML, validated against the XSD |
| TD-16 | 2026-10-30 | Evidence integrity | SHA-256 per file; hash chain over incident events; Ed25519-signed chain head (PyNaCl) |
| TD-17 | 2026-10-30 | Stage deployment | VPS + Dokploy in Docker Compose mode (not Stack) runs web, api, worker, PostGIS, Redis, OSRM and MediaMTX |
| TD-18 | 2026-10-30 | GPU host | NVIDIA Brev, 1× L40S (L4 as fallback) |
| TD-19 | 2026-10-30 | CI/CD | GitHub Actions builds images and pushes them to GHCR (ghcr.io/nadjeet/nadjet-{web,api,vision}) |
| TD-20 | 2026-10-30 | ~~Hosted AI fallback (Gemini)~~ **Superseded by TD-27** |  |
| TD-21 | 2026-10-30 | Offline fallback | deploy/local runs the full stack on the laptop except AI inference (Brev only, TD-27) |
| TD-22 | 2026-10-30 | AI sweep | every 5 s per camera, the last 8 s at 2 fps, 640 px, sent as one video to the same Qwen3-VL server |
| TD-23 | 2026-10-30 | Upload two-pass | pass 1 scans the whole clip at 1–2 fps with a "no collision" option; pass 2 looks at ±2–3 s around the found time at 4 fps; correc |
| TD-24 | 2026-10-30 | Validation layer | Pydantic schema check; every finding must cite an existing frame ID, else dropped and logged; trilingual (AR/FR/EN) safety-word fi |
| TD-25 | 2026-10-30 | `models.lock.yaml` | every weight pinned with source, version, checksum and licence; CI fails if an AGPL or non-commercial model is enabled by default  |
| TD-26 | 2026-10-30 | Task runner: GNU `make` | (Makefile, works with make 3.81 on macOS) |
| TD-27 | 2026-10-30 | NVIDIA Brev is the only AI inference provider | , including local development: make brev-forward (localhost:18000 → instance:8000), make brev-check |
| TD-28 | 2026-10-30 | Testing policy | pytest unit + integration tests for every non-frontend component (integration = real PostGIS/Redis, fail when infra is down); @pyt |
| TD-29 | 2026-10-30 | Contracts source of truth = code | Pydantic models in packages/shared and FastAPI routes; make gen exports contracts/events/*.schema.json, contracts/openapi.json and |
| TD-30 | 2026-10-30 | Local infra | imresamu/postgis:17-3.5 (multi-arch: arm64 Macs + amd64 CI), redis:8-alpine, bluenviron/mediamtx:1.21.2-ffmpeg; host ports 55432 / |
| TD-31 | 2026-10-30 | Agent workflow | AGENTS.md single source; /implement-issue (Sonnet 5.5) and /review-pr (Opus 5.5) skills in .agents/skills/ linked into .claude/ski |
| TD-32 | 2026-10-30 | Auth | argon2-cffi (Argon2id) passwords, PyJWT HS256 access tokens (NADJET_JWT_SECRET, refused in production if weak), PyYAML for seed-us |
| TD-33 | 2026-10-30 | Incident engine | candidates go API → Redis Stream incidents.candidates (consumer group api, ack after DB commit, stale entries re-claimed with XAUT |
| TD-34 | 2026-10-30 | Web foundation | code-based TanStack Router (role guards in beforeLoad), TanStack Query over @nadjet/api-client with a bearer middleware; live even |
| TD-35 | 2026-10-30 | Camera registry and simulator controls | catalogue, zones and held-out models live in nadjet_shared (read by camsim and the API) |
| TD-36 | 2026-10-30 | Vision runtime on Brev (dev) | one image services/vision/Dockerfile (CUDA 12.4 runtime, Python 3.12 via uv, nadjet-vision[gpu], apt ffmpeg with NVIDIA_DRIVER_CAP |
| TD-37 | 2026-10-30 | Vision worker pipeline | one process per camera, started by a supervisor (nadjet-vision camera --all, restart back-off 1 s to 30 s) |
| TD-38 | 2026-10-30 | Map and routing delivery | OSRM ghcr.io/project-osrm/osrm-backend:v6.0.0 (multi-arch; newest tag published as an image), car profile, MLD, graph in the osrmd |
| TD-39 | 2026-10-30 | Crash triggers and evidence | triggers.py evaluates the four rules per frame over per-track state (one engine per camera, thresholds = §6 defaults, overridable  |
| TD-40 | 2026-10-30 | Notifications | NotificationChannel protocol (SMS gateway, Web Push via pywebpush + VAPID, optional Telegram) |
| TD-41 | 2026-10-30 | AI verifier and sweep wiring | each verification (video + call + one retry) is bounded by NADJET_VLM_BUDGET_S = 7 s (SC-001 leaves the AI about that much); a tim |
| TD-42 | 2026-10-30 | Evaluation harness | eval/run_eval.py (make eval, needs the GPU extra and make brev-forward) replays each scorable clip through the real CameraPipeline |
| TD-43 | 2026-10-30 | Browser playback | LiveStream is a plain RTCPeerConnection WHEP client (non-trickle: offer POSTed after ICE gathering, 201 + SDP answer, RFC 9725) ag |
| TD-44 | 2026-10-30 | Dispatch | nadjet_api/dispatch/ |
| TD-45 | 2026-10-30 | Public citizen report | POST /public/reports (multipart: lat, lon, optional people_reported, trapped, phone, photo; optional Idempotency-Key) ingests one  |
| TD-46 | 2026-10-30 | Try it yourself (sandbox) | abuse guards on the anonymous surface: per-IP rate limits on uploads, sessions and progress streams; global caps on open jobs (NAD |
| TD-47 | 2026-10-30 | KPIs, admin and retention | GET /kpis (supervisor, admin) derives durations from the signed incident events, in memory: detection = event time to the first cr |
| TD-48 | 2026-10-30 | Responder app (`/respond`) | an installable PWA on the existing generated service worker; web push handlers live in apps/web/public/push-sw.js, loaded with Wor |
| TD-49 | 2026-10-30 | Hospital pre-alert view (`/hospital`) | GET /facilities/{id}/prealerts returns the facility, the incoming pre-alerts (open incidents whose active hospital dispatch is thi |
| TD-50 | 2026-10-30 | Demo blockers found in the first end-to-end Brev run | (1) the worker image now takes torch/torchvision from the PyTorch CUDA 12.6 index on Linux ([tool.uv.sources] with an explicit pyt |
| TD-51 | 2026-10-30 | Production deploy (#21) | deploy/vps/compose.yml (Dokploy Docker Compose mode) pulls ghcr.io/nadjeet/nadjet-{api,web}:${NADJET_TAG} built by CI (also on dem |
| TD-52 | 2026-10-30 | Release readiness (#22) | nadjet-api seed-demo composes the existing seeders and never writes incidents (FR-VIS-020); docs/demo/RESULTS.md holds only measur |
| TD-53 | 2026-10-31 | Fixes from the second end-to-end run (4 simulated cameras, Brev) | (1) Camera wall: each <video> has one owner, the latest supervise call (a WeakMap of claims); a WHEP or HLS attempt that was cance |
| TD-54 | 2026-10-31 | Final visual identity (Figma "najat") in the web app | light mode only (the operator console's dark theme, .dark tokens and dark: classes are removed); the Figma logo (node 75:6347) is  |
| TD-55 | 2026-10-31 | Exact Figma fidelity, superseding the contrast tweaks of TD-54 | The designer's file is the source of truth: exact colours (brand #007dfe/#55a8fe, success #22c55e/#16a34a, warning #f59e0b, error  |
