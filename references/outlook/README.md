# Outlook Mail API Reference

**App name:** `microsoft-outlook`
**Provider:** `microsoft` | **Service:** `outlook`
**Base URL:** `https://graph.microsoft.com/v1.0`

## IntegraClaw Actions

| Action | Description |
|--------|-------------|
| `send_email` | Send an email |
| `list_messages` | List messages in inbox |
| `read_message` | Read a specific message |
| `search_messages` | Search messages |
| `create_draft` | Create an email draft |

### Example: Send Email
```json
{
  "provider": "microsoft",
  "service": "outlook",
  "action": "send_email",
  "params": {
    "to": "recipient@example.com",
    "subject": "Hello!",
    "body": "Message content"
  }
}
```

## Native API Endpoints

### List Messages
```bash
GET https://graph.microsoft.com/v1.0/me/messages
```

With filter:
```bash
GET https://graph.microsoft.com/v1.0/me/messages?$filter=isRead eq false&$top=10
```

### Get Message
```bash
GET https://graph.microsoft.com/v1.0/me/messages/{messageId}
```

### Send Message
```bash
POST https://graph.microsoft.com/v1.0/me/sendMail
Content-Type: application/json

{
  "message": {
    "subject": "Hello",
    "body": {"contentType": "Text", "content": "Email body."},
    "toRecipients": [{"emailAddress": {"address": "recipient@example.com"}}]
  },
  "saveToSentItems": true
}
```

### Create Draft
```bash
POST https://graph.microsoft.com/v1.0/me/messages
Content-Type: application/json

{
  "subject": "Hello",
  "body": {"contentType": "Text", "content": "Email body."},
  "toRecipients": [{"emailAddress": {"address": "recipient@example.com"}}]
}
```

### Update Message (Mark as Read)
```bash
PATCH https://graph.microsoft.com/v1.0/me/messages/{messageId}
Content-Type: application/json

{"isRead": true}
```

### Delete Message
```bash
DELETE https://graph.microsoft.com/v1.0/me/messages/{messageId}
```

### List Mail Folders
```bash
GET https://graph.microsoft.com/v1.0/me/mailFolders
```

## OData Query Parameters

- `$top=10` - Limit results
- `$skip=20` - Skip results (pagination)
- `$select=subject,from` - Select specific fields
- `$filter=isRead eq false` - Filter results
- `$orderby=receivedDateTime desc` - Sort results
- `$search="keyword"` - Search content

## Notes

- Use `me` as the user identifier for the authenticated user
- Message body content types: `Text` or `HTML`
- Well-known folder names work as folder IDs: `Inbox`, `Drafts`, `SentItems`

## Resources

- [Mail API](https://learn.microsoft.com/en-us/graph/api/resources/mail-api-overview)
- [Microsoft Graph API](https://learn.microsoft.com/en-us/graph/api/overview)
