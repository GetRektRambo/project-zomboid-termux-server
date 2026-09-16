# Canonical servertest.ini

    Port=16261
    Public=false
    MaxPlayers=10
    Mods=StarlitLibrary;<your-mod-ids-here>
    WorkshopItems=
    DoLuaChecksum=false
    AntiCheatChecksum=false
    SaveWorldEveryMinutes=30

## Setting rationale

| Setting | Value | Why |
|---------|-------|-----|
| `Mods=` | your ids, dependency-first order | order matters for loading |
| `WorkshopItems=` | EMPTY by design | local tree loads, not Steam Workshop |
| `DoLuaChecksum=false` | private-fleet setting | see checksum war doc before going public |
| `AntiCheatChecksum=false` | private-fleet setting | content-mismatch kicks |
| `SaveWorldEveryMinutes=30` | sane write cadence | don't crank it |

## Public-server day

When you go public, `DoLuaChecksum=true` goes back in — decide then,
not before. Private fleets tolerate the mismatch to keep friends
playing through mod-sync mistakes; public servers must not.

## Receipt

Count your Mods= entries and match against your mod folder count:

    grep '^Mods=' ~/Zomboid/Server/servertest.ini
    ls ~/Zomboid/mods | wc -l

If the counts disagree, a mod on disk is missing from the ini (or vice
versa) and joins will misbehave before they connect anyone.
