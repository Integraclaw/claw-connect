# Google Sheets API Reference

**App name:** `google-sheets`
**Provider:** `google` | **Service:** `sheets`
**Base URL:** `https://sheets.googleapis.com/v4`

## IntegraClaw Actions

| Action | Description |
|--------|-------------|
| `get_values` | Read values from a range |
| `update_values` | Write values to a range |
| `append_values` | Append rows to a sheet |
| `clear_values` | Clear values from a range |
| `get_spreadsheet` | Get spreadsheet metadata |
| `create_spreadsheet` | Create a new spreadsheet |

### Example: Read Values
```json
{
  "provider": "google",
  "service": "sheets",
  "action": "get_values",
  "params": {
    "spreadsheet_id": "SPREADSHEET_ID",
    "range": "Sheet1!A1:D10"
  }
}
```

## Native API Endpoints

### Get Spreadsheet Metadata
```bash
GET https://sheets.googleapis.com/v4/spreadsheets/{spreadsheetId}
```

### Get Values
```bash
GET https://sheets.googleapis.com/v4/spreadsheets/{spreadsheetId}/values/{range}
```

Example:
```bash
GET https://sheets.googleapis.com/v4/spreadsheets/SHEET_ID/values/Sheet1!A1:D10
```

### Update Values
```bash
PUT https://sheets.googleapis.com/v4/spreadsheets/{spreadsheetId}/values/{range}?valueInputOption=USER_ENTERED
Content-Type: application/json

{
  "values": [
    ["A1", "B1", "C1"],
    ["A2", "B2", "C2"]
  ]
}
```

### Append Values
```bash
POST https://sheets.googleapis.com/v4/spreadsheets/{spreadsheetId}/values/{range}:append?valueInputOption=USER_ENTERED
Content-Type: application/json

{
  "values": [
    ["New Row 1", "Data", "More Data"]
  ]
}
```

### Clear Values
```bash
POST https://sheets.googleapis.com/v4/spreadsheets/{spreadsheetId}/values/{range}:clear
```

### Create Spreadsheet
```bash
POST https://sheets.googleapis.com/v4/spreadsheets
Content-Type: application/json

{
  "properties": {"title": "New Spreadsheet"},
  "sheets": [{"properties": {"title": "Sheet1"}}]
}
```

## Range Notation

- `Sheet1!A1:D10` - Specific range
- `Sheet1!A:D` - Entire columns A through D
- `Sheet1!1:10` - Entire rows 1 through 10
- `Sheet1` - Entire sheet

## Value Input Options

- `RAW` - Values are stored as-is
- `USER_ENTERED` - Values are parsed as if typed (formulas executed)

## Resources

- [API Overview](https://developers.google.com/workspace/sheets/api/reference/rest)
- [Get Values](https://developers.google.com/workspace/sheets/api/reference/rest/v4/spreadsheets.values/get)
- [Update Values](https://developers.google.com/workspace/sheets/api/reference/rest/v4/spreadsheets.values/update)
