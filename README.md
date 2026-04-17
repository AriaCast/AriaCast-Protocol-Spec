# AriaCast Protocol Specification

AriaCast is an open, local-first protocol for real-time audio streaming over LAN networks.

This repository defines the AriaCast **v0.1 (Draft)** wire protocol and implementation guidance for senders, receivers, and optional controllers.

## Repository layout

- `spec/` — normative protocol specification
- `schemas/` — JSON Schemas for selected control-plane messages
- `docs/` — design notes and roadmap
- `examples/` — minimal implementation guidance

## Quick start

1. Start with `spec/README.md`
2. Implement transport in `spec/transport.md`
3. Implement binary audio framing in `spec/audio.md`
4. Implement timing/buffering in `spec/timing.md`

## Versioning

This draft targets **AriaCast v0.1**.
