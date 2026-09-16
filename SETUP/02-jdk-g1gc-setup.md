# Step 2 — JVM Tuning: G1GC, and Why ZGC Is Banned

**ZGC IS BANNED.** On Android it dies with `Unknown signal 31` — that's
SIGSYS, delivered by Android's seccomp filter because ZGC needs signals
the Android sandbox forbids. **Do not retry it.** G1GC is the settled
answer, verified across months of uptime.

## Heap sizing on an 8 GB phone

- 2048m: known-good, conservative
- 3072m: verified working (2.5 GB+ still available with server live,
  swap lazy)
- Gate: if swap climbs past ~500 MB under load, drop back a tier

Configure via one variable at the top of your launcher
(see CONFIGURATION/launcher-script.md):

    PZ_HEAP="${PZ_HEAP:-3072m}"
    # ...
    -Xms"$PZ_HEAP" -Xmx"$PZ_HEAP" -XX:+UseG1GC

## Warning — the variable dodge

If you `sed` your launcher for a literal string like `-Xmx2048m` and the
launcher builds its flags from a variable, your edit silently matches
nothing. The launcher reports "edited", syntax checks pass, the JVM runs
at the OLD heap. Always receipt the edit:

    grep -n 'PZ_HEAP' your-launcher.sh

## Final truth gate — ask the running JVM itself

    pgrep -f zomboid | head -1 | xargs -I{} cat /proc/{}/cmdline \
      | tr '\0' ' ' | grep -o 'Xmx[0-9]*m'

This reads the actual command line of the live process. If it doesn't
print your expected heap, the edit never took — no matter what the
script or your editor told you.
