# Calendly Scheduling Action Reference

**Provider:** `calendly`
**Service:** `scheduling`
**App name:** `calendly-scheduling`

## Actions

### get_current_user

Get the current authenticated user.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "calendly",
    "service": "scheduling",
    "action": "get_current_user",
    "params": {}
  }'
```

No parameters required.

### list_event_types

List event types (meeting types) for a user.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "calendly",
    "service": "scheduling",
    "action": "list_event_types",
    "params": {"user": "https://api.calendly.com/users/USER_UUID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user` | string | no | User URI (defaults to current user) |

### list_scheduled_events

List scheduled events with optional time filter.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "calendly",
    "service": "scheduling",
    "action": "list_scheduled_events",
    "params": {
      "user": "https://api.calendly.com/users/USER_UUID",
      "min_start_time": "2026-03-01T00:00:00Z",
      "max_start_time": "2026-03-31T23:59:59Z"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user` | string | no | User URI |
| `min_start_time` | string | no | Start of time range (ISO8601) |
| `max_start_time` | string | no | End of time range (ISO8601) |

### get_scheduled_event

Get a scheduled event by UUID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "calendly",
    "service": "scheduling",
    "action": "get_scheduled_event",
    "params": {"event_uuid": "EVENT_UUID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `event_uuid` | string | yes | Event UUID |

### list_invitees

List invitees for a scheduled event.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "calendly",
    "service": "scheduling",
    "action": "list_invitees",
    "params": {"event_uuid": "EVENT_UUID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `event_uuid` | string | yes | Event UUID |

## Resources

- [Calendly API Reference](https://developer.calendly.com/api-docs)
