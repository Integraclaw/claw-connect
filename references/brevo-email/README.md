# Brevo Email Action Reference

**Provider:** `brevo`
**Service:** `email`
**App name:** `brevo-email`
**Auth:** API Key

## Actions

### send_email

Send a transactional email.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "brevo",
    "service": "email",
    "action": "send_email",
    "params": {
      "to": [{"email": "recipient@example.com", "name": "Recipient Name"}],
      "subject": "Welcome to Acme",
      "htmlContent": "<p>Hello!</p>",
      "textContent": "Hello!",
      "sender": {"email": "noreply@example.com", "name": "Acme"}
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `to` | array | yes | Recipients: array of `{email, name}` objects |
| `subject` | string | yes | Email subject |
| `htmlContent` | string | no | HTML body |
| `textContent` | string | no | Plain-text body |
| `sender` | object | no | Sender `{email, name}` (uses default if omitted) |

### list_contacts

List Brevo contacts with optional pagination.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "brevo",
    "service": "email",
    "action": "list_contacts",
    "params": {"limit": 50, "offset": 0}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | integer | no | Maximum contacts to return |
| `offset` | integer | no | Number of contacts to skip |

### create_contact

Create a new contact.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "brevo",
    "service": "email",
    "action": "create_contact",
    "params": {
      "email": "jane@example.com",
      "attributes": {"FIRSTNAME": "Jane", "LASTNAME": "Smith"},
      "listIds": [2, 5]
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `email` | string | yes | Contact email |
| `attributes` | object | no | Custom attributes |
| `listIds` | array | no | List IDs to add contact to |
