# 05 — Security

This page describes the PoC as built. Production hardening is in the [production target](09-production-target.md).

| Area | Implementation |
|---|---|
| Passwords | Argon2id |
| Sessions | HS256 JWT, 30 minutes, no refresh token |
| Roles | Operator, supervisor, responder, hospital, auditor, admin |
| Data scope | Responders see their agency, hospitals their facility |
| Service access | One shared `NADJET_INGEST_TOKEN` for `/internal/*` |
| Streams | MediaMTX asks the API to authorise each request |
| SMS webhooks | HMAC signature with a time window, or a shared secret token |
| Public endpoints | Per-IP rate limits, plus global caps on sandbox work |
| Evidence | SHA-256, per-incident hash chain, Ed25519 signature |
| Audit | Append-only tables enforced by triggers |
| AI endpoint | Loopback or allow-listed Brev host |

## Stream access

| Caller | Credential | Allowed |
|---|---|---|
| Workers, camsim | Service user and ingest token | Publish, read, control |
| Operator, supervisor, admin | Stream token, 5 minutes, renewed by the player | Read |
| Try-it phone | Publish token for its own path | Publish that path |
| Anyone else | None | Rejected |

## Integrity rules enforced by CI

`scripts/check-integrity.sh` fails the build on:

1. Random values in service or package source, which could fabricate results (FR-VIS-020).
2. Any AI provider other than NVIDIA Brev.
3. Frontend test files, which project policy forbids.
4. Use of the held-out list outside the catalogue validators and `eval/run_eval.py` (FR-VIS-022).

## Known gaps

The shared service token, the single signing key in an environment variable, and the lack of refresh tokens are PoC simplifications. See [Status and limits](../Overview/03-status-and-limits.md).
