# Notion API Reference

**App name:** `notion-pages`, `notion-databases`, `notion-blocks`
**Provider:** `notion` | **Services:** `pages`, `databases`, `blocks`
**Base URL:** `https://api.notion.com/v1`

## Required Headers

All Notion API requests require:
```
Notion-Version: 2022-06-28
```

## IntegraClaw Actions

### Pages
| Action | Description |
|--------|-------------|
| `search_pages` | Search for pages |
| `get_page` | Get a page |
| `create_page` | Create a page |
| `update_page` | Update page properties |

### Databases
| Action | Description |
|--------|-------------|
| `query_database` | Query a database |
| `get_database` | Get database metadata |
| `create_database` | Create a database |

### Blocks
| Action | Description |
|--------|-------------|
| `get_block_children` | Get block children |
| `append_block_children` | Append blocks to a page |
| `delete_block` | Delete a block |

### Example: Query Database
```json
{
  "provider": "notion",
  "service": "databases",
  "action": "query_database",
  "params": {
    "database_id": "DATABASE_ID"
  }
}
```

### Example: Create Page
```json
{
  "provider": "notion",
  "service": "pages",
  "action": "create_page",
  "params": {
    "parent_id": "DATABASE_ID",
    "properties": {
      "Name": {"title": [{"text": {"content": "New Page"}}]}
    }
  }
}
```

## Native API Endpoints

### Search
```bash
POST https://api.notion.com/v1/search
Content-Type: application/json
Notion-Version: 2022-06-28

{
  "query": "meeting notes",
  "filter": {"property": "object", "value": "page"}
}
```

### Get Page
```bash
GET https://api.notion.com/v1/pages/{pageId}
Notion-Version: 2022-06-28
```

### Create Page
```bash
POST https://api.notion.com/v1/pages
Content-Type: application/json
Notion-Version: 2022-06-28

{
  "parent": {"database_id": "DATABASE_ID"},
  "properties": {
    "Name": {"title": [{"text": {"content": "New Page"}}]},
    "Status": {"select": {"name": "Active"}}
  }
}
```

### Update Page Properties
```bash
PATCH https://api.notion.com/v1/pages/{pageId}
Content-Type: application/json
Notion-Version: 2022-06-28

{
  "properties": {
    "Status": {"select": {"name": "Done"}}
  }
}
```

### Query Database
```bash
POST https://api.notion.com/v1/databases/{databaseId}/query
Content-Type: application/json
Notion-Version: 2022-06-28

{
  "filter": {
    "property": "Status",
    "select": {"equals": "Active"}
  },
  "sorts": [{"property": "Created", "direction": "descending"}],
  "page_size": 100
}
```

### Get Database
```bash
GET https://api.notion.com/v1/databases/{databaseId}
Notion-Version: 2022-06-28
```

### Create Database
```bash
POST https://api.notion.com/v1/databases
Content-Type: application/json
Notion-Version: 2022-06-28

{
  "parent": {"page_id": "PARENT_PAGE_ID"},
  "title": [{"type": "text", "text": {"content": "New Database"}}],
  "properties": {
    "Name": {"title": {}},
    "Status": {"select": {"options": [{"name": "Active"}, {"name": "Done"}]}}
  }
}
```

### Get Block Children
```bash
GET https://api.notion.com/v1/blocks/{blockId}/children
Notion-Version: 2022-06-28
```

### Append Block Children
```bash
PATCH https://api.notion.com/v1/blocks/{blockId}/children
Content-Type: application/json
Notion-Version: 2022-06-28

{
  "children": [
    {
      "object": "block",
      "type": "paragraph",
      "paragraph": {
        "rich_text": [{"type": "text", "text": {"content": "New paragraph"}}]
      }
    }
  ]
}
```

### Delete Block
```bash
DELETE https://api.notion.com/v1/blocks/{blockId}
Notion-Version: 2022-06-28
```

### List Users
```bash
GET https://api.notion.com/v1/users
Notion-Version: 2022-06-28
```

## Filter Operators

- `equals`, `does_not_equal`
- `contains`, `does_not_contain`
- `starts_with`, `ends_with`
- `is_empty`, `is_not_empty`

## Block Types

- `paragraph` - Text paragraph
- `heading_1`, `heading_2`, `heading_3` - Headings
- `bulleted_list_item`, `numbered_list_item` - List items
- `to_do` - Checkbox item
- `code` - Code block
- `quote` - Quote block
- `divider` - Horizontal divider

## Notes

- All IDs are UUIDs (with or without hyphens)
- Notion uses long-lived tokens (no refresh needed)
- Always include `Notion-Version` header

## Resources

- [API Introduction](https://developers.notion.com/reference/intro)
- [Search](https://developers.notion.com/reference/post-search)
- [Query Database](https://developers.notion.com/reference/post-database-query)
- [Create Page](https://developers.notion.com/reference/post-page)
- [Get Block Children](https://developers.notion.com/reference/get-block-children)
