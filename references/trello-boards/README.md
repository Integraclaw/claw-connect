# Trello Boards Action Reference

**Provider:** `trello`
**Service:** `boards`
**App name:** `trello-boards`

## Actions

### list_boards

List accessible boards.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "trello",
    "service": "boards",
    "action": "list_boards",
    "params": {}
  }'
```

No parameters required.

### get_board

Get a board by ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "trello",
    "service": "boards",
    "action": "get_board",
    "params": {"board_id": "BOARD_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `board_id` | string | yes | Board ID |

### list_lists

List lists (columns) on a board.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "trello",
    "service": "boards",
    "action": "list_lists",
    "params": {"board_id": "BOARD_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `board_id` | string | yes | Board ID |

### list_cards

List cards in a list.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "trello",
    "service": "boards",
    "action": "list_cards",
    "params": {"list_id": "LIST_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `list_id` | string | yes | List ID |

### create_card

Create a new card in a list.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "trello",
    "service": "boards",
    "action": "create_card",
    "params": {
      "list_id": "LIST_ID",
      "name": "New task",
      "desc": "Task description and details"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `list_id` | string | yes | List ID |
| `name` | string | yes | Card name |
| `desc` | string | no | Card description |

## Resources

- [Trello API Reference](https://developer.atlassian.com/cloud/trello/rest/)
