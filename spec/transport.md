# AriaCast Transport

**Version:** v0.1 (Draft)

## Endpoint format

- Scheme: `ws://` (v0.1 default)
- Host: receiver LAN IP or hostname
- Port: announced via mDNS SRV record
- Path: `/ariacast/v1/stream`

Example:

```text
ws://192.168.1.40:12889/ariacast/v1/stream
```

## Connection lifecycle

1. TCP connect
2. WebSocket upgrade
3. Handshake (`hello` -> `welcome`)
4. Active session (text + binary frames)
5. Graceful close (`1000`) or protocol/error close (`1002`, `1011`)

## Handshake messages

### `hello` (Sender/Controller -> Receiver)

```json
{
  "type": "hello",
  "version": "0.1",
  "role": "sender",
  "device": {
    "id": "android-7f3a",
    "name": "Pixel 9"
  },
  "audio": {
    "codec": "pcm_s16le",
    "sample_rate": 48000,
    "channels": 2,
    "frame_duration_us": 20000
  },
  "capabilities": ["control", "metadata", "latency_update"]
}
```

### `welcome` (Receiver -> Sender/Controller)

```json
{
  "type": "welcome",
  "version": "0.1",
  "session_id": "6e322c67-33b4-4fdb-bf7f-3c3083e31df7",
  "server": {
    "id": "receiver-lr-01",
    "name": "Living Room Receiver"
  },
  "accepted_audio": {
    "codec": "pcm_s16le",
    "sample_rate": 48000,
    "channels": 2,
    "frame_duration_us": 20000
  },
  "latency_target_us": 120000,
  "capabilities": ["control", "metadata", "latency_update"]
}
```

## Message classes

| WebSocket frame type | Payload type | Allowed messages |
|---|---|---|
| Text | JSON UTF-8 object | `hello`, `welcome`, `control`, `error` |
| Binary | AriaCast audio frame | audio data only |

## Session handling

- `session_id` from `welcome` identifies the active stream session.
- All post-handshake JSON messages SHOULD include `session_id`.
- Receiver MUST reject binary frames before a successful handshake.
- Receiver SHOULD close with `1002` on invalid message order.

## Extensibility rules

- Receivers MUST ignore unknown JSON fields.
- Senders MUST ignore unknown fields in `welcome`.
- Unknown `type` values MUST trigger an `error` response.
