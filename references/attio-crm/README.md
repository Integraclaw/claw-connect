# Attio CRM Action Reference

**Provider:** `attio`
**Service:** `crm`
**App name:** `attio-crm`
**Auth:** API Key

## Actions

### list_objects

List available objects (e.g. people, companies) in the CRM schema.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "attio",
    "service": "crm",
    "action": "list_objects",
    "params": {}
  }'
```

### list_records

List records for an object (e.g. people or companies).

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "attio",
    "service": "crm",
    "action": "list_records",
    "params": {
      "object": "people",
      "limit": 50
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `object` | string | yes | Object type (e.g. "people", "companies") |
| `limit` | integer | no | Maximum records to return |

### get_record

Get a single record by object and record ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "attio",
    "service": "crm",
    "action": "get_record",
    "params": {
      "object": "people",
      "record_id": "rec_abc123xyz"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `object` | string | yes | Object type (e.g. "people", "companies") |
| `record_id` | string | yes | Record ID |
