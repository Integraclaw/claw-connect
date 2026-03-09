# Airtable Bases Action Reference

**Provider:** `airtable`
**Service:** `bases`
**App name:** `airtable-bases`

## Actions

### list_bases

List accessible Airtable bases.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "airtable",
    "service": "bases",
    "action": "list_bases",
    "params": {}
  }'
```

No parameters required.

### list_records

List records from a table.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "airtable",
    "service": "bases",
    "action": "list_records",
    "params": {
      "base_id": "appXXXXXXXXXXXXXX",
      "table_name": "Tasks",
      "max_records": 100,
      "view": "Grid view"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `base_id` | string | yes | Base ID |
| `table_name` | string | yes | Table name or ID |
| `max_records` | integer | no | Maximum records to return |
| `view` | string | no | View name for filtering/sorting |

### get_record

Get a record by ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "airtable",
    "service": "bases",
    "action": "get_record",
    "params": {
      "base_id": "appXXXXXXXXXXXXXX",
      "table_name": "Tasks",
      "record_id": "recXXXXXXXXXXXXXX"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `base_id` | string | yes | Base ID |
| `table_name` | string | yes | Table name or ID |
| `record_id` | string | yes | Record ID |

### create_records

Create one or more records.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "airtable",
    "service": "bases",
    "action": "create_records",
    "params": {
      "base_id": "appXXXXXXXXXXXXXX",
      "table_name": "Tasks",
      "records": [
        {"fields": {"Name": "New Task", "Status": "To Do"}}
      ]
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `base_id` | string | yes | Base ID |
| `table_name` | string | yes | Table name or ID |
| `records` | array | yes | Array of record objects with `fields` |

### update_records

Update one or more records.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "airtable",
    "service": "bases",
    "action": "update_records",
    "params": {
      "base_id": "appXXXXXXXXXXXXXX",
      "table_name": "Tasks",
      "records": [
        {"id": "recXXXXXXXXXXXXXX", "fields": {"Status": "Done"}}
      ]
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `base_id` | string | yes | Base ID |
| `table_name` | string | yes | Table name or ID |
| `records` | array | yes | Array of record objects with `id` and `fields` |

## Resources

- [Airtable API Reference](https://airtable.com/developers/web/api/introduction)
