# Google Sheets Action Reference

**Provider:** `google`
**Service:** `sheets`
**App name:** `google-sheets`

## Actions

### get_spreadsheet

Get spreadsheet metadata (sheets, properties).

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "sheets",
    "action": "get_spreadsheet",
    "params": {"spreadsheet_id": "SPREADSHEET_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `spreadsheet_id` | string | yes | Spreadsheet ID |

### read_range

Read values from a range.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "sheets",
    "action": "read_range",
    "params": {
      "spreadsheet_id": "SPREADSHEET_ID",
      "range": "Sheet1!A1:D10"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `spreadsheet_id` | string | yes | Spreadsheet ID |
| `range` | string | yes | A1 notation range (e.g. `Sheet1!A1:D10`) |

### write_range

Write values to a range.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "sheets",
    "action": "write_range",
    "params": {
      "spreadsheet_id": "SPREADSHEET_ID",
      "range": "Sheet1!A1",
      "values": [["Name", "Email"], ["John", "john@example.com"]]
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `spreadsheet_id` | string | yes | Spreadsheet ID |
| `range` | string | yes | A1 notation range (e.g. `Sheet1!A1`) |
| `values` | array | yes | 2D array of values (rows of columns) |

### append_rows

Append rows after a range.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "sheets",
    "action": "append_rows",
    "params": {
      "spreadsheet_id": "SPREADSHEET_ID",
      "range": "Sheet1!A1",
      "values": [["New Row 1", "Data"], ["New Row 2", "More Data"]]
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `spreadsheet_id` | string | yes | Spreadsheet ID |
| `range` | string | yes | A1 range to append after (e.g. `Sheet1!A1`) |
| `values` | array | yes | 2D array of row values to append |

### clear_range

Clear values from a range.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "sheets",
    "action": "clear_range",
    "params": {
      "spreadsheet_id": "SPREADSHEET_ID",
      "range": "Sheet1!A1:D10"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `spreadsheet_id` | string | yes | Spreadsheet ID |
| `range` | string | yes | A1 notation range to clear |

### create_spreadsheet

Create a new spreadsheet.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "sheets",
    "action": "create_spreadsheet",
    "params": {"title": "New Spreadsheet"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `title` | string | yes | Spreadsheet title |

## Range Notation

- `Sheet1!A1:D10` — Specific range
- `Sheet1!A:D` — Entire columns A through D
- `Sheet1!1:10` — Entire rows 1 through 10
- `Sheet1` — Entire sheet
- `A1:D10` — Range in first sheet

## Resources

- [Google Sheets API Reference](https://developers.google.com/workspace/sheets/api/reference/rest)
