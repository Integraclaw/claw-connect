# Notion Action Reference

**Provider:** `notion`
**Services:** `pages`, `databases`, `blocks`

Notion actions are split across three services:

| Service | App Name | Actions |
|---------|----------|---------|
| Pages | `notion-pages` | search, get, create, update |
| Databases | `notion-databases` | query, get, create |
| Blocks | `notion-blocks` | get_children, append, delete |

---

## Pages

### search

Search pages.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "notion",
    "service": "pages",
    "action": "search",
    "params": {"query": "meeting notes", "page_size": 10}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | no | Search query text |
| `page_size` | integer | no | Number of results (default 10, max 100) |
| `start_cursor` | string | no | Pagination cursor |

### get

Get a page by ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "notion",
    "service": "pages",
    "action": "get",
    "params": {"page_id": "PAGE_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `page_id` | string | yes | Page ID |

### create

Create a new page.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "notion",
    "service": "pages",
    "action": "create",
    "params": {
      "parent_database_id": "DATABASE_ID",
      "title": "Meeting Notes",
      "properties": {
        "Status": {"select": {"name": "Active"}}
      }
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `title` | string | yes | Page title |
| `parent_page_id` | string | no | Parent page ID (creates as child page) |
| `parent_database_id` | string | no | Parent database ID (creates as database entry) |
| `properties` | object | no | Page properties (for database entries, must match schema) |

### update

Update a page.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "notion",
    "service": "pages",
    "action": "update",
    "params": {
      "page_id": "PAGE_ID",
      "properties": {"Status": {"select": {"name": "Done"}}}
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `page_id` | string | yes | Page ID to update |
| `properties` | object | no | Properties to update |
| `archived` | boolean | no | Set to `true` to archive the page |

---

## Databases

### query

Query a database with filters and sorts.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "notion",
    "service": "databases",
    "action": "query",
    "params": {
      "database_id": "DATABASE_ID",
      "filter": {"property": "Status", "select": {"equals": "Active"}},
      "sorts": [{"property": "Created", "direction": "descending"}],
      "page_size": 50
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `database_id` | string | yes | Database ID |
| `filter` | object | no | Notion filter object |
| `sorts` | array | no | Array of sort objects |
| `page_size` | integer | no | Number of results (default 10, max 100) |
| `start_cursor` | string | no | Pagination cursor |

### get

Get database metadata and schema.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "notion",
    "service": "databases",
    "action": "get",
    "params": {"database_id": "DATABASE_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `database_id` | string | yes | Database ID |

### create

Create a new database.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "notion",
    "service": "databases",
    "action": "create",
    "params": {
      "parent_page_id": "PAGE_ID",
      "title": "Task Tracker",
      "properties": {
        "Name": {"title": {}},
        "Status": {"select": {"options": [{"name": "To Do"}, {"name": "Done"}]}}
      }
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `parent_page_id` | string | yes | Parent page ID |
| `title` | string | yes | Database title |
| `properties` | object | yes | Database property schema |

---

## Blocks

### get_children

Get block children (page content).

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "notion",
    "service": "blocks",
    "action": "get_children",
    "params": {"block_id": "PAGE_OR_BLOCK_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `block_id` | string | yes | Block or page ID |
| `page_size` | integer | no | Number of results (default 100, max 100) |
| `start_cursor` | string | no | Pagination cursor |

### append

Append blocks to a page or block.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "notion",
    "service": "blocks",
    "action": "append",
    "params": {
      "block_id": "PAGE_ID",
      "children": [
        {
          "type": "paragraph",
          "paragraph": {
            "rich_text": [{"type": "text", "text": {"content": "Hello from IntegraClaw"}}]
          }
        }
      ]
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `block_id` | string | yes | Block or page ID to append to |
| `children` | array | yes | Array of block objects |

### delete

Delete (archive) a block.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "notion",
    "service": "blocks",
    "action": "delete",
    "params": {"block_id": "BLOCK_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `block_id` | string | yes | Block ID to delete (archive) |

---

## Common Block Types (for append)

- `paragraph` — Text paragraph
- `heading_1`, `heading_2`, `heading_3` — Headings
- `bulleted_list_item`, `numbered_list_item` — List items
- `to_do` — Checkbox item
- `code` — Code block
- `quote` — Quote block
- `divider` — Horizontal divider

## Filter Operators

- `equals`, `does_not_equal`
- `contains`, `does_not_contain`
- `starts_with`, `ends_with`
- `is_empty`, `is_not_empty`
- `greater_than`, `less_than`

## Resources

- [Notion API Reference](https://developers.notion.com/reference/intro)
- [Filter Reference](https://developers.notion.com/reference/post-database-query-filter)
