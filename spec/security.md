# AriaCast Security

**Version:** v0.1 (Draft)

## Current model (v0.1)

AriaCast v0.1 assumes a trusted local network.

- Transport defaults to unencrypted `ws://`
- No mandatory authentication in v0.1
- Implementations SHOULD bind only on LAN interfaces
- Implementations SHOULD allow user opt-in allowlists

## Threat considerations

On untrusted LANs, attackers may:
- inject control commands
- observe metadata
- disrupt playback

Use only on trusted networks until stronger security is enabled.

## Future security extensions

### Pairing

Planned pairing flow:
- receiver enters pairing mode
- sender proves possession of short code/token
- receiver stores sender identity key

### Authentication

Planned authenticated sessions:
- `hello` includes signed nonce/token
- receiver validates sender identity
- unauthorized sessions rejected with `UNAUTHORIZED`

### TLS support

Planned secure transport:
- `wss://` endpoint advertisement
- certificate pinning or trust-on-first-use options
- capability flag for mandatory secure transport
