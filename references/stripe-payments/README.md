# Stripe Payments Action Reference

**Provider:** `stripe`
**Service:** `payments`
**App name:** `stripe-payments`

## Actions

### list_charges

List charges with optional filters.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "stripe",
    "service": "payments",
    "action": "list_charges",
    "params": {
      "limit": 20,
      "customer": "cus_xxxxxxxxxxxxx"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | integer | no | Maximum charges to return |
| `customer` | string | no | Filter by customer ID |

### list_customers

List customers.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "stripe",
    "service": "payments",
    "action": "list_customers",
    "params": {
      "limit": 50,
      "email": "customer@example.com"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | integer | no | Maximum customers to return |
| `email` | string | no | Filter by email |

### get_customer

Get a customer by ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "stripe",
    "service": "payments",
    "action": "get_customer",
    "params": {"customer_id": "cus_xxxxxxxxxxxxx"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `customer_id` | string | yes | Customer ID |

### list_subscriptions

List subscriptions with optional filters.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "stripe",
    "service": "payments",
    "action": "list_subscriptions",
    "params": {
      "limit": 20,
      "customer": "cus_xxxxxxxxxxxxx",
      "status": "active"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | integer | no | Maximum subscriptions to return |
| `customer` | string | no | Filter by customer ID |
| `status` | string | no | Filter by status (active, past_due, canceled, etc.) |

## Resources

- [Stripe API Reference](https://stripe.com/docs/api)
