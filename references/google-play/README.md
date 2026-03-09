# Google Play Action Reference

**Provider:** `google`
**Service:** `play`
**App name:** `google-play`

## Actions

### list_reviews

List app reviews for an Android app in Google Play Console.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "play",
    "action": "list_reviews",
    "params": {
      "package_name": "com.example.myapp",
      "max_results": 100,
      "token": "REVIEW_PAGINATION_TOKEN"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `package_name` | string | yes | Android package name (e.g. `com.example.myapp`) |
| `max_results` | integer | no | Max reviews to return |
| `token` | string | no | Pagination token from previous response |

### get_review

Retrieve a single review by ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "play",
    "action": "get_review",
    "params": {
      "package_name": "com.example.myapp",
      "review_id": "gp:AOqpToE12345..."
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `package_name` | string | yes | Android package name |
| `review_id` | string | yes | The review ID (starts with `gp:AOqpTo...`) |

### reply_review

Reply to a user review (developer response).

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "play",
    "action": "reply_review",
    "params": {
      "package_name": "com.example.myapp",
      "review_id": "gp:AOqpToE12345...",
      "reply_text": "Thanks for your feedback! We have fixed this in the latest update."
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `package_name` | string | yes | Android package name |
| `review_id` | string | yes | The review ID to reply to |
| `reply_text` | string | yes | Developer reply text |
