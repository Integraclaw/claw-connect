# Google Drive Action Reference

**Provider:** `google`
**Service:** `drive`
**App name:** `google-drive`

## Actions

### list_files

List files in Drive.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "drive",
    "action": "list_files",
    "params": {"page_size": 20}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `page_size` | integer | no | Number of files to return (default 20) |
| `page_token` | string | no | Pagination token |
| `order_by` | string | no | Sort order (e.g. `modifiedTime desc`) |

### search_files

Search files using Drive query syntax.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "drive",
    "action": "search_files",
    "params": {"query": "name contains '\''report'\'' and mimeType='\''application/pdf'\''"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | yes | Drive search query |
| `page_size` | integer | no | Max results (default 20) |
| `page_token` | string | no | Pagination token |

### get_file

Get file metadata.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "drive",
    "action": "get_file",
    "params": {"file_id": "FILE_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `file_id` | string | yes | File ID |

### create_folder

Create a new folder.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "drive",
    "action": "create_folder",
    "params": {"name": "Project Reports", "parent_id": "PARENT_FOLDER_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | yes | Folder name |
| `parent_id` | string | no | Parent folder ID |

### copy_file

Copy a file.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "drive",
    "action": "copy_file",
    "params": {"file_id": "FILE_ID", "name": "Copy of Report"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `file_id` | string | yes | ID of the file to copy |
| `name` | string | no | Name for the copy |
| `parent_id` | string | no | Destination folder ID |

### update_file

Update file metadata.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "drive",
    "action": "update_file",
    "params": {"file_id": "FILE_ID", "name": "Renamed File"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `file_id` | string | yes | File ID to update |
| `name` | string | no | New file name |
| `description` | string | no | New description |

### delete_file

Delete a file.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "drive",
    "action": "delete_file",
    "params": {"file_id": "FILE_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `file_id` | string | yes | File ID to delete |

## Query Operators (for search_files)

- `name = 'exact name'`
- `name contains 'partial'`
- `mimeType = 'application/pdf'`
- `'folderId' in parents`
- `trashed = false`
- `modifiedTime > '2024-01-01T00:00:00'`

Combine with `and`:
```
name contains 'report' and mimeType = 'application/pdf'
```

## Common MIME Types

- `application/vnd.google-apps.document` — Google Docs
- `application/vnd.google-apps.spreadsheet` — Google Sheets
- `application/vnd.google-apps.presentation` — Google Slides
- `application/vnd.google-apps.folder` — Folder

## Resources

- [Google Drive API Reference](https://developers.google.com/drive/api/reference/rest/v3)
- [Search Query Syntax](https://developers.google.com/drive/api/guides/search-files)
