# Acuity Scheduling Action Reference

**Provider:** `acuity`
**Service:** `scheduling`
**App name:** `acuity-scheduling`
**Auth:** API Key

## Actions

### list_appointments

List appointments with optional date range and limit.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "acuity",
    "service": "scheduling",
    "action": "list_appointments",
    "params": {
      "max": 50,
      "minDate": "2025-03-01",
      "maxDate": "2025-03-31"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `max` | integer | no | Maximum appointments to return |
| `minDate` | string | no | Start date for filter |
| `maxDate` | string | no | End date for filter |

### list_appointment_types

List available appointment types (services).

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "acuity",
    "service": "scheduling",
    "action": "list_appointment_types",
    "params": {}
  }'
```
