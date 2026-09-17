# Project Zomboid Native Android Server (Termux)

Run a Project Zomboid dedicated server **fully native on Android** — no root,
no x86 translation, no emulation. ARM64 Java + rebuilt engine libraries,
hosting real players.

**Verified against:** Build 42.20.4, LineageOS (vanilla, no GApps), Termux, aarch64
**Last updated:** 2026-09-15

## ⚠️ YOU MUST OWN THE GAME

This guide does **not** provide, distribute, or link to any copy of Project
Zomboid or its server files. You must **purchase your own copy** from The Indie
Stone / Steam and use your own legally obtained dedicated-server files. No
piracy support here — buy the game, it's worth it.

## ⚠️ Known Limitation (read this first)

**300-zombie entity cap:** the server tracks a maximum of ~300 zombie
entities. Population above this budget = oldest zombies get deleted.
Increasing `ZombiesCountBeforeDelete` carries documented community-reported
risks of disk write errors and world-save corruption. This guide documents
the cap and the trade-offs, not a bypass. See
[TROUBLESHOOTING/zombie-despawn-cap.md](TROUBLESHOOTING/zombie-despawn-cap.md).

## What This Covers

- Step-by-step setup: preflight → Termux → JDK + natives → server → mods
- Every milestone has a **verification step (receipt)** — how to prove it worked
- The four Android wars: phantom killer, cached-app freezer, monitoring false
  positives, and checksum mismatches — with fixes
- Hardware guidance (including what ambushes you if you run stock Android)
- Mod pipeline: rsync → lowercase-normalize → validate → modpack seal
- Watchdog/daemonization patterns and marker doctrine
- Ghost lessons: operations that reported success while doing nothing

## What This Does NOT Cover

- Running the PZ *client* on Android (different challenge entirely)
- Root-based instrumentation
- Public server hosting (this setup is private-fleet/tailnet oriented)

## Time Investment (Honest Numbers)

| Phase | Following this guide | Trial & error (what it took me) |
|-------|---------------------|--------------------------------|
| Research & dead ends | 0 h | 40–80+ h |
| Termux + JDK + natives | ~2 h | 5–15 h |
| Android survival stack | ~1 h | 15–40 h |
| Server config & verification | ~1 h | 5–10 h |
| Mod pipeline + checksum war | ~2 h | 20–50 h |
| Watchdog + boot hardening | ~1 h | 10–20 h |

This guide represents well over 100 hours of debugging, four wars, and a
dozen "ghost operations" (things that lied about what they did). Every
receipt in here exists because something lied to us first.

## Quick Start

Start with [HARDWARE_REQUIREMENTS.md](HARDWARE_REQUIREMENTS.md), then
[SETUP/00-preflight-checks.md](SETUP/00-preflight-checks.md).

## License

- Code/templates: MIT
- Documentation: CC BY 4.0

*Not affiliated with The Indie Stone. Not responsible for corrupted worlds
or save loss — keep backups, and read the zombie-cap doc before touching
sandbox settings.*
