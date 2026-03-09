# HubSpot CRM Action Reference

**Provider:** `hubspot`
**Service:** `crm`
**App name:** `hubspot-crm`

## Actions

### list_contacts

List contacts with optional pagination.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "hubspot",
    "service": "crm",
    "action": "list_contacts",
    "params": {"limit": 20, "after": null}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | integer | no | Maximum contacts to return |
| `after` | string | no | Pagination cursor (from previous response) |

### get_contact

Get a contact by ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "hubspot",
    "service": "crm",
    "action": "get_contact",
    "params": {"contact_id": "12345"}
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
    "provider": "hubspot",
    "service": "crm",
    "action": "create_contact",
    "params": {
      "email": "jane@example.com",
      "firstname": "Jane",
      "lastname": "Smith",
      "properties": {"company": "Acme Inc"}
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `email` | string | yes | Contact email |
| `firstname` | string | no | First name |
| `lastname` | string | no | Last name |
| `properties` | object | no | Additional HubSpot properties |

### search_contacts

Search contacts by query.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "hubspot",
    "service": "crm",
    "action": "search_contacts",
    "params": {"query": "Acme"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | yes | Search query |

### list_deals

List deals.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "hubspot",
    "service": "crm",
    "action": "list_deals",
    "params": {"limit": 20}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | integer | no | Maximum deals to return |

### create_deal

Create a new deal.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "hubspot",
    "service": "crm",
    "action": "create_deal",
    "params": {
      "dealname": "Acme Corp - Enterprise",
      "pipeline": "default",
      "dealstage": "qualifiedtobuy",
      "amount": 50000
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `dealname` | string | yes | Deal name |
| `pipeline` | string | no | Pipeline ID |
| `dealstage` | string | no | Stage ID |
| `amount` | number | no | Deal amount |

## Resources

- [HubSpot CRM API](https://developers.hubspot.com/docs/api/crm/contacts)
