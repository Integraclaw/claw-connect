# Salesforce CRM Action Reference

**Provider:** `salesforce`
**Service:** `crm`
**App name:** `salesforce-crm`

## Actions

### query

Execute a SOQL query.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "salesforce",
    "service": "crm",
    "action": "query",
    "params": {
      "soql": "SELECT Id, Name, Email FROM Contact WHERE AccountId = '\''001000000000001'\'' LIMIT 10"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `soql` | string | yes | SOQL query string |

### get_record

Get a record by object type and ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "salesforce",
    "service": "crm",
    "action": "get_record",
    "params": {
      "object_type": "Contact",
      "record_id": "003000000000001"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `object_type` | string | yes | Object API name (e.g. Contact, Account, Lead) |
| `record_id` | string | yes | Record ID (15 or 18 characters) |

### create_record

Create a new record.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "salesforce",
    "service": "crm",
    "action": "create_record",
    "params": {
      "object_type": "Contact",
      "fields": {
        "FirstName": "Jane",
        "LastName": "Smith",
        "Email": "jane@example.com",
        "AccountId": "001000000000001"
      }
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `object_type` | string | yes | Object API name |
| `fields` | object | yes | Field name to value map |

### update_record

Update an existing record.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "salesforce",
    "service": "crm",
    "action": "update_record",
    "params": {
      "object_type": "Contact",
      "record_id": "003000000000001",
      "fields": {"Title": "VP of Sales", "Phone": "+1-555-0100"}
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `object_type` | string | yes | Object API name |
| `record_id` | string | yes | Record ID |
| `fields` | object | yes | Fields to update |

## Resources

- [Salesforce REST API Reference](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/)
- [SOQL Reference](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/)
