# 03 — Status and limits

This page separates what exists from what doesn't. The numbers come from the repository, not from new measurements.

## What exists

| Area | State |
|---|---|
| Detection | Works on 4 simulated cameras from video files; real cameras are not connected |
| AI | One worker per camera, verifier and sweep, on one Brev GPU |
| Coordination | Modular API with 72 HTTP operations, consumer and jobs worker |
| Web | Operator, responder, hospital and public surfaces in Arabic, French and English |
| Field | Two-way SMS through one Android phone, plus Web Push |
| Tests | 67 Python test files, with no frontend tests by policy |

## What is not proven

- `docs/demo/RESULTS.md` holds no latency, accuracy or false-alarm results yet.
- AI thresholds (0.8, 0.5, 0.5) are untuned starting values (TQ-07).
- TD-50 measured one verifier call at 6.2–6.7 s alone on the L40S, and about 11 s each with two at once. Cameras-per-GPU capacity is unknown.
- At those speeds, four cameras sweeping every 8 s may exceed verifier capacity. This is an estimate, and sweep clips are smaller than verifier clips.

## Known limitations

From a code review on 2026-10-31. "Probably" means it needs checking on the deployed system.

| # | Limitation | Severity |
|---|---|---|
| 1 | The video path crosses the public internet, which can't be used with real surveillance cameras | Blocks production |
| 2 | AI runs on rented foreign-owned cloud, and the server location is unconfirmed | Blocks production |
| 3 | Candidates are lost after about 15.5 s of failed posts or on a worker restart | High |
| 4 | One VPS, one GPU instance and one SMS phone are single points of failure | High |
| 5 | Operators are logged out every 30 minutes, with no refresh token | High |
| 6 | Fire, rollover and vehicle counts from AI don't feed priority | High |
| 7 | An undelivered SMS warns the operator only after 450 s | High |
| 8 | Public rate limits are probably shared behind the proxy | High |
| 9 | `/system/ai` probably reports unavailable in production | Medium |
| 10 | Evidence retention makes the integrity check report invalid | Medium |
| 11 | Admin camera edits don't reach running workers | Medium |
| 12 | The evaluation applies a veto that live operation doesn't | Medium |
| 13 | A dead worker raises no active alert | Medium |
| 14 | The event chain can't detect tail truncation | Medium |

## Coverage gaps

A road with no camera and no mobile signal has no reporting channel, and a crash with no camera and no witness goes undetected. See [Coverage model](../Architecture/07-coverage-model.md).

## Legal status

Part 2 of the [production target](../Architecture/09-production-target.md) lists the Algerian texts that shape the design. That reading is an engineering input. Algerian counsel must confirm it before any deployment.
