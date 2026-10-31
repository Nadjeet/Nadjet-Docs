# 04 — Testing and CI

## Policy

| Area | Rule |
|---|---|
| Backend, vision, packages | Unit and integration tests are required, with pytest |
| Frontend | **Never create tests.** Validate with type checks, lint and build |
| Live AI | `@pytest.mark.brev`, run with `make test-brev`, never in CI |

Integration tests use real PostGIS and Redis, the `nadjet_test` database and Redis db 15. They fail, rather than skip, when infrastructure is down.

## Definition of done

```bash
make format && make lint && make typecheck && make check-contracts && make test
```

## CI jobs (`.github/workflows/ci.yml`)

| Job | Does |
|---|---|
| `python` | Lint, types, contracts, single Alembic head, migrations, tests |
| `web` | Client up to date, lint, types, format, build |
| `rules` | Integrity script, traceability report, commit message check |
| `vision-image` | Builds and publishes the vision image |
| `app-images` | Builds and publishes the API and web images |

## Evaluation

`make eval` replays held-out clips through the real pipeline on Brev and writes `eval/results`. Demo clips come from the held-out list and are never used for tuning. See [AI, evaluation](../AI/08-evaluation-deployment-configuration.md).

## Git workflow

Branch from `dev`, open PRs to `dev`, and use one-line Conventional Commits with no co-author trailers. Direct commits to `dev` or `main` are blocked by hooks.
