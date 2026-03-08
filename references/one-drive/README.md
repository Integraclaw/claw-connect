# OneDrive Action Reference

**Provider:** `microsoft`
**Service:** `onedrive`
**App name:** `microsoft-onedrive`

## Actions

### list_files

List files in a folder.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "microsoft",
    "service": "onedrive",
    "action": "list_files",
    "params": {"folder_path": "/Documents", "top": 20}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `folder_path` | string | no | Folder path (default: root). Use format like `/Documents/Reports` |
| `top` | integer | no | Number of items to return (default 20) |

### search_files

Search files in OneDrive.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "microsoft",
    "service": "onedrive",
    "action": "search_files",
    "params": {"query": "quarterly report", "top": 10}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | yes | Search query |
| `top` | integer | no | Max results (default 20) |

### get_file

Get file metadata.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "microsoft",
    "service": "onedrive",
    "action": "get_file",
    "params": {"item_id": "ITEM_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `item_id` | string | yes | Item ID (file or folder) |

### create_folder

Create a new folder.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "microsoft",
    "service": "onedrive",
    "action": "create_folder",
    "params": {"name": "Project Files", "parent_path": "/Documents"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | yes | Folder name |
| `parent_path` | string | no | Parent folder path (default: root) |

## Notes

- Uses Microsoft Graph API under the hood
- Download URLs in `@microsoft.graph.downloadUrl` are pre-authenticated

## Resources

- [OneDrive Developer Documentation](https://learn.microsoft.com/en-us/onedrive/developer/)
- [Microsoft Graph DriveItem](https://learn.microsoft.com/en-us/graph/api/resources/driveitem)
