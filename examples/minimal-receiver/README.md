# Minimal Receiver Outline

1. Advertise `_ariacast._tcp` with required TXT records
2. Accept WebSocket connections on `/ariacast/v1/stream`
3. Validate `hello`, return `welcome` with `session_id`
4. Parse AriaCast binary frame header and payload
5. Handle `control` messages and emit `error` on violations
