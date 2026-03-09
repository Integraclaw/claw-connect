# ContaAzul Action Reference

**Provider:** `contaazul`
**Services:** `customers`, `products`, `sales`

ContaAzul actions are split across three services:

| Service | App Name | Actions |
|---------|----------|---------|
| Customers | `contaazul-customers` | list, get, create, update |
| Products | `contaazul-products` | list, get, create, update |
| Sales | `contaazul-sales` | list, get, create |

---

## Customers

### list

List customers.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "contaazul",
    "service": "customers",
    "action": "list",
    "params": {}
  }'
```

No parameters required.

### get

Get a customer by ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "contaazul",
    "service": "customers",
    "action": "get",
    "params": {"customer_id": "CUSTOMER_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `customer_id` | string | yes | Customer ID |

### create

Create a new customer.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "contaazul",
    "service": "customers",
    "action": "create",
    "params": {
      "name": "Acme Inc",
      "email": "contato@acme.com"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | yes | Customer name |
| `email` | string | no | Customer email |

### update

Update a customer.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "contaazul",
    "service": "customers",
    "action": "update",
    "params": {
      "customer_id": "CUSTOMER_ID",
      "name": "Acme Corp"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `customer_id` | string | yes | Customer ID |
| `name` | string | no | New customer name |

---

## Products

### list

List products.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "contaazul",
    "service": "products",
    "action": "list",
    "params": {}
  }'
```

No parameters required.

### get

Get a product by ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "contaazul",
    "service": "products",
    "action": "get",
    "params": {"product_id": "PRODUCT_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `product_id` | string | yes | Product ID |

### create

Create a new product.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "contaazul",
    "service": "products",
    "action": "create",
    "params": {
      "name": "Premium Plan",
      "value": 99.90
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | yes | Product name |
| `value` | number | yes | Product value |

### update

Update a product.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "contaazul",
    "service": "products",
    "action": "update",
    "params": {
      "product_id": "PRODUCT_ID",
      "name": "Premium Plan Plus",
      "value": 129.90
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `product_id` | string | yes | Product ID |
| `name` | string | no | New product name |
| `value` | number | no | New product value |

---

## Sales

### list

List sales.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "contaazul",
    "service": "sales",
    "action": "list",
    "params": {}
  }'
```

No parameters required.

### get

Get a sale by ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "contaazul",
    "service": "sales",
    "action": "get",
    "params": {"sale_id": "SALE_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `sale_id` | string | yes | Sale ID |

### create

Create a new sale.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "contaazul",
    "service": "sales",
    "action": "create",
    "params": {
      "customer_id": "CUSTOMER_ID",
      "products": [
        {"product_id": "PRODUCT_ID", "quantity": 2}
      ]
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `customer_id` | string | yes | Customer ID |
| `products` | array | yes | Array of product objects with product_id and quantity |

---

## Resources

- [ContaAzul API](https://api.contaazul.com/)
