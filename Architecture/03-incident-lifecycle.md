# 03 — Incident lifecycle

## From camera to field

```mermaid
sequenceDiagram
  participant W as Vision worker
  participant V as vLLM
  participant A as API
  participant R as Redis
  participant C as Consumer
  participant P as PostGIS
  participant O as Operator
  participant J as Jobs worker
  participant F as Field phone

  W->>V: Clip and prompt
  V-->>W: JSON verdict
  W->>A: POST /internal/candidates
  A->>R: XADD
  R-->>C: Deliver entry
  C->>P: Merge or create, append signed event
  C->>R: XACK, then publish change
  R-->>A: Pub/Sub change
  A-->>O: SSE with id and version
  O->>A: Fetch, then verify
  W->>A: Upload clip and keyframes
  O->>A: Dispatch units and hospital
  A->>P: One transaction
  A->>R: Enqueue notifications
  R-->>J: Delivery job
  J->>F: SMS and Web Push
  F->>A: Status and triage
```

The worker verifies a rule candidate **before** posting it, so the operator doesn't see an unverified status flip. If the AI is unavailable, the candidate still goes out, labelled.

## State machine

From `services/api/src/nadjet_api/incidents/lifecycle.py`.

```mermaid
stateDiagram-v2
  [*] --> candidate
  candidate --> verified
  candidate --> rejected
  candidate --> merged
  verified --> dispatched
  dispatched --> acknowledged
  acknowledged --> en_route
  en_route --> on_scene
  on_scene --> transporting
  on_scene --> closed
  transporting --> at_hospital
  at_hospital --> closed
  rejected --> [*]
  merged --> [*]
  closed --> [*]
```

Reject reasons are `false_positive`, `duplicate`, `not_emergency` and `other`.

## Merging

An open incident absorbs a new report that is close in space and time: within 50 m (`MERGE_RADIUS_M`) and, for different sources, within 60 s (`MERGE_WINDOW_S`). Reports from the same camera use their own window. See `incidents/merge.py`.

## Priority

Priority runs from P1, the most urgent, to P4 (`incidents/priority.py`). Each fired rule sets a minimum, and the final level is the most urgent minimum. Night, highway and an AI raise add at most one level together, and the AI never lowers it. Triage, once present, replaces the AI raise and the context bump. A bus involved, or five or more victims, marks a mass-casualty incident.

Each priority keeps its reasons, with their rule or model source.

> Some signals are not fed yet. See limitation 6 in [Status and limits](../Overview/03-status-and-limits.md).

## Dispatch needs

From `dispatch/need.py`:

| Situation | Agencies that can respond | Hospital capability |
|---|---|---|
| Normal | Civil Protection, SAMU | `emergency` |
| Priority 1–2 or mass casualty | Civil Protection, SAMU | `trauma` |
| Someone trapped | Civil Protection only | `trauma` |
