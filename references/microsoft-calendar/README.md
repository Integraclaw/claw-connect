# Outlook Calendar API Reference

**App name:** `microsoft-calendar`
**Provider:** `microsoft` | **Service:** `calendar`
**Base URL:** `https://graph.microsoft.com/v1.0`

## IntegraClaw Actions

| Action | Description |
|--------|-------------|
| `list_events` | List calendar events |
| `get_event` | Get a specific event |
| `create_event` | Create a calendar event |
| `update_event` | Update an existing event |
| `delete_event` | Delete an event |
| `list_calendars` | List all calendars |

### Example: Create Event
```json
{
  "provider": "microsoft",
  "service": "calendar",
  "action": "create_event",
  "params": {
    "subject": "Team Meeting",
    "start_time": "2026-03-10T10:00:00Z",
    "end_time": "2026-03-10T11:00:00Z"
  }
}
```

## Native API Endpoints

### List Calendars
```bash
GET https://graph.microsoft.com/v1.0/me/calendars
```

### List Events
```bash
GET https://graph.microsoft.com/v1.0/me/calendar/events?$top=10
```

With filter:
```bash
GET https://graph.microsoft.com/v1.0/me/calendar/events?$filter=start/dateTime ge '2026-01-01'&$top=10
```

### Create Event
```bash
POST https://graph.microsoft.com/v1.0/me/calendar/events
Content-Type: application/json

{
  "subject": "Meeting",
  "start": {"dateTime": "2026-03-10T10:00:00", "timeZone": "UTC"},
  "end": {"dateTime": "2026-03-10T11:00:00", "timeZone": "UTC"},
  "attendees": [{"emailAddress": {"address": "attendee@example.com"}, "type": "required"}]
}
```

### Update Event
```bash
PATCH https://graph.microsoft.com/v1.0/me/events/{eventId}
Content-Type: application/json

{"subject": "Updated Meeting Title"}
```

### Delete Event
```bash
DELETE https://graph.microsoft.com/v1.0/me/events/{eventId}
```

## Notes

- Uses Microsoft Graph API
- Calendar events use ISO 8601 datetime format
- Supports OData query parameters for filtering

## Resources

- [Calendar API](https://learn.microsoft.com/en-us/graph/api/resources/calendar)
- [Microsoft Graph API](https://learn.microsoft.com/en-us/graph/api/overview)
