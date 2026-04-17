# Minimal Client Outline

1. Discover receiver via `_ariacast._tcp`
2. Open `ws://<host>:<port>/ariacast/v1/stream`
3. Send `hello`
4. Wait for `welcome`
5. Send binary PCM frames using the 16-byte AriaCast header
