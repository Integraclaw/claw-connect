# Cal.com Scheduling Action Reference

**Provider:** `calcom`
**Service:** `scheduling`
**App name:** `calcom-scheduling`
**Auth:** API Key

## Actions

### list_event_types

List all event types (meeting types) in the account.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "calcom",
    "service": "scheduling",
    "action": "list_event_types",
    "params": {}
  }'
```

### list_bookings

List bookings with optional status and pagination filters.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "calcom",
    "service": "scheduling",
    "action": "list_bookings",
    "params": {"status": "ACCEPTED", "page": 1}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `status` | string | no | Filter by status (e.g. PENDING, ACCEPTED, CANCELLED) |
| `page` | integer | no | Page number for pagination |
