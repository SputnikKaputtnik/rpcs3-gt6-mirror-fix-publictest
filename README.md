# RPCS3 GT6 Mirror Fix - Experimental Public Test Build

This repository publishes an **unofficial experimental Windows test build** for two Gran Turismo 6 mirror issues in RPCS3.

It is **not an official RPCS3 build**, not an upstream-ready pull request, and not intended for general RPCS3 support requests. The RPCS3 wiki support guidance says support should be based on the latest official build, so please treat this only as a focused public test artifact.

## Target

Tested locally:

```text
Game: Gran Turismo 6
Title ID: BCES01893
Game version: 1.05
Base RPCS3 commit: b533a560e6df6f8aa1c271bfffbd1e39f500bd65
Embedded version before patch: 19374-b533a560
```

## Intended Fixes

- Default 2D HUD rear-view mirror showing wrong content such as minimap-like data.
- Car/material flicker in the default HUD mirror and cockpit mirrors.

Terminology note: this is not GT6's bonnet view. GT6 bonnet view has no rear-view mirror in this test case.

## Recommended GT6 Test Config

```text
Read Color Buffers: true
Write Color Buffers: true
Asynchronous Texture Streaming: true
Multithreaded RSX: true
Resolution Scale: 100
```

During local testing, unrelated full-screen flicker was traced to configuration and disappeared with the settings above.

## Files

- `gt6-mirror-fix-b533a560e.patch` - source diff against the base commit.
- `gt6_mirror_public_testing.md` - test notes, known limits, and cross-checks.
- `SHA256SUMS.txt` - hashes for the release payload.
- `LICENSE-RPCS3-GPLv2.txt` - RPCS3 license text copied from the base source tree.

The executable and ZIP package are attached to the GitHub release, not committed to this repository.

## Known Limits

- Tested locally only on `BCES01893` so far.
- Not title-gated.
- Relies on observed GT6 RSX addresses, dimensions, and scissor patterns.
- Should be treated as an experimental workaround for public testing and review, not as a merge proposal.

## Cross-Checks

- Gran Turismo 5 Prologue: rear-view mirror path appears different; GT6 mirror bugs not observed.
- Need for Speed Shift: rear-view mirror path appears different; GT6 mirror bugs not observed.

## Source Availability

This package includes the patch required to reproduce the build from RPCS3 commit `b533a560e6df6f8aa1c271bfffbd1e39f500bd65`.

Upstream RPCS3 source is available at:

```text
https://github.com/RPCS3/rpcs3
```

## AI Transparency

This experiment was developed collaboratively by the tester and OpenAI Codex/ChatGPT. See `AI_TRANSPARENCY.md`.
