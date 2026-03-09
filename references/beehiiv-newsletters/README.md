# Beehiiv Newsletters Action Reference

**Provider:** `beehiiv`
**Service:** `newsletters`
**App name:** `beehiiv-newsletters`
**Auth:** API Key

## Actions

### list_publications

List all publications in the account.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "beehiiv",
    "service": "newsletters",
    "action": "list_publications",
    "params": {}
  }'
```

### list_subscribers

List subscribers for a publication.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "beehiiv",
    "service": "newsletters",
    "action": "list_subscribers",
    "params": {
      "publication_id": "pub_abc123",
      "page": 1,
      "limit": 100
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `publication_id` | string | yes | Publication ID |
| `page` | integer | no | Page number for pagination |
| `limit` | integer | no | Subscribers per page |
