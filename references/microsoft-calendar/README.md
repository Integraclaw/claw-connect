# Microsoft Calendar Action Reference

**Provider:** `microsoft`
**Service:** `calendar`
**App name:** `microsoft-calendar`

## Actions

### list_events

List calendar events.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "microsoft",
    "service": "calendar",
    "action": "list_events",
    "params": {
      "start_time": "2026-03-01T00:00:00",
      "end_time": "2026-03-31T23:59:59",
      "top": 20
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `start_time` | string | no | Start of time range (ISO 8601) |
| `end_time` | string | no | End of time range (ISO 8601) |
| `top` | integer | no | Maximum events (default 10) |

### get_event

Get a specific event.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "microsoft",
    "service": "calendar",
    "action": "get_event",
    "params": {"event_id": "EVENT_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `event_id` | string | yes | Event ID |

### create_event

Create a new calendar event.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "microsoft",
    "service": "calendar",
    "action": "create_event",
    "params": {
      "subject": "Team Meeting",
      "start": "2026-03-10T10:00:00",
      "end": "2026-03-10T11:00:00",
      "timezone": "America/Sao_Paulo",
      "attendees": ["colleague@example.com"]
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `subject` | string | yes | Event subject/title |
| `start` | string | yes | Start time (ISO 8601) |
| `end` | string | yes | End time (ISO 8601) |
| `timezone` | string | no | Timezone (default UTC) |
| `body` | string | no | Event body/description |
| `location` | string | no | Event location |
| `attendees` | array | no | List of attendee email addresses |
| `is_all_day` | boolean | no | Whether this is an all-day event |

### delete_event

Delete an event.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "microsoft",
    "service": "calendar",
    "action": "delete_event",
    "params": {"event_id": "EVENT_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `event_id` | string | yes | Event ID to delete |

## Notes

- Times use ISO 8601 format (e.g. `2026-03-10T10:00:00`)
- Uses Microsoft Graph API under the hood

## Resources

- [Microsoft Graph Calendar API](https://learn.microsoft.com/en-us/graph/api/resources/calendar)
