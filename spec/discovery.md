# AriaCast Discovery

**Version:** v0.1 (Draft)

## mDNS service

Receivers advertise:

- Service type: `_ariacast._tcp`
- Instance name: human-readable receiver name
- Port: AriaCast WebSocket port

## TXT records

| Key | Required | Example | Description |
|---|---|---|---|
| `name` | yes | `Living Room Receiver` | Display name |
| `version` | yes | `0.1` | Protocol version |
| `path` | yes | `/ariacast/v1/stream` | WebSocket path |
| `caps` | no | `control,metadata,latency_update` | Capabilities |
| `id` | no | `receiver-lr-01` | Stable receiver ID |

## Discovery flow

1. Sender browses `_ariacast._tcp`.
2. Sender resolves SRV + TXT records.
3. Sender builds endpoint `ws://<host>:<port><path>`.
4. Sender initiates transport handshake.

## Fallback manual connection

If mDNS is unavailable, sender MAY accept manual endpoint input:

```text
ws://192.168.1.40:12889/ariacast/v1/stream
```

Manual mode SHOULD still require normal handshake validation.
