# 01 — Code map

Root: `services/api/src/nadjet_api/`.

| Module | Responsibility |
|---|---|
| `main.py` | App factory, lifespan, router registration |
| `cli.py` | `serve`, `consume`, `worker`, seed and key commands |
| `config.py` | `NADJET_*` settings |
| `auth/` | Users, login, JWT, roles, scope |
| `audit/` | Append-only action log |
| `incidents/` | Candidates, lifecycle, merge, priority, events, signing, evidence |
| `dispatch/` | Needs, recommendations, dispatch, hospital changes |
| `routing/` | OSRM client, ranking, route endpoint |
| `notifications/` | Channels, retries, SMS grammar, inbound SMS, CAP, VAPID, arq jobs |
| `citizen/` | Public report endpoints |
| `hospital/` | Board and Ready or Divert |
| `cameras/` | Registry, health, internal health endpoint |
| `streams/` | MediaMTX auth and stream tokens |
| `geo/` | Facilities, units, map resources, tiles |
| `kpis/` | Response-time KPIs |
| `evaluation/` | Serves the latest evaluation result |
| `tryit/` | Public sandbox |
| `admin/` | Admin operations |
| `system/` | System and AI status |
| `retention.py` | Purge of expired data, on a loop in the API |
| `ratelimit.py` | Redis rate limits |
| `migrations/` | Alembic |

Shared contracts live in `packages/shared/src/nadjet_shared/`: `events.py`, `health.py`, `tryit.py`, `catalogue.py`, `evaluation.py`.

## Conventions

- Async I/O, typed with `mypy --strict`, formatted by Ruff.
- Pydantic v2 models at boundaries.
- Pure logic stays in plain functions, such as `lifecycle.py`, `priority.py` and `need.py`, so it can be unit tested.
- After changing routes or models, run `make gen` and commit the generated files.
