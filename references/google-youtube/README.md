# Google YouTube Action Reference

**Provider:** `google`
**Service:** `youtube`
**App name:** `google-youtube`

## Actions

### list_channels

List YouTube channels for the authenticated user or by channel ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "youtube",
    "action": "list_channels",
    "params": {"mine": true, "max_results": 10}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `mine` | boolean | no | If true, return channels for authenticated user |
| `channel_id` | string | no | Specific channel ID (use with mine=false) |
| `max_results` | integer | no | Max channels to return (default 25) |

### search_videos

Search for videos by query string.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "youtube",
    "action": "search_videos",
    "params": {
      "query": "integrating APIs with Go",
      "max_results": 15,
      "page_token": "CAoQAA"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | yes | Search query string |
| `max_results` | integer | no | Max results to return (default 25) |
| `page_token` | string | no | Pagination token from previous response |

### get_video

Get video metadata including title, description, statistics, and content details.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "youtube",
    "action": "get_video",
    "params": {"video_id": "dQw4w9WgXcQ"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `video_id` | string | yes | YouTube video ID (11-character string) |

### list_playlists

List playlists for a channel or the authenticated user.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "youtube",
    "action": "list_playlists",
    "params": {
      "channel_id": "UC_x5XG1OV2P6uZZ5FSM9Ttw",
      "mine": false,
      "max_results": 20
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `channel_id` | string | no | Filter by channel ID |
| `mine` | boolean | no | If true, return playlists for authenticated user |
| `max_results` | integer | no | Max playlists to return (default 25) |

### list_playlist_items

List videos in a playlist.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "youtube",
    "action": "list_playlist_items",
    "params": {
      "playlist_id": "PLrAXtmErZgOeiKm4sgNOknGvNjby9efdfg",
      "max_results": 50,
      "page_token": "EAAaBlBUOk..."
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `playlist_id` | string | yes | The ID of the playlist |
| `max_results` | integer | no | Max items to return (default 25) |
| `page_token` | string | no | Pagination token from previous response |
