# 05 — Auth and access

Code: `services/api/src/nadjet_api/auth/`.

## Login

`POST /auth/login` checks an Argon2id hash and returns a JWT signed with HS256. Tokens last 30 minutes (`NADJET_ACCESS_TOKEN_TTL_MINUTES`). There is no refresh token.

`NADJET_JWT_SECRET` is required in production, at 32 characters or more, and the app refuses to start without it.

## Roles and binding

`validate_role_binding` enforces these rules:

| Role | Must have |
|---|---|
| `responder` | An agency and no facility |
| `hospital` | A facility and no agency |
| Others | Neither |

## Who can do what

From `require_roles` in the route modules. This covers the main groups, not every route.

| Action | Roles |
|---|---|
| Verify, reject, dispatch | Operator, supervisor, admin |
| View incident integrity | Operator, supervisor, admin, auditor |
| KPIs and evaluation | Supervisor, admin |
| Watch camera streams | Console roles, through a stream token |
| Admin routes | Admin |

## Scope

`Scope` filters queries. Unrestricted roles see everything. Otherwise results are limited to one agency or facility.

## Service authentication

Workers call `/internal/*` with `Authorization: Bearer $NADJET_INGEST_TOKEN`. Rotating it means updating the VPS and restarting the Brev workers.

## Streams

`POST /auth/stream-token` issues a short-lived token for console roles. MediaMTX calls `POST /internal/mediamtx-auth` for each publish, read or API request. See [Security](../Architecture/05-security.md).
