# Gmail API Reference

**App name:** `google-gmail`
**Provider:** `google` | **Service:** `gmail`
**Base URL:** `https://gmail.googleapis.com`

## IntegraClaw Actions

| Action | Description |
|--------|-------------|
| `send_email` | Send an email |
| `list_messages` | List messages in inbox |
| `search_messages` | Search messages with query |
| `read_message` | Read a specific message |
| `mark_as_read` | Mark message as read |
| `archive` | Archive a message |
| `list_labels` | List all labels |
| `create_draft` | Create an email draft |

### Example: Send Email
```json
{
  "provider": "google",
  "service": "gmail",
  "action": "send_email",
  "params": {
    "to": "recipient@example.com",
    "subject": "Hello!",
    "body": "Message content"
  }
}
```

## Native API Endpoints

Use with `POST /api/v1/token/get` for direct API access.

### List Messages
```bash
GET https://gmail.googleapis.com/gmail/v1/users/me/messages?maxResults=10
```

With query filter:
```bash
GET https://gmail.googleapis.com/gmail/v1/users/me/messages?q=is:unread&maxResults=10
```

### Get Message
```bash
GET https://gmail.googleapis.com/gmail/v1/users/me/messages/{messageId}
```

With metadata only:
```bash
GET https://gmail.googleapis.com/gmail/v1/users/me/messages/{messageId}?format=metadata&metadataHeaders=From&metadataHeaders=Subject&metadataHeaders=Date
```

### Send Message
```bash
POST https://gmail.googleapis.com/gmail/v1/users/me/messages/send
Content-Type: application/json

{
  "raw": "BASE64_ENCODED_EMAIL"
}
```

### List Labels
```bash
GET https://gmail.googleapis.com/gmail/v1/users/me/labels
```

### List Threads
```bash
GET https://gmail.googleapis.com/gmail/v1/users/me/threads?maxResults=10
```

### Get Thread
```bash
GET https://gmail.googleapis.com/gmail/v1/users/me/threads/{threadId}
```

### Modify Message Labels
```bash
POST https://gmail.googleapis.com/gmail/v1/users/me/messages/{messageId}/modify
Content-Type: application/json

{
  "addLabelIds": ["STARRED"],
  "removeLabelIds": ["UNREAD"]
}
```

### Create Draft
```bash
POST https://gmail.googleapis.com/gmail/v1/users/me/drafts
Content-Type: application/json

{
  "message": {
    "raw": "BASE64URL_ENCODED_EMAIL"
  }
}
```

### Get Profile
```bash
GET https://gmail.googleapis.com/gmail/v1/users/me/profile
```

## Query Operators

Use in the `q` parameter:
- `is:unread` - Unread messages
- `is:starred` - Starred messages
- `from:email@example.com` - From specific sender
- `to:email@example.com` - To specific recipient
- `subject:keyword` - Subject contains keyword
- `after:2024/01/01` - After date
- `before:2024/12/31` - Before date
- `has:attachment` - Has attachments

## Notes

- Use `me` as userId for the authenticated user
- Message body is base64url encoded in the `raw` field
- IntegraClaw actions handle encoding/decoding automatically

## Resources

- [API Overview](https://developers.google.com/gmail/api/reference/rest)
- [List Messages](https://developers.google.com/gmail/api/reference/rest/v1/users.messages/list)
- [Send Message](https://developers.google.com/gmail/api/reference/rest/v1/users.messages/send)
