# Google Slides Action Reference

**Provider:** `google`
**Service:** `slides`
**App name:** `google-slides`

## Actions

### get_presentation

Retrieve metadata and structure of a Google Slides presentation.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "slides",
    "action": "get_presentation",
    "params": {"presentation_id": "1a2b3c4d5e6f7g8h9i0j"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `presentation_id` | string | yes | The ID of the presentation (from the URL) |

### create_presentation

Create a new blank Google Slides presentation.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "slides",
    "action": "create_presentation",
    "params": {"title": "Q4 Product Launch Deck"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `title` | string | yes | Title for the new presentation |

### get_page

Get a specific slide/page from a presentation by its object ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "slides",
    "action": "get_page",
    "params": {
      "presentation_id": "1a2b3c4d5e6f7g8h9i0j",
      "page_id": "p"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `presentation_id` | string | yes | The ID of the presentation |
| `page_id` | string | yes | The object ID of the slide (e.g. `p` for first slide) |

### batch_update

Apply multiple updates to a presentation in a single request (add slides, insert text, update shapes, etc.).

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "slides",
    "action": "batch_update",
    "params": {
      "presentation_id": "1a2b3c4d5e6f7g8h9i0j",
      "requests": [
        {"createSlide": {"slideLayoutReference": {"predefinedLayout": "TITLE_AND_BODY"}}},
        {"insertText": {"objectId": "TEXT_BOX_ID", "text": "Hello World", "insertionIndex": 0}}
      ]
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `presentation_id` | string | yes | The ID of the presentation |
| `requests` | array | yes | Array of update request objects (createSlide, insertText, etc.) |
