# Outlook Action Reference

**Provider:** `microsoft`
**Service:** `outlook`
**App name:** `microsoft-outlook`

## Actions

### send_email

Send an email via Outlook.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "microsoft",
    "service": "outlook",
    "action": "send_email",
    "params": {
      "to": "recipient@example.com",
      "subject": "Hello",
      "body": "Email body text"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `to` | string | yes | Recipient email address |
| `subject` | string | yes | Email subject |
| `body` | string | yes | Email body |
| `cc` | string | no | CC recipients (comma-separated) |
| `body_type` | string | no | Body content type: `text` or `html` (default `text`) |

### list_messages

List messages from a mail folder.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "microsoft",
    "service": "outlook",
    "action": "list_messages",
    "params": {"folder": "inbox", "top": 10}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `folder` | string | no | Mail folder (default: `inbox`) |
| `top` | integer | no | Number of messages (default 10) |
| `skip` | integer | no | Number of messages to skip (pagination) |

### search_messages

Search messages.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "microsoft",
    "service": "outlook",
    "action": "search_messages",
    "params": {"query": "from:boss subject:project", "top": 10}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | yes | Search query |
| `top` | integer | no | Max results (default 10) |

### read_message

Read a specific message.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "microsoft",
    "service": "outlook",
    "action": "read_message",
    "params": {"message_id": "MESSAGE_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `message_id` | string | yes | The message ID |

### list_folders

List all mail folders.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "microsoft",
    "service": "outlook",
    "action": "list_folders",
    "params": {}
  }'
```

No parameters required.

## Well-known Folder Names

`Inbox`, `Drafts`, `SentItems`, `DeletedItems`, `Archive`, `JunkEmail`

## Resources

- [Microsoft Graph Mail API](https://learn.microsoft.com/en-us/graph/api/resources/mail-api-overview)
