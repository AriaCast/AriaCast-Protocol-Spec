# AriaCast Protocol Overview

**Version:** v0.1 (Draft)

## Purpose

AriaCast defines an open protocol for low-latency audio streaming on local networks.

Primary goals:
- real-time LAN audio transport
- simple multi-language implementation
- local-only operation without cloud dependency
- forward-compatible extensibility

## Roles

| Role | Description |
|---|---|
| Sender | Captures or produces audio and transmits stream + control messages |
| Receiver | Accepts stream, buffers, decodes, and plays audio |
| Controller (optional) | Sends control commands to active session |

## Architecture

AriaCast uses one WebSocket connection per sender/receiver session.

- JSON text frames: handshake, control, and errors
- Binary frames: audio payload

Discovery is performed via mDNS (`_ariacast._tcp`) before connection.

## Typical data flow

1. Sender discovers receivers using mDNS.
2. Sender opens WebSocket to receiver.
3. Sender sends `hello` JSON message.
4. Receiver replies with `welcome` JSON message.
5. Sender streams binary audio frames.
6. Either side exchanges `control` JSON messages as needed.
7. Receiver may emit `error` JSON if protocol/state violations occur.

## Design philosophy

- **Local-first:** no cloud services required.
- **Open:** wire format is public and implementation-neutral.
- **Simple:** fixed binary frame header, explicit JSON control envelope.
- **Extensible:** negotiated capabilities and unknown-field tolerance.
