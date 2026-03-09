# Baserow Databases Action Reference

**Provider:** `baserow`
**Service:** `databases`
**App name:** `baserow-databases`
**Auth:** API Key

## Actions

### list_tables

List tables in a database.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "baserow",
    "service": "databases",
    "action": "list_tables",
    "params": {"database_id": 123}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `database_id` | integer | yes | Database ID |

### list_rows

List rows in a table.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "baserow",
    "service": "databases",
    "action": "list_rows",
    "params": {
      "table_id": 456,
      "page": 1,
      "size": 100
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `table_id` | integer | yes | Table ID |
| `page` | integer | no | Page number for pagination |
| `size` | integer | no | Rows per page |
