# stream-failover-relay

[![License: CC0-1.0](https://img.shields.io/badge/License-CC0%201.0-lightgrey.svg)](http://creativecommons.org/publicdomain/zero/1.0/)

Pass-through stream relay that **remuxes without transcoding** and automatically **fails over** to a fallback input (another live source and/or a local file) when the primary stream goes down.

`stream-failover-relay` is a tool when you want:
- Low CPU load, zero GPU load and zero quality loss (no transcoding).
- Low latency.
- Always-on output for downstream consumers.
- A clean automatic switch to a "standby" stream ("BRB", slate, countdown, etc.).

## How it works

- **Relays** a stream from **primary input → output** without transcoding (packet copy / remux).
- **Detects primary failure** (disconnect, timeout, "no packets for N seconds", etc.).
- **Switches to fallback**: a backup live URL or a local media file.
- **Returns** to primary once it’s healthy again.

## What it does not do

- No video/audio transcoding, scaling, overlays, filters, or bitrate adaptation. But if you need any of that, please create a ticket, I have other projects that do just that.

## Quick start

```sh
stream-failover-relay \
    rtmp://my.server/live/main \
    /opt/brb.mp4 \
    rtmp://remote.server/live/out \
```

Here `rtmp://my.server/live/main` will be the primary source, `/opt/brb.mp4` the BRB placeholder, and `rtmp://remote.server/live/out` the output destination.

## Caveats

Both sources (primary and fallback) must have the same properties:
* Resolution.
* FPS.
* Codecs.
* Audio frame Rate.
* Audio channels.
* etc.

## Install

Go to [Releases](https://github.com/xaionaro-go/stream-failover-relay/releases) and download
the executable you need.

## Build from the source

```bash
apt install -y libavcodec-dev libavdevice-dev libavfilter-dev libavformat-dev libavutil-dev
git clone https://github.com/xaionaro-go/stream-failover-relay
cd stream-failover-relay
go build ./cmd/stream-failover-relay
```
