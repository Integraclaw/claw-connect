# QuickBooks Accounting Action Reference

**Provider:** `quickbooks`
**Service:** `accounting`
**App name:** `quickbooks-accounting`

## Actions

### query

Execute a QuickBooks SQL query.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "quickbooks",
    "service": "accounting",
    "action": "query",
    "params": {
      "query": "SELECT * FROM Customer WHERE Active = true MAXRESULTS 10"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | yes | QuickBooks SQL query |

### get_customer

Get a customer by ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "quickbooks",
    "service": "accounting",
    "action": "get_customer",
    "params": {"customer_id": "123"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `customer_id` | string | yes | Customer ID |

### get_invoice

Get an invoice by ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "quickbooks",
    "service": "accounting",
    "action": "get_invoice",
    "params": {"invoice_id": "456"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `invoice_id` | string | yes | Invoice ID |

### create_invoice

Create a new invoice.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "quickbooks",
    "service": "accounting",
    "action": "create_invoice",
    "params": {
      "customer_id": "123",
      "line_items": [
        {"description": "Consulting services", "amount": 500.00},
        {"description": "Support retainer", "amount": 200.00}
      ]
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `customer_id` | string | yes | Customer ID |
| `line_items` | array | yes | Array of line item objects with `description` and `amount` |

## Resources

- [QuickBooks API Reference](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/invoice)
- [QuickBooks Query Language](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/query-and-search)
