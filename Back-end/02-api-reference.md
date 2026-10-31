# 02 — API reference

Generated from `contracts/openapi.json` (72 operations). The OpenAPI file is itself generated from the FastAPI routes by `make gen`, and CI fails when it is out of date, so that file stays the contract.

The auth column is read from the OpenAPI security block and the path prefix. A `Bearer` route may also need a specific role; see [Auth and access](05-auth-and-access.md). Routes marked `None` may check a secret themselves, such as the SMS webhooks. `/internal/mediamtx-auth` is called by MediaMTX to authorise stream access, and judges the credentials MediaMTX forwards.

## `/admin`

| Method | Path | Summary | Auth |
|---|---|---|---|
| GET | `/admin/cameras` | List Cameras | Bearer |
| PATCH | `/admin/cameras/{camera_id}` | Update Camera | Bearer |
| PUT | `/admin/cameras/{camera_id}/zones` | Replace Zones | Bearer |
| GET | `/admin/facilities` | List Facilities | Bearer |
| PATCH | `/admin/facilities/{facility_id}` | Update Facility | Bearer |
| GET | `/admin/units` | List Units | Bearer |
| PATCH | `/admin/units/{unit_id}` | Update Unit | Bearer |
| GET | `/admin/users` | List Users | Bearer |
| POST | `/admin/users` | Create User | Bearer |
| PATCH | `/admin/users/{user_id}` | Update User | Bearer |

## `/auth`

| Method | Path | Summary | Auth |
|---|---|---|---|
| POST | `/auth/login` | Login | None |
| GET | `/auth/me` | Me | Bearer |
| POST | `/auth/stream-token` | Stream Token | Bearer |

## `/cameras`

| Method | Path | Summary | Auth |
|---|---|---|---|
| GET | `/cameras` | List Cameras | Bearer |
| GET | `/cameras/{camera_id}` | Get Camera | Bearer |

## `/evaluation`

| Method | Path | Summary | Auth |
|---|---|---|---|
| GET | `/evaluation/latest` | Latest Evaluation | Bearer |

## `/events`

| Method | Path | Summary | Auth |
|---|---|---|---|
| GET | `/events` | Incident Events | Bearer |

## `/facilities`

| Method | Path | Summary | Auth |
|---|---|---|---|
| GET | `/facilities/{facility_id}/prealerts` | Prealerts | Bearer |
| POST | `/facilities/{facility_id}/prealerts/{incident_id}/ack` | Acknowledge | Bearer |
| POST | `/facilities/{facility_id}/readiness` | Readiness | Bearer |

## `/health`

| Method | Path | Summary | Auth |
|---|---|---|---|
| GET | `/health` | Health | None |

## `/incidents`

| Method | Path | Summary | Auth |
|---|---|---|---|
| GET | `/incidents` | List Incidents | Bearer |
| POST | `/incidents` | Create Incident | Bearer |
| GET | `/incidents/{incident_id}` | Get Incident | Bearer |
| GET | `/incidents/{incident_id}.cap.xml` | Incident Cap | Bearer |
| POST | `/incidents/{incident_id}/casualties` | Add Casualty | Bearer |
| POST | `/incidents/{incident_id}/dispatch` | Dispatch Incident | Bearer |
| GET | `/incidents/{incident_id}/dispatches` | List Dispatches | Bearer |
| GET | `/incidents/{incident_id}/evidence/{evidence_id}` | Download Evidence | Bearer |
| POST | `/incidents/{incident_id}/hospital` | Change Hospital | Bearer |
| GET | `/incidents/{incident_id}/integrity` | Integrity | Bearer |
| POST | `/incidents/{incident_id}/merge` | Merge Incident | Bearer |
| GET | `/incidents/{incident_id}/recommendations` | Recommendations | Bearer |
| POST | `/incidents/{incident_id}/reject` | Reject Incident | Bearer |
| POST | `/incidents/{incident_id}/status` | Set Status | Bearer |
| POST | `/incidents/{incident_id}/verify` | Verify Incident | Bearer |

## `/internal`

| Method | Path | Summary | Auth |
|---|---|---|---|
| POST | `/internal/camera-health` | Post Camera Health | Service token |
| POST | `/internal/candidates` | Post Candidate | Service token |
| POST | `/internal/evidence` | Upload Evidence | Service token |
| POST | `/internal/mediamtx-auth` | Mediamtx Auth | Service token |
| POST | `/internal/try/claim` | Claim | Service token |
| POST | `/internal/try/jobs/{job_id}/fail` | Post Failure | Service token |
| GET | `/internal/try/jobs/{job_id}/file` | Job File | Service token |
| POST | `/internal/try/jobs/{job_id}/progress` | Progress | Service token |
| POST | `/internal/try/jobs/{job_id}/result` | Post Result | Service token |
| PUT | `/internal/try/jobs/{job_id}/video` | Put Video | Service token |
| GET | `/internal/try/sessions` | Active Sessions | Service token |
| POST | `/internal/try/sessions/{session_id}/events` | Session Event | Service token |
| POST | `/internal/try/status` | Report Status | Service token |

## `/kpis`

| Method | Path | Summary | Auth |
|---|---|---|---|
| GET | `/kpis` | Kpis | Bearer |

## `/map`

| Method | Path | Summary | Auth |
|---|---|---|---|
| GET | `/map/resources` | Map Resources | Bearer |

## `/public`

| Method | Path | Summary | Auth |
|---|---|---|---|
| POST | `/public/reports` | Post Report | Public |
| POST | `/public/try/jobs` | Create Job | Public |
| DELETE | `/public/try/jobs/{job_id}` | Delete Job | Public |
| GET | `/public/try/jobs/{job_id}` | Get Job | Public |
| GET | `/public/try/jobs/{job_id}/events` | Stream Job | Public |
| GET | `/public/try/jobs/{job_id}/video` | Job Video | Public |
| POST | `/public/try/sessions` | Create Session | Public |
| DELETE | `/public/try/sessions/{session_id}` | End Session | Public |
| GET | `/public/try/sessions/{session_id}` | Get Session State | Public |
| POST | `/public/try/sessions/{session_id}/ping` | Ping Session | Public |
| GET | `/public/try/status` | Try Status | Public |

## `/push`

| Method | Path | Summary | Auth |
|---|---|---|---|
| GET | `/push/public-key` | Push Public Key | None |
| POST | `/push/subscriptions` | Subscribe Push | Bearer |

## `/ready`

| Method | Path | Summary | Auth |
|---|---|---|---|
| GET | `/ready` | Ready | None |

## `/routing`

| Method | Path | Summary | Auth |
|---|---|---|---|
| GET | `/routing/recommendations` | Recommendations | Bearer |

## `/sms`

| Method | Path | Summary | Auth |
|---|---|---|---|
| GET | `/sms/gateway` | Sms Gateway | Bearer |
| POST | `/sms/inbound` | Sms Inbound | None |
| POST | `/sms/status` | Sms Status | None |

## `/system`

| Method | Path | Summary | Auth |
|---|---|---|---|
| GET | `/system/ai` | System Ai | Bearer |
| GET | `/system/cameras` | System Cameras | Bearer |

## `/tiles`

| Method | Path | Summary | Auth |
|---|---|---|---|
| GET | `/tiles/algeria.pmtiles` | Algeria Tiles | None |
