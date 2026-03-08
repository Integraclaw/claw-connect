# OneDrive API Reference

**App name:** `microsoft-onedrive`
**Provider:** `microsoft` | **Service:** `onedrive`
**Base URL:** `https://graph.microsoft.com/v1.0`

## IntegraClaw Actions

| Action | Description |
|--------|-------------|
| `list_files` | List files in root or folder |
| `search_files` | Search files |
| `upload_file` | Upload a file |
| `download_file` | Download a file |

### Example: List Files
```json
{
  "provider": "microsoft",
  "service": "onedrive",
  "action": "list_files",
  "params": {}
}
```

## Native API Endpoints

### Get User's Drive
```bash
GET https://graph.microsoft.com/v1.0/me/drive
```

### List Root Children
```bash
GET https://graph.microsoft.com/v1.0/me/drive/root/children
```

### Get Item by ID
```bash
GET https://graph.microsoft.com/v1.0/me/drive/items/{item-id}
```

### Get Item by Path
```bash
GET https://graph.microsoft.com/v1.0/me/drive/root:/Documents/file.txt
```

### List Folder Children by Path
```bash
GET https://graph.microsoft.com/v1.0/me/drive/root:/Documents:/children
```

### Create Folder
```bash
POST https://graph.microsoft.com/v1.0/me/drive/root/children
Content-Type: application/json

{
  "name": "New Folder",
  "folder": {}
}
```

### Upload File (up to 4MB)
```bash
PUT https://graph.microsoft.com/v1.0/me/drive/root:/filename.txt:/content
Content-Type: text/plain

{file content}
```

### Delete Item
```bash
DELETE https://graph.microsoft.com/v1.0/me/drive/items/{item-id}
```

### Search Files
```bash
GET https://graph.microsoft.com/v1.0/me/drive/root/search(q='query')
```

### Create Sharing Link
```bash
POST https://graph.microsoft.com/v1.0/me/drive/items/{item-id}/createLink
Content-Type: application/json

{"type": "view", "scope": "anonymous"}
```

### Recent Files
```bash
GET https://graph.microsoft.com/v1.0/me/drive/recent
```

## Notes

- Uses Microsoft Graph API
- Use colon (`:`) syntax for path-based addressing
- Simple uploads limited to 4MB
- Supports OData query parameters: `$select`, `$expand`, `$filter`, `$orderby`, `$top`

## Resources

- [OneDrive Developer Documentation](https://learn.microsoft.com/en-us/onedrive/developer/)
- [DriveItem Resource](https://learn.microsoft.com/en-us/graph/api/resources/driveitem)
