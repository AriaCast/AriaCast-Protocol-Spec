# AriaCast Timing and Synchronization

**Version:** v0.1 (Draft)

## Timestamp format

`timestamp_us` is an unsigned 64-bit microsecond stream clock.

- Origin: start of session audio timeline (`0` at first sample)
- Units: microseconds
- Monotonic: MUST NOT go backward

## Playback synchronization model

Receiver playout time is:

```text
playout_time_us = frame.timestamp_us + latency_target_us
```

Where `latency_target_us` is provided in `welcome` (default `120000`).

## Buffering strategy

Recommended startup behavior:

1. Collect frames until buffered audio >= `latency_target_us`
2. Start playback at buffered playout target
3. Continue with steady-state buffer between 60 ms and 200 ms

## Jitter handling (basic)

- **Late frame**: if frame misses playout deadline, drop frame.
- **Small gap**: optionally perform zero-fill concealment for one frame.
- **Burst delay**: temporarily increase target latency (if supported).

Receiver SHOULD expose effective latency through control telemetry.

## Frame duration guidance

At 48 kHz:
- 20 ms frame = 960 samples/channel
- Stereo PCM payload = 960 * 2 * 2 = 3840 bytes

## Future multiroom sync

v0.1 does not define cross-receiver clock sync. A future version may add:
- shared wall-clock alignment
- drift correction messages
- group session synchronization IDs
