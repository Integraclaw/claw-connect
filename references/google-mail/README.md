# Gmail Action Reference

**Provider:** `google`
**Service:** `gmail`
**App name:** `google-gmail`

## Actions

### send_email

Send an email.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "gmail",
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
| `body` | string | yes | Email body (plain text) |
| `cc` | string | no | CC recipients (comma-separated) |
| `bcc` | string | no | BCC recipients (comma-separated) |

### list_messages

List messages from the mailbox.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "gmail",
    "action": "list_messages",
    "params": {
      "max_results": 10,
      "label_ids": ["INBOX", "UNREAD"]
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `max_results` | integer | no | Maximum number of messages (default 10) |
| `page_token` | string | no | Token for the next page of results |
| `label_ids` | array | no | Filter by label IDs (e.g. `["INBOX", "UNREAD"]`) |

### search_messages

Search messages using Gmail query syntax.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "gmail",
    "action": "search_messages",
    "params": {
      "query": "from:boss@company.com newer_than:7d",
      "max_results": 20
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | yes | Gmail search query (e.g. `"from:boss@company.com newer_than:7d"`) |
| `max_results` | integer | no | Maximum results (default 20) |
| `page_token` | string | no | Pagination token |

### read_message

Read a specific message by ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "gmail",
    "action": "read_message",
    "params": {
      "message_id": "18e1234abc"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `message_id` | string | yes | The message ID |
| `format` | string | no | Response format: `full`, `metadata`, `minimal`, `raw` (default `full`) |

### mark_as_read

Mark a message as read.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "gmail",
    "action": "mark_as_read",
    "params": {"message_id": "18e1234abc"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `message_id` | string | yes | The message ID |

### archive

Archive a message (remove from Inbox).

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "gmail",
    "action": "archive",
    "params": {"message_id": "18e1234abc"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `message_id` | string | yes | The message ID |

### list_labels

List all labels in the mailbox.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "gmail",
    "action": "list_labels",
    "params": {}
  }'
```

No parameters required.

### create_draft

Create an email draft.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "gmail",
    "action": "create_draft",
    "params": {
      "to": "recipient@example.com",
      "subject": "Draft email",
      "body": "This is a draft."
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `to` | string | yes | Recipient email address |
| `subject` | string | yes | Email subject |
| `body` | string | yes | Email body (plain text) |
| `cc` | string | no | CC recipients (comma-separated) |
| `bcc` | string | no | BCC recipients (comma-separated) |

## Query Operators (for search_messages)

- `is:unread` — Unread messages
- `is:starred` — Starred messages
- `from:email@example.com` — From specific sender
- `to:email@example.com` — To specific recipient
- `subject:keyword` — Subject contains keyword
- `after:2024/01/01` — After date
- `before:2024/12/31` — Before date
- `has:attachment` — Has attachments
- `newer_than:7d` — Newer than 7 days

## Resources

- [Gmail API Overview](https://developers.google.com/gmail/api/reference/rest)
- [Search Operators](https://support.google.com/mail/answer/7190)
