# Google Drive API Reference

**App name:** `google-drive`
**Provider:** `google` | **Service:** `drive`
**Base URL:** `https://www.googleapis.com/drive/v3`

## IntegraClaw Actions

| Action | Description |
|--------|-------------|
| `list_files` | List files and folders |
| `search_files` | Search files by query |
| `get_file` | Get file metadata |
| `download_file` | Download file content |
| `create_folder` | Create a new folder |
| `upload_file` | Upload a file |
| `share_file` | Share a file with someone |

### Example: List Files
```json
{
  "provider": "google",
  "service": "drive",
  "action": "list_files",
  "params": {
    "page_size": 10,
    "query": "name contains 'report'"
  }
}
```

## Native API Endpoints

### List Files
```bash
GET https://www.googleapis.com/drive/v3/files?pageSize=10&fields=files(id,name,mimeType,createdTime,modifiedTime,size)
```

### Search Files
```bash
GET https://www.googleapis.com/drive/v3/files?q=name%20contains%20'report'&pageSize=10
```

### Get File Metadata
```bash
GET https://www.googleapis.com/drive/v3/files/{fileId}?fields=id,name,mimeType,size,createdTime
```

### Download File Content
```bash
GET https://www.googleapis.com/drive/v3/files/{fileId}?alt=media
```

### Export Google Docs (to PDF)
```bash
GET https://www.googleapis.com/drive/v3/files/{fileId}/export?mimeType=application/pdf
```

### Create Folder
```bash
POST https://www.googleapis.com/drive/v3/files
Content-Type: application/json

{
  "name": "New Folder",
  "mimeType": "application/vnd.google-apps.folder"
}
```

### Update File Metadata
```bash
PATCH https://www.googleapis.com/drive/v3/files/{fileId}
Content-Type: application/json

{
  "name": "Renamed File"
}
```

### Delete File
```bash
DELETE https://www.googleapis.com/drive/v3/files/{fileId}
```

### Share File
```bash
POST https://www.googleapis.com/drive/v3/files/{fileId}/permissions
Content-Type: application/json

{
  "role": "reader",
  "type": "user",
  "emailAddress": "user@example.com"
}
```

## Query Operators

Use in the `q` parameter:
- `name = 'exact name'`
- `name contains 'partial'`
- `mimeType = 'application/pdf'`
- `'folderId' in parents`
- `trashed = false`
- `modifiedTime > '2024-01-01T00:00:00'`

## Common MIME Types

- `application/vnd.google-apps.document` - Google Docs
- `application/vnd.google-apps.spreadsheet` - Google Sheets
- `application/vnd.google-apps.presentation` - Google Slides
- `application/vnd.google-apps.folder` - Folder

## Resources

- [API Overview](https://developers.google.com/workspace/drive/api/reference/rest/v3)
- [List Files](https://developers.google.com/drive/api/reference/rest/v3/files/list)
- [Search Files](https://developers.google.com/drive/api/guides/search-files)
