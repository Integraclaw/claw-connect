# Apollo Prospecting Action Reference

**Provider:** `apollo`
**Service:** `prospecting`
**App name:** `apollo-prospecting`
**Auth:** API Key

## Actions

### search_people

Search for people with optional organization or keyword filters.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "apollo",
    "service": "prospecting",
    "action": "search_people",
    "params": {
      "q_organization_name": "Acme Inc",
      "q_keywords": "VP Sales",
      "page": 1,
      "per_page": 25
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `q_organization_name` | string | no | Filter by organization name |
| `q_keywords` | string | no | Search keywords (e.g. job title) |
| `page` | integer | no | Page number for pagination |
| `per_page` | integer | no | Results per page |

### get_person

Get a person by ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "apollo",
    "service": "prospecting",
    "action": "get_person",
    "params": {"person_id": "5f9a1b2c3d4e5"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `person_id` | string | yes | Person ID |

### search_organizations

Search for organizations by name.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "apollo",
    "service": "prospecting",
    "action": "search_organizations",
    "params": {
      "q_organization_name": "Stripe",
      "page": 1,
      "per_page": 25
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `q_organization_name` | string | yes | Organization name to search |
| `page` | integer | no | Page number for pagination |
| `per_page` | integer | no | Results per page |
