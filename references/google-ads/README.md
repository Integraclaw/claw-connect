# Google Ads Action Reference

**Provider:** `google`
**Service:** `ads`
**App name:** `google-ads`

## Actions

### search

Execute a GAQL (Google Ads Query Language) search against a customer account.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "ads",
    "action": "search",
    "params": {
      "customer_id": "123-456-7890",
      "query": "SELECT campaign.id, campaign.name, metrics.impressions, metrics.clicks FROM campaign WHERE segments.date DURING LAST_30_DAYS"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `customer_id` | string | yes | Google Ads customer ID (with or without hyphens) |
| `query` | string | yes | GAQL query string |

### list_customers

List all Google Ads customer accounts accessible to the authenticated user.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "ads",
    "action": "list_customers"
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| *none* | — | — | No parameters required |

### get_customer

Retrieve details for a specific customer account.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "ads",
    "action": "get_customer",
    "params": {"customer_id": "123-456-7890"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `customer_id` | string | yes | Google Ads customer ID |

## GAQL Examples

```sql
-- Campaign performance (last 30 days)
SELECT campaign.id, campaign.name, metrics.impressions, metrics.clicks, metrics.cost_micros
FROM campaign
WHERE segments.date DURING LAST_30_DAYS

-- Ad group metrics
SELECT ad_group.id, ad_group.name, metrics.conversions
FROM ad_group
WHERE campaign.id = 12345
```
