# GT6 Mirror Fix Public Testing Notes

Date: 2026-05-15

Base for the public test build:

```text
repo: rpcs3-master-fresh
base commit: b533a560e6df6f8aa1c271bfffbd1e39f500bd65
embedded version before patch: 19374-b533a560
build path: rpcs3-master-fresh/bin/rpcs3.exe
```

## Scope

This is an experimental GT6 mirror workaround for public testing, not an upstream-ready PR.

Tested game/title:

```text
Gran Turismo 6
BCES01893
game version used in current tests: 1.05
```

The patch targets two visible mirror problems:

1. Default 2D HUD view rear-view mirror can show wrong content such as minimap-like data.
2. Default HUD and cockpit mirror car materials can flicker.

Important terminology: this is not GT6's bonnet view. GT6 bonnet view has no rear-view mirror in this test case.

## Patch Files

```text
rpcs3-master-fresh/rpcs3/Emu/RSX/Common/texture_cache.h
rpcs3-master-fresh/rpcs3/Emu/RSX/VK/VKDraw.cpp
```

## Fix Summary

Default HUD mirror final image:

* GT6 samples a mirror-sized color target at `0xc2200000`.
* In affected frames, the surface-cache overlap winner can become a later depth target instead of the exact color surface.
* The patch prefers the exact color overlap for the `0xc2200000`, `475x98`, `pitch=8192` mirror read.
* It also forces a copy for this mirror color read so the shader does not sample the still-bound render target directly.

Mirror material flicker:

* GT6 mirror material passes use a framebuffer-backed reflective cubemap at `0xc40c0000`.
* The same passes use a framebuffer-backed `1x1` lighting/material scalar at `0xc0000000`.
* Keeping those temporary cache entries alive stabilizes resource identity across mirror draws.
* The `1x1` scalar is changed from `copy_image_static` to `copy_image_dynamic` so cached hits refresh the current value instead of freezing stale lighting/material data.
* The cockpit mirror extension covers observed mirror targets/scissors:
  `0xc3500000`, `0xc1320000`, `0xc0660000`, and `0xc0cc0000`.

## Current Test Configuration

The local full-screen flicker seen during development was a configuration issue, not the mirror patch. Meaningful GT6 A/B tests should use the same settings that made the clean master build stable:

```text
Read Color Buffers: true
Write Color Buffers: true
Asynchronous Texture Streaming: true
Multithreaded RSX: true
Resolution Scale: 100
```

## First Public-Test Result

Initial local test result after porting the patch to the clean CI-near fresh master build:

```text
Default HUD mirror: fixed in first test
Cockpit mirrors: fixed in first test
Unrelated full-screen flicker: gone with the config above
```

Recommended public test coverage:

* multiple GT6 tracks, especially Grand Valley East;
* default 2D HUD view with rear-view mirror;
* cockpit center and side mirrors;
* outdoor/tunnel lighting transitions;
* cold boot and savestate resume, because savestates caused unrelated HUD/list artifacts during development.

Cross-checks:

* Gran Turismo 5 Prologue: rear-view mirror path appears different; GT6 mirror bugs not observed.
* Need for Speed Shift: rear-view mirror path appears different; GT6 mirror bugs not observed.

## Known Limits

The current implementation is intentionally narrow but still heuristic:

* It is not title-gated.
* It relies on observed GT6 RSX addresses, dimensions, and scissor patterns.
* It has only been tested locally on `BCES01893`.
* It should be treated as an experimental review/public testing build before any upstream PR discussion.
