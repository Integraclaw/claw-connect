# Google Calendar Action Reference

**Provider:** `google`
**Service:** `calendar`
**App name:** `google-calendar`

## Actions

### list_calendars

List all calendars.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "calendar",
    "action": "list_calendars",
    "params": {}
  }'
```

No parameters required.

### list_events

List events from a calendar.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "calendar",
    "action": "list_events",
    "params": {
      "time_min": "2026-03-01T00:00:00Z",
      "time_max": "2026-03-31T23:59:59Z",
      "max_results": 20
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `calendar_id` | string | no | Calendar ID (default: `primary`) |
| `time_min` | string | no | Start of time range (RFC3339) |
| `time_max` | string | no | End of time range (RFC3339) |
| `max_results` | integer | no | Maximum events (default 10) |
| `page_token` | string | no | Pagination token |

### search_events

Search events by text.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "calendar",
    "action": "search_events",
    "params": {
      "query": "team meeting"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | yes | Free-text search query |
| `calendar_id` | string | no | Calendar ID (default: `primary`) |
| `time_min` | string | no | Start of time range (RFC3339) |
| `max_results` | integer | no | Maximum events (default 10) |

### get_event

Get a specific event.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "calendar",
    "action": "get_event",
    "params": {"event_id": "EVENT_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `event_id` | string | yes | Event ID |
| `calendar_id` | string | no | Calendar ID (default: `primary`) |

### create_event

Create a new event.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "calendar",
    "action": "create_event",
    "params": {
      "summary": "Team Meeting",
      "start": "2026-03-10T10:00:00",
      "end": "2026-03-10T11:00:00",
      "timezone": "America/Sao_Paulo",
      "attendees": ["colleague@example.com"]
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `summary` | string | yes | Event title |
| `start` | string | yes | Start time (RFC3339) or date (YYYY-MM-DD for all-day) |
| `end` | string | yes | End time (RFC3339) or date (YYYY-MM-DD for all-day) |
| `description` | string | no | Event description |
| `location` | string | no | Event location |
| `timezone` | string | no | Timezone (e.g. `America/Sao_Paulo`). Required for timed events. |
| `attendees` | array | no | List of attendee email addresses |
| `calendar_id` | string | no | Calendar ID (default: `primary`) |

### update_event

Update an existing event.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "calendar",
    "action": "update_event",
    "params": {
      "event_id": "EVENT_ID",
      "summary": "Updated Meeting Title",
      "location": "Room 42"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `event_id` | string | yes | Event ID to update |
| `calendar_id` | string | no | Calendar ID (default: `primary`) |
| `summary` | string | no | New event title |
| `start` | string | no | New start time (RFC3339) or date |
| `end` | string | no | New end time (RFC3339) or date |
| `description` | string | no | New description |
| `location` | string | no | New location |
| `timezone` | string | no | Timezone for timed events |

### delete_event

Delete an event.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "calendar",
    "action": "delete_event",
    "params": {"event_id": "EVENT_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `event_id` | string | yes | Event ID to delete |
| `calendar_id` | string | no | Calendar ID (default: `primary`) |

### quick_add

Create an event from natural language.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "calendar",
    "action": "quick_add",
    "params": {"text": "Lunch with John tomorrow at noon"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `text` | string | yes | Natural language event description |
| `calendar_id` | string | no | Calendar ID (default: `primary`) |

## Notes

- Use `primary` as `calendar_id` for the user's main calendar
- Times must be in RFC3339 format (e.g. `2026-03-10T10:00:00Z`)
- For all-day events, use `YYYY-MM-DD` format for `start` and `end`

## Resources

- [Google Calendar API Reference](https://developers.google.com/calendar/api/v3/reference)
