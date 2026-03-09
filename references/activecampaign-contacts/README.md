# ActiveCampaign Contacts Action Reference

**Provider:** `activecampaign`
**Service:** `contacts`
**App name:** `activecampaign-contacts`
**Auth:** API Key

## Actions

### list_contacts

List contacts with optional pagination.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "activecampaign",
    "service": "contacts",
    "action": "list_contacts",
    "params": {"limit": 20, "offset": 0}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | integer | no | Maximum contacts to return |
| `offset` | integer | no | Number of contacts to skip |

### get_contact

Get a contact by ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "activecampaign",
    "service": "contacts",
    "action": "get_contact",
    "params": {"contact_id": "123"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `contact_id` | string | yes | Contact ID |

### create_contact

Create a new contact.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "activecampaign",
    "service": "contacts",
    "action": "create_contact",
    "params": {
      "email": "jane@example.com",
      "firstName": "Jane",
      "lastName": "Smith",
      "phone": "+15551234567"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `email` | string | yes | Contact email |
| `firstName` | string | no | First name |
| `lastName` | string | no | Last name |
| `phone` | string | no | Phone number |

### search_contacts

Search contacts by email or query.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "activecampaign",
    "service": "contacts",
    "action": "search_contacts",
    "params": {"email": "jane@example.com"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `email` | string | no | Filter by email |
| `query` | string | no | Search query |
