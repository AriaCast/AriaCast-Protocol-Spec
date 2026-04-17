# AriaCast Control Messages

**Version:** v0.1 (Draft)

Control messages use WebSocket text frames with JSON payload.

## Envelope

```json
{
  "type": "control",
  "session_id": "6e322c67-33b4-4fdb-bf7f-3c3083e31df7",
  "command": "play",
  "timestamp_us": 0,
  "params": {}
}
```

## Commands

| Command | Direction | `params` |
|---|---|---|
| `play` | Sender/Controller -> Receiver | `{}` |
| `pause` | Sender/Controller -> Receiver | `{}` |
| `stop` | Sender/Controller -> Receiver | `{}` |
| `latency_update` | Either direction | `{ "latency_target_us": <u32> }` |
| `metadata` | Sender -> Receiver | `{ "title": "...", "artist": "..." }` |

## Examples

### Pause

```json
{
  "type": "control",
  "session_id": "6e322c67-33b4-4fdb-bf7f-3c3083e31df7",
  "command": "pause",
  "timestamp_us": 2540000,
  "params": {}
}
```

### Latency update

```json
{
  "type": "control",
  "session_id": "6e322c67-33b4-4fdb-bf7f-3c3083e31df7",
  "command": "latency_update",
  "timestamp_us": 2600000,
  "params": {
    "latency_target_us": 150000
  }
}
```

### Metadata update

```json
{
  "type": "control",
  "session_id": "6e322c67-33b4-4fdb-bf7f-3c3083e31df7",
  "command": "metadata",
  "timestamp_us": 3000000,
  "params": {
    "title": "Night Drive",
    "artist": "Aria Ensemble"
  }
}
```

## Extensibility

- New commands MUST use new `command` values.
- Receivers MUST ignore unknown fields in `params`.
- Unknown `command` MUST return `error` with `UNKNOWN_COMMAND`.
