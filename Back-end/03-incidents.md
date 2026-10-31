# 03 — Incidents

Code: `services/api/src/nadjet_api/incidents/`.

## Ingest path

1. `POST /internal/candidates` validates the service token and requires an `Idempotency-Key`.
2. `ingest.py` writes the candidate to a Redis Stream and answers straight away.
3. The `consume` process reads with a consumer group, merges or creates the incident, and appends a signed event.
4. It acknowledges with `XACK` **only after the database commit**, then publishes the change.
5. Entries left pending are re-claimed after `NADJET_CANDIDATE_CLAIM_IDLE_MS`.

If the consumer is down, candidates wait in the stream and are saved when it restarts.

## Key files

| File | Role |
|---|---|
| `internal.py` | Worker-facing endpoints |
| `ingest.py` | Stream writer and consumer loop |
| `service.py` | Create, merge, verify, reject, evidence |
| `lifecycle.py` | State machine |
| `merge.py` | Merge rules |
| `priority.py` | Explainable priority |
| `signing.py` | Ed25519 signing |
| `store.py` | `EvidenceStore` protocol and local implementation |
| `events.py` | Live change publishing and the SSE stream |
| `shortcodes.py` | Crockford base32 short codes |

## Live updates

`GET /events` sends only the type, incident ID and version. Clients then fetch through the API, which applies the user's data scope.

## Integrity

Every event links to the previous hash and is signed. Evidence is hashed with SHA-256.

See also [Incident lifecycle](../Architecture/03-incident-lifecycle.md).
