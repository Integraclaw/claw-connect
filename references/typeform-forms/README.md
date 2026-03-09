# Typeform Forms Action Reference

**Provider:** `typeform`
**Service:** `forms`
**App name:** `typeform-forms`

## Actions

### list_forms

List forms (surveys).

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "typeform",
    "service": "forms",
    "action": "list_forms",
    "params": {"page_size": 50}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `page_size` | integer | no | Maximum forms to return |

### get_form

Get form details by ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "typeform",
    "service": "forms",
    "action": "get_form",
    "params": {"form_id": "FORM_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `form_id` | string | yes | Form ID |

### list_responses

List form responses with optional date filter.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "typeform",
    "service": "forms",
    "action": "list_responses",
    "params": {
      "form_id": "FORM_ID",
      "page_size": 100,
      "since": "2026-03-01T00:00:00Z",
      "until": "2026-03-31T23:59:59Z"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `form_id` | string | yes | Form ID |
| `page_size` | integer | no | Maximum responses to return |
| `since` | string | no | Start of date range (ISO8601) |
| `until` | string | no | End of date range (ISO8601) |

## Resources

- [Typeform API Reference](https://developer.typeform.com/openapi/reference/)
