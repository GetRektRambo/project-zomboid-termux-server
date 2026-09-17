# Mod Troubleshooting (the checksum war, abridged)

**Symptom:** clients force-disconnected with

    checksum-File doesn't exist on the client: <path>

## What I eliminated first (with receipts)

Tree md5 identical both sides (LC_ALL=C sort, or the diff lies), no
case mismatches, file exists at the demanded path, fresh db, fresh
boot. Every check passed; the kick kept coming. Days of elimination.

## THE FIX — two different flags, one misleading error

`DoLuaChecksum=false` governs the **file-existence** check.
`AntiCheatChecksum` governs **content-mismatch** kicks. **Two different
flags — the error message never names which one fired.** Fixing the
wrong one changes nothing and costs you a weekend.

Private fleet: `DoLuaChecksum=false` (mismatched mods still connect,
silent desync possible → modpack tarball stays mandatory).
Public someday: it goes back to `true`.

## Other classics

- A failed join can WEDGE the RakNet listener — it accepts nothing
  afterward while looking alive. Probe, restart on fail, never retry a
  wedged server.
- The modpack tarball extracts at `~`, NOT inside the mods dir —
  players extract into `~/Zomboid/mods` themselves or you get nesting
  hell.
