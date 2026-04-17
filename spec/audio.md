# AriaCast Audio Frames

**Version:** v0.1 (Draft)

## Supported formats (v0.1)

| Field | Value |
|---|---|
| Codec | `pcm_s16le` |
| Sample rate | `48000` Hz default |
| Channels | `1` (mono) or `2` (stereo) |
| Frame duration | `20000` us recommended |

## Binary frame envelope

Each WebSocket binary message contains exactly one AriaCast frame.

### Header layout (16 bytes, little-endian)

```text
Offset  Size  Type    Field
0       8     u64     timestamp_us
8       4     u32     frame_id
12      4     u32     payload_length
16      N     bytes   payload
```

### Field definitions

| Field | Description |
|---|---|
| `timestamp_us` | Stream timestamp in microseconds (see `timing.md`) |
| `frame_id` | Monotonic per-session frame counter starting at 0 |
| `payload_length` | Number of payload bytes following the header |
| `payload` | Raw PCM data for this frame |

## PCM payload layout

For stereo (`channels=2`), samples are interleaved:

```text
L0, R0, L1, R1, L2, R2, ...
```

Each sample is signed 16-bit little-endian.

For mono (`channels=1`), payload is:

```text
M0, M1, M2, ...
```

## Validation requirements

- `payload_length` MUST equal remaining bytes in binary frame.
- `frame_id` SHOULD increment by 1 per frame.
- Receiver SHOULD drop malformed frames and emit `error`.

## Future codec extensibility

Future versions may add codecs (e.g., Opus, AAC) via handshake negotiation:
- sender advertises preferred codec in `hello.audio.codec`
- receiver confirms selected codec in `welcome.accepted_audio.codec`
- unknown codecs MUST be rejected with `UNSUPPORTED_FORMAT`
