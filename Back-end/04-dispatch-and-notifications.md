# 04 — Dispatch and notifications

## Dispatch

1. `recommend` (`dispatch/service.py`) finds units and hospitals that fit the incident's needs and ranks them by road ETA from OSRM.
2. The operator selects them.
3. `dispatch` writes the assignment, moves the status and queues alerts in one transaction.
4. If a hospital declares Divert, `handle_hospital_divert` deals with the assigned incidents, and `change_hospital` changes the destination.

> The PoC refuses dispatch when OSRM is down. The production design allows manual dispatch without an ETA.

## Notifications

Channels implement the `NotificationChannel` protocol in `notifications/channels.py`.

| Channel | Implementation | Delivery confirmed by |
|---|---|---|
| SMS | capcom6 `POST /api/3rdparty/v1/messages` | Webhook |
| Web Push | `pywebpush` with VAPID | Accepted by the push service |
| Telegram | Optional | Accepted |

An arq job runs the delivery. Retries wait 30 s and double, up to 3 retries, then a `notification_failed` event alerts the operator (TD-40). The `notifications` table has a unique row per incident, kind, channel and recipient.

## SMS grammar

`contracts/sms-grammar.md` is the source of truth, and `notifications/grammar.py` implements it. Each outbound message is one SMS of at most 160 GSM-7 characters.

### Outbound

| Message | Meaning |
|---|---|
| `ACC\|P<n>\|<lat>,<lon>\|<road> PK<km>\|<k>V\|<code>` | Alert to a responder |
| `PRE\|P<n>\|<lat>,<lon>\|<road> PK<km>\|<k>V\|E<min>\|<code>` | Hospital pre-alert |

### Inbound

| Message | Effect |
|---|---|
| `OK`, `ER`, `OS`, `TR`, `AH`, `CL` + `<code>` | Status: acknowledged, en route, on scene, transporting, at hospital, closed |
| `T <code> R# Y# G# B#` | Triage counts |
| `H <code> READY` or `DIVERT` | Hospital state |
| `ACCIDENT <lat>,<lon> [P<n>] [C<0\|1>] [text]` | Citizen report at the coordinates |
| `ACCIDENT <text>` | Citizen report, flagged for call-back |

Inbound SMS is deduplicated by message ID and limited to 30 messages per phone per 10 minutes.

## CAP export

`notifications/cap.py` builds CAP 1.2 with the standard library.
