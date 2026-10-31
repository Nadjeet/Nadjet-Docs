# 02 — Web architecture

| Concern | Choice | Where |
|---|---|---|
| Framework | React 19, Vite, strict TypeScript | `apps/web` |
| Routing | TanStack Router | `src/router.tsx` |
| Server state | TanStack Query | `src/lib/*-queries.ts` |
| API calls | Generated `@nadjet/api-client` only | `packages/api-client` |
| Live updates | SSE, which triggers refetches | `src/live/` |
| Video | WebRTC (WHEP), LL-HLS fallback through hls.js | `src/components/stream/` |
| Map | MapLibre with offline PMTiles | `src/map/` |
| UI | Tailwind, Radix, shadcn-style parts | `src/components/ui/` |
| Languages | Arabic, French, English, with i18next and RTL support | `src/locales/` |
| PWA | `vite-plugin-pwa`, `push-sw.js` for push | `vite.config.ts`, `public/` |
| Offline actions | Queue with an idempotency key per action (FR-RSP-004) | `src/lib/offline-queue.ts` |

## How the API is reached

The client uses the base URL `/api`. nginx proxies it to the API with buffering off, so SSE works. The API is on the same origin, and a separate `api.` host serves workers and webhooks.

## Rules

- No frontend tests of any kind; CI fails if any appear. Validate with `make typecheck`, `make lint` and `pnpm build`.
- API calls go only through `@nadjet/api-client`.
- Light mode only, following the Figma design system (TD-54, TD-55).
