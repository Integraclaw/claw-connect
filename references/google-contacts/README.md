# Google Contacts Action Reference

**Provider:** `google`
**Service:** `contacts`
**App name:** `google-contacts`

## Actions

### list_contacts

List contacts for the authenticated user.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "contacts",
    "action": "list_contacts",
    "params": {
      "page_size": 50
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `page_size` | integer | no | Number of contacts to return (default 100, max 1000) |
| `page_token` | string | no | Pagination token |
| `sort_order` | string | no | Sort: `LAST_MODIFIED_ASCENDING`, `LAST_MODIFIED_DESCENDING`, `FIRST_NAME_ASCENDING`, `LAST_NAME_ASCENDING` |

### search_contacts

Search contacts by name, email, phone, or organization.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "contacts",
    "action": "search_contacts",
    "params": {
      "query": "John"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | **yes** | Search query (prefix matching on names, emails, phones, organizations) |
| `page_size` | integer | no | Max results (default 10, max 30) |

### get_contact

Get full details of a specific contact.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "contacts",
    "action": "get_contact",
    "params": {
      "resource_name": "people/c12345678"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `resource_name` | string | **yes** | Contact resource name (e.g. `people/c12345678`) |

### create_contact

Create a new contact.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "contacts",
    "action": "create_contact",
    "params": {
      "given_name": "John",
      "family_name": "Doe",
      "email": "john@example.com",
      "phone": "+1234567890",
      "organization": "Acme Inc",
      "title": "Software Engineer"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `given_name` | string | **yes** | First name |
| `family_name` | string | no | Last name |
| `email` | string | no | Email address |
| `phone` | string | no | Phone number |
| `organization` | string | no | Company/organization name |
| `title` | string | no | Job title |

### delete_contact

Permanently delete a contact.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "contacts",
    "action": "delete_contact",
    "params": {
      "resource_name": "people/c12345678"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `resource_name` | string | **yes** | Contact resource name (e.g. `people/c12345678`) |
