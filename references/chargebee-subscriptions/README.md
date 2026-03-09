# Chargebee Subscriptions Action Reference

**Provider:** `chargebee`
**Service:** `subscriptions`
**App name:** `chargebee-subscriptions`
**Auth:** API Key

## Actions

### list_subscriptions

List subscriptions with optional filters and pagination.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "chargebee",
    "service": "subscriptions",
    "action": "list_subscriptions",
    "params": {
      "limit": 20,
      "offset": 0,
      "status": "active"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | integer | no | Maximum subscriptions to return |
| `offset` | integer | no | Number of subscriptions to skip |
| `status` | string | no | Filter by status (e.g. active, cancelled) |

### list_customers

List customers with optional pagination.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "chargebee",
    "service": "subscriptions",
    "action": "list_customers",
    "params": {"limit": 20, "offset": 0}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | integer | no | Maximum customers to return |
| `offset` | integer | no | Number of customers to skip |
