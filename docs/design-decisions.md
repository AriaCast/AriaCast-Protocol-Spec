# AriaCast Design Decisions

- Chosen transport: WebSocket over TCP for broad client support
- Initial codec: PCM S16LE for implementation simplicity
- Timestamp unit: microseconds for precise scheduling
- Extensibility: capability negotiation in `hello`/`welcome`
