# Project Zomboid Native Android Server (Termux)

Run a Project Zomboid dedicated server **fully native on Android** — no root,
no x86 translation, no emulation. ARM64 Java + rebuilt engine libraries,
hosting real players.

**Verified against:** Build 42.20.4, LineageOS (vanilla, no GApps), Termux, aarch64
**Last updated:** 2026-09-15

## ⚠ YOU MUST OWN THE GAME

This guide does **not** provide, distribute, or link to any copy of Project
Zomboid or its server files. You must **purchase your own copy** from The Indie
Stone / Steam and use your own legally obtained dedicated-server files. No
piracy support here — buy the game, it's worth it.

## ⚠ Known Limitations (read this first)

Honest inventory of what this setup is not. If any of these are dealbreakers,
this guide will waste your time — and that's fine, not everything needs to be
for everyone.

1. **The ~300-zombie entity cap.** The server tracks a maximum of ~300 zombie
   entities. Population above this budget = oldest zombies deleted.
   Increasing `ZombiesCountBeforeDelete` carries documented community-reported
   risks of disk write errors and world-save corruption. This is a tracking
   budget, not a memory problem. See
   [TROUBLESHOOTING/zombie-despawn-cap.md](TROUBLESHOOTING/zombie-despawn-cap.md).

2. **The Android survival stack is perishable.** The phantom-killer fix,
   freezer whitelist, and battery exemptions can all be reset by a system
   OTA update. Every Android update means re-running the survival-stack
   receipts or the server silently dies again. Treat updates as
   re-verification exercises, not routine maintenance.

3. **The hardware ceiling is real.** 6 GB RAM minimum, 8 GB recommended,
   heap above 3072m is unverified territory. More players, more mods, a
   bigger world — the phone cannot grow. This is a fleet server for a
   handful of friends, not a community box.

4. **Manual-start doctrine.** By design, the server does not auto-resurrect
   on reboot unless you deliberately opt in. Great for a shared device,
   confusing for friends who expect 24/7 uptime. "Up yesterday, down today"
   means ask the owner, not assume it's broken
   (see SETUP/05-friends-joining-your-server.md).

5. **Single device, single point of failure.** No root means no proper
   service manager or daemon supervision — the watchdog is a tmux loop
   whose only restart trigger is process death. Sophisticated failure
   modes (freezer, RakNet wedge) are detected only when players complain.

6. **Build-locked verification.** Everything here is verified against
   42.20.4. Game updates restore the x86 natives silently (see SETUP/03)
   and shift JDK requirements between builds. An update is not passive
   for this server — it's a re-verification exercise.

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
receipt in here exists because something lied to me first.

## Quick Start

Start with [HARDWARE_REQUIREMENTS.md](HARDWARE_REQUIREMENTS.md), then
[SETUP/00-preflight-checks.md](SETUP/00-preflight-checks.md).

## License

- Code/templates: MIT
- Documentation: CC BY 4.0

*Not affiliated with The Indie Stone. Not responsible for corrupted worlds
or save loss — keep backups, and read the limitations above before touching
sandbox settings.*
