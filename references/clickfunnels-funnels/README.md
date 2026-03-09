# ClickFunnels Funnels Action Reference

**Provider:** `clickfunnels`
**Service:** `funnels`
**App name:** `clickfunnels-funnels`
**Auth:** API Key

## Actions

### list_workspaces

List all workspaces in the account.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "clickfunnels",
    "service": "funnels",
    "action": "list_workspaces",
    "params": {}
  }'
```

### list_contacts

List contacts for a workspace.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "clickfunnels",
    "service": "funnels",
    "action": "list_contacts",
    "params": {
      "workspace_id": "12345",
      "page": 1
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `workspace_id` | string | yes | Workspace ID |
| `page` | integer | no | Page number for pagination |
