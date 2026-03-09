# Google Forms Action Reference

**Provider:** `google`
**Service:** `forms`
**App name:** `google-forms`

## Actions

### get_form

Retrieve form metadata, structure, and question definitions.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "forms",
    "action": "get_form",
    "params": {"form_id": "1FAIpQLSd...xyz123"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `form_id` | string | yes | The ID of the form (from the form URL) |

### create_form

Create a new Google Form with a title.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "forms",
    "action": "create_form",
    "params": {"title": "Customer Feedback Survey"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `title` | string | yes | Title for the new form |

### list_responses

List form responses with pagination.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "forms",
    "action": "list_responses",
    "params": {
      "form_id": "1FAIpQLSd...xyz123",
      "page_size": 50,
      "page_token": "NEXT_PAGE_TOKEN"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `form_id` | string | yes | The ID of the form |
| `page_size` | integer | no | Number of responses to return (default varies) |
| `page_token` | string | no | Pagination token from previous response |

### get_response

Retrieve a single form response by ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "forms",
    "action": "get_response",
    "params": {
      "form_id": "1FAIpQLSd...xyz123",
      "response_id": "1i9_abc123xyz"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `form_id` | string | yes | The ID of the form |
| `response_id` | string | yes | The ID of the response to fetch |
