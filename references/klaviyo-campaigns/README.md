# Klaviyo Campaigns Action Reference

**Provider:** `klaviyo`
**Service:** `campaigns`
**App name:** `klaviyo-campaigns`
**Auth:** API Key

## Actions

### list_campaigns

List email campaigns with optional pagination.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "klaviyo",
    "service": "campaigns",
    "action": "list_campaigns",
    "params": {"page_size": 50}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `page_size` | integer | no | Number of campaigns per page |

### get_campaign

Get a campaign by ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "klaviyo",
    "service": "campaigns",
    "action": "get_campaign",
    "params": {"campaign_id": "01HXYZ123ABC"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `campaign_id` | string | yes | Campaign ID |

### list_lists

List Klaviyo lists.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "klaviyo",
    "service": "campaigns",
    "action": "list_lists",
    "params": {"page_size": 50}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `page_size` | integer | no | Number of lists per page |
