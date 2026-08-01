---
name: Configure OverOps view alerting
description: Create a view, attach alert settings to it, and test the alert delivery in an OverOps environment.
api: openapi/overops-openapi-original.json
operations:
- GET /services
- POST /services/{env_id}/views
- GET /services/{env_id}/views/{view_id}/alerts
- POST /services/{env_id}/views/{view_id}/alerts
- POST /services/{env_id}/alerts/test
- POST /services/{env_id}/views/{view_id}/custom-alert
---

# Configure OverOps view alerting

Base URL: `https://api.overops.com/api/v1`
Auth: `X-API-KEY` header (recommended) or HTTP Basic.

## Steps

1. **Select the environment.** `GET /services` -> pick the `{env_id}` (e.g. `S1234`).
2. **Create the view to alert on.** `POST /services/{env_id}/views` creates a saved view (filter) that defines which events trigger the alert.
3. **Attach alert settings.** `POST /services/{env_id}/views/{view_id}/alerts` sets the alert rule for the view; `GET /services/{env_id}/views/{view_id}/alerts` reads back the current settings.
4. **Verify delivery.** `POST /services/{env_id}/alerts/test` sends a test alert so you can confirm the channel (email/ServiceNow/etc.) is wired correctly.
5. **Send ad-hoc alerts (optional).** `POST /services/{env_id}/views/{view_id}/custom-alert` fires a one-off custom alert from a view.

## Notes
- Alerts and metrics can also be published outbound (`/settings/publish-metrics`, `/alerts/servicenow/tables`).
- Auth, error, and versioning conventions: see `conventions/overops-conventions.yml`.
