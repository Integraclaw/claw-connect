# Pipedrive Deals Action Reference

**Provider:** `pipedrive`
**Service:** `deals`
**App name:** `pipedrive-deals`

## Actions

### list_deals

List deals with pagination.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "pipedrive",
    "service": "deals",
    "action": "list_deals",
    "params": {"limit": 50, "start": 0}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | integer | no | Maximum deals to return |
| `start` | integer | no | Pagination start offset |

### get_deal

Get a deal by ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "pipedrive",
    "service": "deals",
    "action": "get_deal",
    "params": {"deal_id": "1"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `deal_id` | string | yes | Deal ID |

### create_deal

Create a new deal.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "pipedrive",
    "service": "deals",
    "action": "create_deal",
    "params": {
      "title": "Acme Corp - Enterprise License",
      "value": 25000,
      "currency": "USD",
      "person_id": 123
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `title` | string | yes | Deal title |
| `value` | number | no | Deal value |
| `currency` | string | no | Currency code (e.g. USD, BRL) |
| `person_id` | integer | no | Associated person ID |

### update_deal

Update an existing deal.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "pipedrive",
    "service": "deals",
    "action": "update_deal",
    "params": {
      "deal_id": "1",
      "title": "Updated Deal Title",
      "value": 30000,
      "status": "won"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `deal_id` | string | yes | Deal ID |
| `title` | string | no | New deal title |
| `value` | number | no | New deal value |
| `status` | string | no | Deal status |

### search_deals

Search deals by term.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "pipedrive",
    "service": "deals",
    "action": "search_deals",
    "params": {"term": "Acme"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `term` | string | yes | Search term |

## Resources

- [Pipedrive API Reference](https://developers.pipedrive.com/docs/api/v1)
