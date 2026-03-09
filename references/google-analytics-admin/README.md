# Google Analytics Admin Action Reference

**Provider:** `google`
**Service:** `analytics-admin`
**App name:** `google-analytics-admin`

## Actions

### list_accounts

List all GA4 accounts accessible to the authenticated user.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "analytics-admin",
    "action": "list_accounts",
    "params": {"page_size": 50, "page_token": "NEXT_PAGE"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `page_size` | integer | no | Number of accounts per page |
| `page_token` | string | no | Pagination token from previous response |

### list_properties

List GA4 properties under an account.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "analytics-admin",
    "action": "list_properties",
    "params": {
      "filter": "parent:accounts/123456789",
      "page_size": 50
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `filter` | string | no | Filter expression (e.g. `parent:accounts/123456789`) |
| `page_size` | integer | no | Number of properties per page |

### get_property

Retrieve details for a GA4 property.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "analytics-admin",
    "action": "get_property",
    "params": {"property_id": "123456789"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `property_id` | string | yes | GA4 property ID (numeric) |

### list_data_streams

List data streams (web, iOS, Android) for a property.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "analytics-admin",
    "action": "list_data_streams",
    "params": {"property_id": "123456789"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `property_id` | string | yes | GA4 property ID |
