# Google Docs Action Reference

**Provider:** `google`
**Service:** `docs`
**App name:** `google-docs`

## Actions

### create_document

Create a new blank Google Docs document.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "docs",
    "action": "create_document",
    "params": {
      "title": "Meeting Notes"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `title` | string | **yes** | Document title |

### get_document

Get the full content and metadata of a document.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "docs",
    "action": "get_document",
    "params": {
      "document_id": "DOCUMENT_ID"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `document_id` | string | **yes** | Document ID |

### insert_text

Insert text into a document. By default appends to the end of the document body.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "docs",
    "action": "insert_text",
    "params": {
      "document_id": "DOCUMENT_ID",
      "text": "Hello, this is appended text.\n"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `document_id` | string | **yes** | Document ID |
| `text` | string | **yes** | Text to insert |
| `index` | integer | no | Position index (1-based). If omitted, appends to end of body. |

### replace_text

Find and replace all occurrences of text in a document.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "docs",
    "action": "replace_text",
    "params": {
      "document_id": "DOCUMENT_ID",
      "find": "old text",
      "replace": "new text",
      "match_case": true
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `document_id` | string | **yes** | Document ID |
| `find` | string | **yes** | Text to find |
| `replace` | string | **yes** | Replacement text |
| `match_case` | boolean | no | Case-sensitive matching (default false) |
