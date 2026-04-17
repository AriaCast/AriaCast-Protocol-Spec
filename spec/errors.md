# AriaCast Error Model

**Version:** v0.1 (Draft)

## Standard error message

```json
{
  "type": "error",
  "session_id": "6e322c67-33b4-4fdb-bf7f-3c3083e31df7",
  "code": "UNSUPPORTED_FORMAT",
  "message": "Codec opus is not supported in v0.1",
  "recoverable": false
}
```

## Error fields

| Field | Type | Required | Description |
|---|---|---|---|
| `type` | string | yes | Always `error` |
| `session_id` | string | no | Session context when available |
| `code` | string | yes | Machine-readable error code |
| `message` | string | yes | Human-readable explanation |
| `recoverable` | boolean | yes | Whether sender can continue/retry |

## Error codes

| Code | Meaning | Typical action |
|---|---|---|
| `UNSUPPORTED_FORMAT` | Codec/channel/rate not accepted | Re-negotiate or stop |
| `INVALID_MESSAGE` | JSON malformed or missing required fields | Fix payload and retry |
| `INVALID_STATE` | Command not valid in current playback state | Adjust command sequence |
| `UNKNOWN_COMMAND` | `control.command` not recognized | Fallback/feature-detect |
| `PAYLOAD_MISMATCH` | Binary payload length/header mismatch | Drop frame and resend subsequent frames |
| `UNAUTHORIZED` | Future auth failure | Re-pair/re-authenticate |

## Recovery expectations

- Recoverable errors SHOULD keep WebSocket open.
- Non-recoverable errors MAY be followed by close `1002` or `1011`.
- Receiver SHOULD rate-limit repetitive error emission.
