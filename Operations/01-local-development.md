# 01 — Local development

## Requirements

Docker with Compose, uv 0.9+, Node 22 or later with pnpm 10, GNU make, the GitHub CLI and the Brev CLI. `make doctor` checks them.

## First run

```bash
git clone git@github.com:Nadjeet/nadjet.git && cd nadjet
make setup
make dev
```

| Service | Address |
|---|---|
| API docs | http://localhost:8080/docs |
| API readiness | http://localhost:8080/ready |
| Web | http://localhost:5173 |
| MediaMTX RTSP | `rtsp://localhost:8554/<camera-id>` |
| MediaMTX WebRTC | `http://localhost:8889/<camera-id>` |
| OSRM | `localhost:55000` |
| PostGIS | `localhost:55432` |

## Common commands

| Command | Does |
|---|---|
| `make infra` | Starts PostGIS and Redis |
| `make infra-video` | Starts simulated cameras |
| `make infra-sms` | Starts the SMS gateway server |
| `make migrate` | Applies migrations |
| `make seed-cameras`, `make seed-geo` | Loads cameras, units and facilities |
| `make map-data`, `make routing-data` | Downloads map and routing data, once |
| `make routing-up` | Starts OSRM |
| `make gen` | Regenerates contracts and the TS client |
| `make test`, `make lint`, `make typecheck` | Checks |
| `make help` | Lists everything |

## Simulating failures

`make camsim-freeze CAM=<id>`, `make camsim-outage CAM=<id>`, `make camsim-restore CAM=<id>` and `make camsim-swap CAM=<id> VIDEO=<file>` drive the simulated cameras.

## AI

`make brev-forward` maps `localhost:18000` to the instance, and `make brev-check` tests it. Starting or stopping Brev instances spends credits, and needs team approval.
