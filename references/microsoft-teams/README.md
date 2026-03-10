# Microsoft Teams Action Reference

**Provider:** `microsoft`
**Service:** `teams`
**App name:** `microsoft-teams`

## Actions

### list_teams

List teams the user has joined.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "microsoft",
    "service": "teams",
    "action": "list_teams",
    "params": {}
  }'
```

No parameters required.

### list_channels

List channels in a team.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "microsoft",
    "service": "teams",
    "action": "list_channels",
    "params": {"team_id": "TEAM_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `team_id` | string | yes | Team ID |

### send_message

Send a message to a channel.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "microsoft",
    "service": "teams",
    "action": "send_message",
    "params": {
      "team_id": "TEAM_ID",
      "channel_id": "CHANNEL_ID",
      "message": "Hello from Integraclaw!"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `team_id` | string | yes | Team ID |
| `channel_id` | string | yes | Channel ID |
| `message` | string | yes | Message content |
| `content_type` | string | no | Content type: `text` or `html` (default `text`) |

### list_messages

List messages from a channel.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "microsoft",
    "service": "teams",
    "action": "list_messages",
    "params": {
      "team_id": "TEAM_ID",
      "channel_id": "CHANNEL_ID"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `team_id` | string | yes | Team ID |
| `channel_id` | string | yes | Channel ID |

## Notes

- Channel IDs include thread suffix: `19:xxx@thread.tacv2`
- Message body content types: `text` or `html`
- Uses Microsoft Graph API under the hood

## Resources

- [Microsoft Teams API Overview](https://learn.microsoft.com/en-us/graph/api/resources/teams-api-overview)
