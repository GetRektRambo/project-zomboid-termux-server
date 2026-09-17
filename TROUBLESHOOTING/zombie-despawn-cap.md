# Zombie Despawn — the ~300-Entity Tracking Cap

## Reality

The server tracks a maximum of ~300 zombie entities
(`ZombiesCountBeforeDelete`). Population above the budget = oldest
zombies deleted. High population multipliers accelerate it. This is a
tracking budget, not a memory problem — more RAM/heap does NOT fix it,
and neither does anything in the Android survival stack.

## The risk of raising it

Community-documented: raising `ZombiesCountBeforeDelete` carries real
risk of disk write errors and world-save corruption. I deliberately
left it at default — losing a world is worse than losing zombies.

## Options, ranked conservative-first

1. **Accept it.** Stable choice. Hordes still work; population pressure
   still presents through migration.
2. **Incremental bump** (e.g. 300 → 350), monitor save stability for
   48 h before touching it again — keep world backups religiously.
3. **Slow the drain instead:** raise `RedistributeHours` (weekly zombie
   migration) — spawn-throttle levers, not delete-budget levers.

## Diagnostic that settles it

Lure 60–80 zombies somewhere enclosed, watch 15–20 min with a player
nearby. Rapid bleed = the cap (or a mod); slow drift over days =
migration settings.

This guide documents the cap and the trade-offs, not a bypass. If a
higher cap is worth world-corruption risk to you, that's your call —
go in with backups and eyes open.
