# 02 — Glossary

| Term | Meaning |
|---|---|
| **Candidate** | A suspicious event sent to the API. Not yet a validated incident. |
| **Incident** | The record the operators work on. Created or merged from candidates and reports. |
| **Short code** | The 3-character code that identifies an incident in SMS, such as `7F3`. |
| **Rule (trigger)** | A geometric test on tracked objects: `collision_candidate`, `hard_stop`, `stalled_vehicle`, `pedestrian_on_road`. |
| **Sweep** | A periodic AI look at a camera, independent of the rules. Default every 8 s per camera. |
| **Verifier** | The AI step that reviews a clip after a rule fires. |
| **AI status** | `confirmed`, `probable`, `possible` or `insufficient_evidence`. |
| **Worker** | One process that analyses one camera. |
| **vLLM** | The server that hosts the AI model. |
| **Qwen3-VL** | The vision-language model, `Qwen3-VL-8B-Instruct-FP8`. |
| **Brev** | The NVIDIA GPU instance. The only allowed place for AI inference. |
| **Edge node** | A proposed on-premise GPU server. The Brev instance simulates one. |
| **MediaMTX** | The media server that receives and serves video. |
| **WHIP / WHEP** | Browser protocols to publish and to watch video over WebRTC. |
| **RTSP** | The stream protocol used between MediaMTX and workers. |
| **SRT** | A protocol for lossy links, proposed for production and off in the PoC. |
| **SSE** | Server-Sent Events: one-way live updates to the browser. |
| **PWA** | A web app that can be installed and work partly offline. |
| **OSRM** | The self-hosted routing engine, used for road ETAs. |
| **PK** | Point kilométrique: a kilometre marker on a road. |
| **PostGIS** | The geographic extension of PostgreSQL. |
| **Triage tags** | Red, yellow, green and black casualty categories. |
| **CAP** | Common Alerting Protocol, used for the XML incident export. |
| **Outbox** | A proposed local queue that holds data on an edge node until the core confirms it. |
| **ANF, ANPDP** | Algerian frequency agency, and the Algerian personal data authority. |
| **FR, NFR, SC** | Requirement IDs in `PRD.md`. |
| **TD** | Technical decision ID in `docs/11-technical-decisions.md`. |
