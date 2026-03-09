# Box Files Action Reference

**Provider:** `box`
**Service:** `files`
**App name:** `box-files`

## Actions

### list_folder_items

List items in a folder (use "0" for root).

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "box",
    "service": "files",
    "action": "list_folder_items",
    "params": {
      "folder_id": "0",
      "limit": 100
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `folder_id` | string | yes | Folder ID (default `0` for root) |
| `limit` | integer | no | Maximum items to return |

### get_file

Get file metadata.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "box",
    "service": "files",
    "action": "get_file",
    "params": {"file_id": "FILE_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `file_id` | string | yes | File ID |

### search

Search for files and folders.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "box",
    "service": "files",
    "action": "search",
    "params": {
      "query": "quarterly report",
      "limit": 50
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | yes | Search query |
| `limit` | integer | no | Maximum results |

### create_folder

Create a new folder.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "box",
    "service": "files",
    "action": "create_folder",
    "params": {
      "name": "Project Documents",
      "parent_id": "0"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | yes | Folder name |
| `parent_id` | string | yes | Parent folder ID |

## Resources

- [Box API Reference](https://developer.box.com/reference/)
