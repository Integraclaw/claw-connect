# Microsoft Teams API Reference

**App name:** `microsoft-teams`
**Provider:** `microsoft` | **Service:** `teams`
**Base URL:** `https://graph.microsoft.com/v1.0`

## IntegraClaw Actions

| Action | Description |
|--------|-------------|
| `list_teams` | List joined teams |
| `list_channels` | List channels in a team |
| `send_channel_message` | Send a message to a channel |
| `list_channel_messages` | List messages in a channel |

### Example: Send Channel Message
```json
{
  "provider": "microsoft",
  "service": "teams",
  "action": "send_channel_message",
  "params": {
    "team_id": "TEAM_ID",
    "channel_id": "CHANNEL_ID",
    "content": "Hello from IntegraClaw!"
  }
}
```

## Native API Endpoints

### List Joined Teams
```bash
GET https://graph.microsoft.com/v1.0/me/joinedTeams
```

### Get Team
```bash
GET https://graph.microsoft.com/v1.0/teams/{team-id}
```

### List Channels
```bash
GET https://graph.microsoft.com/v1.0/teams/{team-id}/channels
```

### List Channel Messages
```bash
GET https://graph.microsoft.com/v1.0/teams/{team-id}/channels/{channel-id}/messages
```

### Send Channel Message
```bash
POST https://graph.microsoft.com/v1.0/teams/{team-id}/channels/{channel-id}/messages
Content-Type: application/json

{"body": {"content": "Hello World"}}
```

### Reply to Message
```bash
POST https://graph.microsoft.com/v1.0/teams/{team-id}/channels/{channel-id}/messages/{message-id}/replies
Content-Type: application/json

{"body": {"content": "Reply content"}}
```

### List Chats
```bash
GET https://graph.microsoft.com/v1.0/me/chats
```

### Send Chat Message
```bash
POST https://graph.microsoft.com/v1.0/chats/{chat-id}/messages
Content-Type: application/json

{"body": {"content": "Hello"}}
```

### Get User Presence
```bash
GET https://graph.microsoft.com/v1.0/me/presence
```

## Notes

- Uses Microsoft Graph API
- Channel IDs include thread suffix: `19:xxx@thread.tacv2`
- Message body content types: `text` or `html`
- Supports OData query parameters

## Resources

- [Teams API Overview](https://learn.microsoft.com/en-us/graph/api/resources/teams-api-overview)
- [Microsoft Graph API](https://learn.microsoft.com/en-us/graph/api/overview)
