# Release Notes

## gt6-mirror-fix-publictest-2026-05-15

Unofficial experimental Windows test build for Gran Turismo 6 mirror issues in RPCS3.

Base:

```text
RPCS3 commit: b533a560e6df6f8aa1c271bfffbd1e39f500bd65
Embedded version before patch: 19374-b533a560
```

Intended fixes:

- Default 2D HUD rear-view mirror wrong-content/minimap-like artifact.
- Car/material flicker in default HUD and cockpit mirrors.

Recommended GT6 config:

```text
Read Color Buffers: true
Write Color Buffers: true
Asynchronous Texture Streaming: true
Multithreaded RSX: true
Resolution Scale: 100
```

ZIP SHA256:

```text
4e57112453e6453e7e85cd55bec187185d8cf0264649ecaf36f5c86ec59f280a
```

Known limits:

- Tested locally only on `BCES01893`.
- Not title-gated.
- Experimental, unofficial, and AI-assisted.
