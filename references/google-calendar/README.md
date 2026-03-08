# Google Calendar API Reference

**App name:** `google-calendar`
**Provider:** `google` | **Service:** `calendar`
**Base URL:** `https://www.googleapis.com/calendar/v3`

## IntegraClaw Actions

| Action | Description |
|--------|-------------|
| `list_events` | List calendar events |
| `get_event` | Get a specific event |
| `create_event` | Create a calendar event |
| `update_event` | Update an existing event |
| `delete_event` | Delete an event |
| `list_calendars` | List all calendars |
| `quick_add` | Create event from natural language |
| `check_availability` | Check free/busy status |

### Example: Create Event
```json
{
  "provider": "google",
  "service": "calendar",
  "action": "create_event",
  "params": {
    "summary": "Team Meeting",
    "start_time": "2026-03-10T10:00:00Z",
    "end_time": "2026-03-10T11:00:00Z",
    "description": "Weekly sync"
  }
}
```

## Native API Endpoints

### List Calendars
```bash
GET https://www.googleapis.com/calendar/v3/users/me/calendarList
```

### List Events
```bash
GET https://www.googleapis.com/calendar/v3/calendars/primary/events?maxResults=10&orderBy=startTime&singleEvents=true
```

With time bounds:
```bash
GET https://www.googleapis.com/calendar/v3/calendars/primary/events?timeMin=2026-01-01T00:00:00Z&timeMax=2026-12-31T23:59:59Z&singleEvents=true&orderBy=startTime
```

### Get Event
```bash
GET https://www.googleapis.com/calendar/v3/calendars/primary/events/{eventId}
```

### Insert Event
```bash
POST https://www.googleapis.com/calendar/v3/calendars/primary/events
Content-Type: application/json

{
  "summary": "Team Meeting",
  "start": {"dateTime": "2026-03-10T10:00:00", "timeZone": "America/Sao_Paulo"},
  "end": {"dateTime": "2026-03-10T11:00:00", "timeZone": "America/Sao_Paulo"},
  "attendees": [{"email": "attendee@example.com"}]
}
```

### Update Event
```bash
PUT https://www.googleapis.com/calendar/v3/calendars/primary/events/{eventId}
Content-Type: application/json

{
  "summary": "Updated Meeting Title",
  "start": {"dateTime": "2026-03-10T10:00:00Z"},
  "end": {"dateTime": "2026-03-10T11:00:00Z"}
}
```

### Delete Event
```bash
DELETE https://www.googleapis.com/calendar/v3/calendars/primary/events/{eventId}
```

### Quick Add (natural language)
```bash
POST https://www.googleapis.com/calendar/v3/calendars/primary/events/quickAdd?text=Meeting+with+John+tomorrow+at+3pm
```

### Free/Busy Query
```bash
POST https://www.googleapis.com/calendar/v3/freeBusy
Content-Type: application/json

{
  "timeMin": "2026-03-10T00:00:00Z",
  "timeMax": "2026-03-11T00:00:00Z",
  "items": [{"id": "primary"}]
}
```

## Notes

- Use `primary` as calendarId for the user's main calendar
- Times must be in RFC3339 format
- For recurring events, use `singleEvents=true` to expand instances
- `orderBy=startTime` requires `singleEvents=true`

## Resources

- [API Overview](https://developers.google.com/calendar/api/v3/reference)
- [List Events](https://developers.google.com/workspace/calendar/api/v3/reference/events/list)
- [Insert Event](https://developers.google.com/workspace/calendar/api/v3/reference/events/insert)
