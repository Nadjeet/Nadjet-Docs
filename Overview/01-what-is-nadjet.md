# 01 — What is Nadjet

Nadjet (نجاة) connects road-crash detection to the emergency response. It answers one question: once a crash happens, how does the right help reach it, and how does everyone involved know what is going on?

## The chain

1. **Detect.** AI watches camera video. Citizens can report a crash from a web page or by SMS, and an operator can create an incident from a phone call.
2. **Verify.** A second AI step reviews a short clip and gives a graded status. An operator then decides.
3. **Dispatch.** The system ranks units and hospitals by road travel time. The operator chooses.
4. **Alert.** Units and the hospital are told by SMS and Web Push.
5. **Follow up.** Responders report status and triage through the app or by SMS, and the hospital can answer Ready or Divert.

**A person verifies before dispatch.** No automation path skips that step.

## Who uses it

| Role | Surface | Main work |
|---|---|---|
| Operator | `/ops` | Watches the queue and cameras, verifies, dispatches |
| Supervisor | `/ops` | Operator work, plus KPIs and evaluation |
| Admin | `/ops/admin` | Admin operations, plus operator work |
| Auditor | `/ops` | Oversight, with viewing roles only |
| Responder | `/respond` | Receives missions, reports status and triage |
| Hospital | `/hospital` | Sees pre-alerts, sets Ready or Divert |
| Citizen | `/public` | Reports a crash, tries the AI on a clip |

Roles are defined in `services/api/src/nadjet_api/auth/roles.py`. Responders are bound to one agency and hospital users to one facility. Agencies are Civil Protection, Gendarmerie, Police and SAMU.

## What makes it different

- **Multiple ways in.** Cameras cover few roads, so citizen and operator channels feed the same pipeline. See [Coverage model](../Architecture/07-coverage-model.md).
- **Works on weak networks.** SMS is a full channel, and the responder app queues actions offline.
- **Honest AI.** Every AI value carries its source, statuses are graded, and failures show as "AI unavailable". The code has no fallback to another provider.
- **Traceable records.** Incident events form a signed hash chain, and evidence files have SHA-256 hashes.

## Where to go next

- [Glossary](02-glossary.md)
- [Status and limits](03-status-and-limits.md)
- [System overview](../Architecture/01-system-overview.md)
