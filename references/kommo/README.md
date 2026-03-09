# Kommo (AmoCRM) Action Reference

**Provider:** `kommo`
**Services:** `leads`, `contacts`, `pipelines`

Kommo actions are split across three services:

| Service | App Name | Actions |
|---------|----------|---------|
| Leads | `kommo-leads` | list, get, create, update |
| Contacts | `kommo-contacts` | list, get, create, update |
| Pipelines | `kommo-pipelines` | list, get, statuses |

---

## Leads

### list

List leads.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "kommo",
    "service": "leads",
    "action": "list",
    "params": {}
  }'
```

No parameters required.

### get

Get a lead by ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "kommo",
    "service": "leads",
    "action": "get",
    "params": {"lead_id": "LEAD_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `lead_id` | string | yes | Lead ID |

### create

Create a new lead.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "kommo",
    "service": "leads",
    "action": "create",
    "params": {
      "name": "Acme Corp - Enterprise Deal",
      "pipeline_id": 123456
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | yes | Lead name |
| `pipeline_id` | integer | no | Pipeline ID |

### update

Update a lead.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "kommo",
    "service": "leads",
    "action": "update",
    "params": {
      "lead_id": "LEAD_ID",
      "name": "Updated lead name",
      "status_id": 142
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `lead_id` | string | yes | Lead ID |
| `name` | string | no | New lead name |
| `status_id` | integer | no | Status ID in pipeline |

---

## Contacts

### list

List contacts.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "kommo",
    "service": "contacts",
    "action": "list",
    "params": {}
  }'
```

No parameters required.

### get

Get a contact by ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "kommo",
    "service": "contacts",
    "action": "get",
    "params": {"contact_id": "CONTACT_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `contact_id` | string | yes | Contact ID |

### create

Create a new contact.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "kommo",
    "service": "contacts",
    "action": "create",
    "params": {"name": "Jane Smith"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | yes | Contact name |

### update

Update a contact.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "kommo",
    "service": "contacts",
    "action": "update",
    "params": {
      "contact_id": "CONTACT_ID",
      "name": "Jane Doe"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `contact_id` | string | yes | Contact ID |
| `name` | string | no | New contact name |

---

## Pipelines

### list

List pipelines.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "kommo",
    "service": "pipelines",
    "action": "list",
    "params": {}
  }'
```

No parameters required.

### get

Get a pipeline by ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "kommo",
    "service": "pipelines",
    "action": "get",
    "params": {"pipeline_id": "PIPELINE_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `pipeline_id` | string | yes | Pipeline ID |

### statuses

Get pipeline statuses (stages).

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "kommo",
    "service": "pipelines",
    "action": "statuses",
    "params": {"pipeline_id": "PIPELINE_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `pipeline_id` | string | yes | Pipeline ID |

---

## Resources

- [Kommo (AmoCRM) API Reference](https://www.amocrm.com/developers/content/crm_platform/api-reference)
