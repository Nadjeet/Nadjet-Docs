# 02 — Component wiring

Every link below was checked against `deploy/vps/compose.yml`, `deploy/gpu/compose.yml`, `apps/web/nginx.conf` and the code.

## Services

| Service | Image or command | Location | Purpose |
|---|---|---|---|
| `web` | `nadjet-web` (nginx) | VPS | Serves the SPA and proxies `/api/` to the API |
| `api` | `nadjet-api` | VPS | HTTP API and SSE |
| `consumer` | `nadjet-api consume` | VPS | Persists candidates from the Redis Stream |
| `jobs` | `nadjet-api worker` | VPS | arq worker that delivers notifications |
| `migrate` | `alembic upgrade head` | VPS | One-shot migration, runs before the API |
| `mediamtx-config` | `nadjet-camsim render` | VPS | One-shot generation of the MediaMTX config |
| `mediamtx` | `mediamtx:1.21.2-ffmpeg` | VPS | Video in and out |
| `db` | PostGIS 17-3.5 | VPS | System of record |
| `redis` | Redis 8, AOF on | VPS | Stream, Pub/Sub, jobs, rate limits, health |
| `osrm` | OSRM 6.0.0 | VPS | Road routing |
| `sms-server`, `sms-worker`, `sms-db` | capcom6 1.34.2, MariaDB | VPS | Private SMS gateway |
| `vllm` | vLLM + Qwen3-VL | Brev | AI model server |
| `workers` | `camera --all` | Brev | One process per camera, under a supervisor |
| `try-worker` | `try` | Brev | Public sandbox analysis |

## Links

| From | To | Protocol | Carries |
|---|---|---|---|
| Video files | MediaMTX | Local files | Simulated camera streams |
| Phone browser | MediaMTX | WHIP | Live phone video |
| MediaMTX | Vision worker | RTSP, port 8554 | Raw video |
| Vision worker | MediaMTX | RTSP | Annotated `<camera>_ai` stream |
| MediaMTX | try-worker | RTSP | Live phone video |
| Vision worker, try-worker | vLLM | HTTP, loopback | Video and prompt, JSON answer |
| API | vLLM | HTTP | AI status check |
| Vision worker | API | HTTPS `/internal/*` | Candidates, evidence, health |
| try-worker | API | HTTPS `/internal/try/*` | Jobs, sessions, results |
| MediaMTX | API | HTTP `/internal/mediamtx-auth` | Stream authorisation |
| API | Redis | Redis | `XADD`, job enqueue |
| Redis | API | Pub/Sub | Change events for SSE |
| Redis | Consumer | Consumer group | Candidate entries, `XACK`; the consumer also publishes changes |
| Redis | Jobs worker | arq | Delivery jobs |
| API, consumer, jobs | PostGIS | SQL | Records |
| API | OSRM | HTTP | Routes and ETAs |
| API | Local volumes | Files | Evidence, sandbox files, map tiles |
| Browser | nginx | HTTPS | SPA and `/api` |
| nginx | API | HTTP | REST and SSE, buffering off |
| Browser | MediaMTX | WHEP, LL-HLS | Video playback |
| Jobs worker | SMS server | HTTP | Outbound SMS |
| API | SMS server | HTTP | Immediate replies to inbound SMS |
| Jobs worker | Browser | Web Push | Alerts through the browser push service |
| SMS server | Android phone | capcom6 | Messages to send |
| Android phone | API | Webhooks, HMAC signed | Inbound SMS, delivery status |
| Android phone | Field phones | GSM | SMS |

## Public hostnames

| Host | Service | Port |
|---|---|---|
| `app.`, `respond.`, `hospital.`, `try.` | `web` | 80 |
| `api.` | `api` | 8000 |
| `stream.` | `mediamtx` | 8889 (WHEP, WHIP) |
| `hls.` | `mediamtx` | 8888 (LL-HLS) |
| `sms.` | `sms-server` | 3000 |

Traefik, managed by Dokploy, terminates HTTPS. Ports 8554 (RTSP) and 8189 (WebRTC media) are published directly.

## Direction of connections

Nothing listens publicly on the Brev instance. Workers connect outward to the VPS (TD-18), and vLLM stays on loopback.
