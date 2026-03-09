# Monday Boards Action Reference

**Provider:** `monday`
**Service:** `boards`
**App name:** `monday-boards`

## Actions

### list_boards

List accessible boards.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "monday",
    "service": "boards",
    "action": "list_boards",
    "params": {"limit": 50}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | integer | no | Maximum boards to return |

### get_board

Get a board by ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "monday",
    "service": "boards",
    "action": "get_board",
    "params": {"board_id": "1234567890"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `board_id` | string | yes | Board ID |

### list_items

List items on a board.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "monday",
    "service": "boards",
    "action": "list_items",
    "params": {
      "board_id": "1234567890",
      "limit": 100
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `board_id` | string | yes | Board ID |
| `limit` | integer | no | Maximum items to return |

### create_item

Create a new item on a board.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "monday",
    "service": "boards",
    "action": "create_item",
    "params": {
      "board_id": "1234567890",
      "item_name": "New Project",
      "column_values": {
        "status": {"label": "Working on it"},
        "text": "Project description"
      }
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `board_id` | string | yes | Board ID |
| `item_name` | string | yes | Item name |
| `column_values` | object | no | Column ID to value map |

## Resources

- [Monday.com API Reference](https://developer.monday.com/api-reference/docs)
