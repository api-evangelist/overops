---
name: Triage production errors with OverOps
description: Find the highest-impact errors captured in an OverOps environment, open their code snapshots, and resolve or label them.
api: openapi/overops-openapi-original.json
operations:
- GET /services
- GET /services/{env_id}/views
- GET /services/{env_id}/views/{view_id}/events
- GET /services/{env_id}/events/{event_id}
- GET /services/{env_id}/events/{event_id}/snapshot
- POST /services/{env_id}/events/{event_id}/resolve
- POST /services/{env_id}/events/{event_id}/labels
---

# Triage production errors with OverOps

Base URL: `https://api.overops.com/api/v1`
Auth: send an `X-API-KEY` header (generate under Settings -> Account Settings), or HTTP Basic. X-API-KEY is recommended.

## Steps

1. **Pick the environment.** `GET /services` lists every environment (service) your key can access. Environment IDs look like `S1234`; use it as `{env_id}` below.
2. **Choose a view.** `GET /services/{env_id}/views` lists saved views (filters). Views like "All Events" or "New in Last Deployment" scope which errors you see.
3. **List the errors.** `GET /services/{env_id}/views/{view_id}/events` returns the events in the view. Narrow the window with the `from`/`to` time parameters and filter with `server`, `app`, or `deployment`.
4. **Inspect an event.** `GET /services/{env_id}/events/{event_id}` returns the event data; `GET /services/{env_id}/events/{event_id}/snapshot` returns the captured stack, source, and variable state for root-cause analysis.
5. **Act on it.** `POST /services/{env_id}/events/{event_id}/resolve` marks it resolved once fixed, or `POST /services/{env_id}/events/{event_id}/labels` to tag it for routing.

## Notes
- Errors are HTTP status codes (401 auth, 403 permission, 404 missing resource) — not `application/problem+json`. See `errors/overops-problem-types.yml`.
- There is no pagination or idempotency-key contract; scope large result sets with `from`/`to`.
