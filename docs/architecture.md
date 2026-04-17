# AriaCast Architecture

AriaCast v0.1 uses:
- mDNS for receiver discovery
- one WebSocket session per stream
- JSON text frames for control plane
- binary frames for PCM data plane
