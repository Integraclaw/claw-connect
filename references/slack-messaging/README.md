# Slack Messaging Action Reference

**Provider:** `slack`
**Service:** `messaging`
**App name:** `slack-messaging`

## Actions

### send_message

Send a message to a Slack channel.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "slack",
    "service": "messaging",
    "action": "send_message",
    "params": {
      "channel": "C01234567",
      "text": "Hello from IntegraClaw!"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `channel` | string | yes | Channel ID |
| `text` | string | yes | Message text |

### list_channels

List accessible Slack channels.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "slack",
    "service": "messaging",
    "action": "list_channels",
    "params": {"limit": 50}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | integer | no | Maximum channels to return |

### list_messages

List messages in a channel.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "slack",
    "service": "messaging",
    "action": "list_messages",
    "params": {
      "channel": "C01234567",
      "limit": 20
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `channel` | string | yes | Channel ID |
| `limit` | integer | no | Maximum messages to return |

### search_messages

Search messages across Slack.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "slack",
    "service": "messaging",
    "action": "search_messages",
    "params": {"query": "project update"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | yes | Search query text |

## Resources

- [Slack API Reference](https://api.slack.com/reference)
