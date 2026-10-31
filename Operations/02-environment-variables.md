# 02 — Environment variables

Generated from the `Settings` classes in `services/api/src/nadjet_api/config.py` and `services/vision/src/nadjet_vision/config.py`. Those classes are the source of truth. Local values go in `.env`, copied from `.env.example`; VPS values start from `deploy/vps/.env.example`.

Secrets never go in git. Rows with an empty default are either required or optional features that stay off.

## API

| Variable | Default | Purpose |
|---|---|---|
| `NADJET_ACCESS_TOKEN_TTL_MINUTES` | `30` |  |
| `NADJET_CANDIDATE_CLAIM_IDLE_MS` | `30_000` | Re-claim stream entries pending longer than this |
| `NADJET_CORS_ORIGINS` | `["http://localhost:5173"]` |  |
| `NADJET_DATABASE_URL` | `PostgresDsn("postgresql+asyncpg://nadjet:nadjet@localhost:55432/nadjet")` | SQLAlchemy async URL (asyncpg driver) |
| `NADJET_EVALUATION_DIR` | `Path(__file__).resolve().parents[4] / "eval" / "results"` | Evaluation results written by `make eval` |
| `NADJET_EVIDENCE_DIR` | `Path("evidence")` | Local evidence store root |
| `NADJET_EVIDENCE_MAX_MB` | `100` |  |
| `NADJET_HLS_URL_TEMPLATE` | `"http://localhost:8888/{path}/index.m3u8"` | Camera LL-HLS URL (WebRTC fallback); {path} = stream path |
| `NADJET_INGEST_TOKEN` | `None` | Service token for /internal/*; endpoints return 503 when unset |
| `NADJET_JWT_SECRET` | `None` | HS256 signing key; required (>= 32 chars) outside development/test |
| `NADJET_LIVE_DEMO_LAT` | `36.7538` | Where a live demo camera sits |
| `NADJET_LIVE_DEMO_LON` | `3.0588` |  |
| `NADJET_MAP_TILES_PATH` | `Path("data/map/algeria.pmtiles")` | PMTiles file served at /tiles |
| `NADJET_NOTIFY_MAX_RETRIES` | `3` |  |
| `NADJET_NOTIFY_RETRY_BASE_S` | `30` | Resend when not delivered after this long, doubling |
| `NADJET_OSRM_URL` | `"http://localhost:55000"` | Self-hosted OSRM base URL |
| `NADJET_PUBLIC_PHOTO_MAX_MB` | `10` |  |
| `NADJET_PUBLIC_REPORT_RATE_LIMIT` | `5` | Citizen reports allowed per client IP per window |
| `NADJET_PUBLIC_REPORT_WINDOW_S` | `600` |  |
| `NADJET_READINESS_TIMEOUT_S` | `2.0` |  |
| `NADJET_REDIS_URL` | `RedisDsn("redis://localhost:56379/0")` |  |
| `NADJET_RETENTION_DAYS` | `90` | Evidence older than this is purged (FR-SEC-005) |
| `NADJET_RETENTION_INTERVAL_S` | `3600.0` | Purge schedule |
| `NADJET_ROUTING_TIMEOUT_S` | `5.0` |  |
| `NADJET_ROUTING_URBAN_MULTIPLIER` | `1.3` | Factor applied to OSRM free-flow durations (no live traffic); see docs/11 |
| `NADJET_RTSP_URL_TEMPLATE` | `"rtsp://localhost:8554/{path}"` | Camera RTSP URL; {path} = stream path |
| `NADJET_SIGNING_KEY` | `None` | Ed25519 seed (64 hex chars) signing incident chain heads;  |
| `NADJET_SMS_GATEWAY_NUMBER` | `None` | Gateway SIM number; responders text their codes to it (FR-RSP-004) |
| `NADJET_SMS_GATEWAY_PASSWORD` | `None` |  |
| `NADJET_SMS_GATEWAY_URL` | `None` | capcom6 SMS Gateway server base URL; SMS disabled when unset |
| `NADJET_SMS_GATEWAY_USER` | `""` | Gateway API username (from the app) |
| `NADJET_SMS_WEBHOOK_SECRET` | `None` | Signing key of the gateway webhooks; /sms/* return 503 when unset |
| `NADJET_SMS_WEBHOOK_TOLERANCE_S` | `300` |  |
| `NADJET_STREAM_SERVICE_USER` | `"nadjet-worker"` | MediaMTX user of camsim/Brev workers; password is NADJET_INGEST_TOKEN |
| `NADJET_STREAM_TOKEN_TTL_S` | `300` | Console stream token lifetime |
| `NADJET_TELEGRAM_BOT_TOKEN` | `None` | Optional Telegram Bot API token |
| `NADJET_TRY_DIR` | `Path("try-sandbox")` | Isolated store of public uploads (FR-CIT-004) |
| `NADJET_TRY_EVENTS_MAX_S` | `120.0` | Life of one progress stream; the browser reconnects |
| `NADJET_TRY_EVENTS_PER_MINUTE` | `30` | Progress streams per IP |
| `NADJET_TRY_JOBS_PER_MINUTE` | `5` | Uploads per IP per minute |
| `NADJET_TRY_JOB_TIMEOUT_S` | `300` | Worker silent: job failed |
| `NADJET_TRY_MAX_LIVE_SESSIONS` | `3` | Phone sessions at once (operator wall and GPU guard) |
| `NADJET_TRY_MAX_MB` | `100` | Max upload size (FR-CIT-002) |
| `NADJET_TRY_MAX_OPEN_JOBS` | `20` | Queued or running uploads at once (disk and GPU guard) |
| `NADJET_TRY_MAX_SECONDS` | `60` | Max clip length (FR-CIT-002) |
| `NADJET_TRY_PURGE_INTERVAL_S` | `60.0` |  |
| `NADJET_TRY_SESSIONS_PER_MINUTE` | `5` | Live sessions per IP |
| `NADJET_TRY_SESSION_IDLE_S` | `30` | No ping for this long: ended |
| `NADJET_TRY_TTL_S` | `3600` | Uploads and results live this long |
| `NADJET_VAPID_PRIVATE_KEY` | `None` | Web Push VAPID private key; `nadjet-api gen-vapid` |
| `NADJET_VAPID_PUBLIC_KEY` | `None` |  |
| `NADJET_VAPID_SUBJECT` | `"mailto:ops@nadjet.example"` |  |
| `NADJET_WHEP_URL_TEMPLATE` | `"http://localhost:8889/{path}/whep"` | Camera WebRTC WHEP URL for the browser; {path} = stream path |
| `NADJET_WHIP_URL_TEMPLATE` | `"http://localhost:8889/{path}/whip"` | WHIP URL; {path} = stream path |

## Vision

| Variable | Default | Purpose |
|---|---|---|
| `NADJET_ANNOTATE` | `True` | Publish <camera>_ai annotated stream |
| `NADJET_ANNOTATED_ENCODER` | `"h264_nvenc"` | ffmpeg video encoder (TD-07) |
| `NADJET_API_URL` | `HttpUrl("http://127.0.0.1:8080")` | Coordination API base URL |
| `NADJET_BREV_INSTANCE` | `"nadjet-gpu"` | Brev instance running vLLM |
| `NADJET_CAMERAS_FILE` | `Path("data/cameras.yaml")` | Camera catalogue |
| `NADJET_DETECTOR_MODEL` | `"rf-detr-medium"` | Entry id in models.lock.yaml |
| `NADJET_DETECTOR_THRESHOLD` | `0.4` | Min confidence |
| `NADJET_EVIDENCE_AFTER_S` | `5.0` | Clip after the peak |
| `NADJET_EVIDENCE_BEFORE_S` | `10.0` | Clip before the peak |
| `NADJET_EVIDENCE_BUFFER_S` | `20.0` | Encoded packet ring; must exceed before + after with slack (FR-VIS-005) |
| `NADJET_EVIDENCE_KEYFRAMES` | `6` | JPEG keyframes per clip |
| `NADJET_HEALTH_PUSH_S` | `5.0` | Health report interval |
| `NADJET_INGEST_TOKEN` | `None` | Service token for /internal/* (never logged) |
| `NADJET_LIVE_CHECK_S` | `4.0` | Live phone AI check (FR-CIT-003) |
| `NADJET_METRICS_PORT` | `None` | Serve /metrics on this port |
| `NADJET_METRICS_PORT_BASE` | `9100` | --all: camera i serves base + i |
| `NADJET_MODELS_LOCK` | `Path("models.lock.yaml")` | Pinned weights (TD-25) |
| `NADJET_PUBLISH_CANDIDATES` | `True` | Post candidates and evidence to the API (FR-VIS-006) |
| `NADJET_RTSP_BASE` | `"rtsp://127.0.0.1:8554"` | RTSP base URL; cameras are read from <base>/<camera-id> |
| `NADJET_SAMPLE_FPS` | `5.0` | Analysed frames per second |
| `NADJET_STALL_S` | `10.0` | No frame for this long = camera fault |
| `NADJET_STATUS_CONFIRMED` | `0.8` | Yes probability for `confirmed` (TQ-07, untuned) |
| `NADJET_STATUS_PROBABLE` | `0.5` | Yes probability for `probable` (TQ-07, untuned) |
| `NADJET_SWEEP_ENABLED` | `True` | AI sweep on every camera (FR-VIS-015) |
| `NADJET_SWEEP_EVERY_S` | `8.0` | Sweep interval per camera (TD-22, TD-50) |
| `NADJET_SWEEP_YES_THRESHOLD` | `0.5` | Yes-token probability that counts as a sweep hit |
| `NADJET_TRIGGER_COOLDOWN_S` | `120.0` | Repeat-trigger cooldown per camera (FR-VIS-010) |
| `NADJET_TRY_POLL_S` | `1.0` | Try-it job and session polling |
| `NADJET_VERIFY_CANDIDATES` | `True` | Ask the AI verifier about every rule candidate (FR-VIS-005) |
| `NADJET_VLM_ALLOWED_HOSTS` |  | Extra hostnames of Brev instances allowed besides loopback |
| `NADJET_VLM_BACKOFF_S` | `30.0` | After the AI times out or is unreachable, answer 'unavailable' at once  |
| `NADJET_VLM_BASE_URL` | `HttpUrl("http://127.0.0.1:18000/v1")` | vLLM OpenAI-compatible endpoint (port-forwarded from Brev) |
| `NADJET_VLM_BUDGET_S` | `15.0` | Whole verification (video + call + retry) per candidate; one call takes  |
| `NADJET_VLM_MODEL` | `"Qwen/Qwen3-VL-8B-Instruct-FP8"` |  |
| `NADJET_VLM_TIMEOUT_S` | `15.0` | PRD FR-VIS-009 timeout |
| `NADJET_WEIGHTS_DIR` | `Path("weights")` | Downloaded, verified weights |

## Compose-only variables

Set only in `deploy/vps/.env.example`: `NADJET_TAG`, `NADJET_DOMAIN`, `NADJET_DB_PASSWORD`, `NADJET_PUBLIC_HOST`, `NADJET_SMS_DB_PASSWORD`, `NADJET_SMS_PRIVATE_TOKEN`.
