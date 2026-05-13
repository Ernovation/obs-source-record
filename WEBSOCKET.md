# Source Record — WebSocket API

Uses the obs-websocket API. Since OBS Studio 28, obs-websocket is bundled with OBS and no separate plugin installation is required. All requests and events use the vendor name `source-record`.

## Common request fields

These fields are accepted by every request:

| Field | Type | Description |
|-------|------|-------------|
| `source` | string | Name of the OBS source that has the filter. If omitted, all sources with a Source Record filter are targeted. |
| `filter` | string | Name of the specific Source Record filter on the source. If omitted, the first matching filter is used (or a new one is created when the request creates a recording). |

## Requests

### `record_start`

Start recording a source.

| Field | Type | Description |
|-------|------|-------------|
| `source` | string | See common fields. |
| `filter` | string | See common fields. |
| `filename` | string | *(optional)* Output filename/path override. Supports strftime-style `%` tokens. |
| `max_seconds` | integer | *(optional)* Stop recording automatically after this many seconds. |
| `stop_existing` | bool | *(optional)* Stop any in-progress recording before starting the new one. |

**Response:** `{ "success": true }`

---

### `record_stop`

Stop recording a source.

| Field | Type | Description |
|-------|------|-------------|
| `source` | string | See common fields. |
| `filter` | string | See common fields. |

**Response:** `{ "success": true }`

---

### `record_pause`

Pause an active recording.

| Field | Type | Description |
|-------|------|-------------|
| `source` | string | See common fields. |
| `filter` | string | See common fields. |

**Response:** `{ "success": true }`

---

### `record_unpause`

Resume a paused recording.

| Field | Type | Description |
|-------|------|-------------|
| `source` | string | See common fields. |
| `filter` | string | See common fields. |

**Response:** `{ "success": true }`

---

### `record_split`

Split the current recording into a new file (requires a muxer that supports file splitting).

| Field | Type | Description |
|-------|------|-------------|
| `source` | string | See common fields. |
| `filter` | string | See common fields. |

**Response:** `{ "success": true }`

---

### `record_add_chapter`

Add a chapter marker to the current recording.

| Field | Type | Description |
|-------|------|-------------|
| `source` | string | See common fields. |
| `filter` | string | See common fields. |
| `chapter_name` | string | *(optional)* Name of the chapter. |

**Response:** `{ "success": true }`

---

### `replay_buffer_start`

Start the replay buffer for a source.

| Field | Type | Description |
|-------|------|-------------|
| `source` | string | See common fields. |
| `filter` | string | See common fields. |
| `filename` | string | *(optional)* Filename format override for saved replays. |
| `stop_existing` | bool | *(optional)* Stop any running replay buffer before starting. |

**Response:** `{ "success": true }`

---

### `replay_buffer_stop`

Stop the replay buffer for a source.

| Field | Type | Description |
|-------|------|-------------|
| `source` | string | See common fields. |
| `filter` | string | See common fields. |

**Response:** `{ "success": true }`

---

### `replay_buffer_save`

Save the current replay buffer to disk.

| Field | Type | Description |
|-------|------|-------------|
| `source` | string | See common fields. |
| `filter` | string | See common fields. |

**Response:** `{ "success": true }`

---

### `stream_start`

Start streaming a source.

| Field | Type | Description |
|-------|------|-------------|
| `source` | string | See common fields. |
| `filter` | string | See common fields. |
| `server` | string | *(optional)* RTMP/WHIP server URL override. |
| `key` | string | *(optional)* Stream key override. |
| `stop_existing` | bool | *(optional)* Stop any active stream before starting the new one. |

**Response:** `{ "success": true }`

---

### `stream_stop`

Stop streaming a source.

| Field | Type | Description |
|-------|------|-------------|
| `source` | string | See common fields. |
| `filter` | string | See common fields. |

**Response:** `{ "success": true }`

---

## Events

All events include the following base fields:

| Field | Type | Description |
|-------|------|-------------|
| `source` | string | Name of the OBS source the filter is attached to. |
| `filter` | string | Name of the Source Record filter. |

### `RecordingStarted`

Fired when a recording output starts successfully.

| Field | Type | Description |
|-------|------|-------------|
| `source` | string | Source name. |
| `filter` | string | Filter name. |
| `outputPath` | string | Path of the file being recorded. |

### `RecordingStopped`

Fired when a recording output stops.

| Field | Type | Description |
|-------|------|-------------|
| `source` | string | Source name. |
| `filter` | string | Filter name. |
| `outputPath` | string | Path of the file that was recorded. |

### `ReplayBufferStarted`

Fired when the replay buffer starts successfully.

| Field | Type | Description |
|-------|------|-------------|
| `source` | string | Source name. |
| `filter` | string | Filter name. |

### `ReplayBufferStopped`

Fired when the replay buffer stops.

| Field | Type | Description |
|-------|------|-------------|
| `source` | string | Source name. |
| `filter` | string | Filter name. |

### `ReplayBufferSaved`

Fired when a replay is saved to disk. The file is fully written and closed by the time this event is emitted.

| Field | Type | Description |
|-------|------|-------------|
| `source` | string | Source name. |
| `filter` | string | Filter name. |
| `outputPath` | string | Full path of the saved replay file. |

### `StreamingStarted`

Fired when a streaming output starts successfully.

| Field | Type | Description |
|-------|------|-------------|
| `source` | string | Source name. |
| `filter` | string | Filter name. |
| `serverUrl` | string | The RTMP/WHIP server URL being streamed to. |

### `StreamingStopped`

Fired when a streaming output stops.

| Field | Type | Description |
|-------|------|-------------|
| `source` | string | Source name. |
| `filter` | string | Filter name. |
| `serverUrl` | string | The RTMP/WHIP server URL that was streamed to. |
