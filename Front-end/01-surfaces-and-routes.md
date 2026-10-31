# 01 — Surfaces and routes

Routes are defined in `apps/web/src/router.tsx`.

| Surface | Route | Screen |
|---|---|---|
| Entry | `/`, `/login` | Landing and sign-in |
| Operations | `/ops` | Console: queue and map |
| | `/ops/incidents`, `/ops/incidents/$incidentId` | Incident list and detail |
| | `/ops/cameras` | Camera wall |
| | `/ops/system` | System status |
| | `/ops/kpis` | KPIs |
| | `/ops/evaluation` | Evaluation results |
| | `/ops/admin` | Admin |
| Responder | `/respond` | Mission list |
| | `/respond/incidents/$incidentId` | Mission detail |
| | `/respond/progress`, `/respond/triage`, `/respond/profile` | Status, triage, profile |
| Hospital | `/hospital` | Board with pre-alerts and readiness |
| Public | `/public` | Home |
| | `/public/report` | Crash report |
| | `/public/try`, `/public/try/live` | Clip upload and live phone sandbox |
| | `/public/privacy` | Privacy |

The hostnames `app.`, `respond.`, `hospital.` and `try.` all serve this one app.
