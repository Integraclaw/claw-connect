# CallRail Calls Action Reference

**Provider:** `callrail`
**Service:** `calls`
**App name:** `callrail-calls`
**Auth:** API Key

## Actions

### list_calls

List calls for a company with optional date range and pagination.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "callrail",
    "service": "calls",
    "action": "list_calls",
    "params": {
      "company_id": "abc123xyz",
      "start_date": "2025-03-01",
      "end_date": "2025-03-08",
      "per_page": 25
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `company_id` | string | yes | CallRail company ID |
| `start_date` | string | no | Start date (YYYY-MM-DD) |
| `end_date` | string | no | End date (YYYY-MM-DD) |
| `per_page` | integer | no | Results per page |
