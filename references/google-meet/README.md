# Google Meet Action Reference

**Provider:** `google`
**Service:** `meet`
**App name:** `google-meet`

## Actions

### create_space

Create a new Google Meet meeting space. Returns the meeting URI (join link) and meeting code.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "meet",
    "action": "create_space",
    "params": {}
  }'
```

No parameters required.

### get_space

Get details of a Google Meet space.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "meet",
    "action": "get_space",
    "params": {
      "space_name": "spaces/abc123"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `space_name` | string | **yes** | Space resource name (e.g. `spaces/abc123`) or meeting code (e.g. `abc-mnop-xyz`) |

### end_conference

End the active conference in a Meet space. All participants will be removed.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "meet",
    "action": "end_conference",
    "params": {
      "space_name": "spaces/abc123"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `space_name` | string | **yes** | Space resource name (must be server-generated ID, not meeting code) |

### list_conference_records

List conference records (past and ongoing meetings).

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "meet",
    "action": "list_conference_records",
    "params": {
      "page_size": 25
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `page_size` | integer | no | Max results per page (default 25, max 100) |
| `page_token` | string | no | Pagination token |
| `filter` | string | no | Filter expression (e.g. `space.meeting_code=abc-mnop-xyz`) |
