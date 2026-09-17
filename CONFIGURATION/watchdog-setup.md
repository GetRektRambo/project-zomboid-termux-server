# Watchdog & Marker Doctrine

The pattern: a watchdog loop checks the server process every ~5 minutes
and restarts it on death — but ONLY honoring marker files, so it never
fights your deliberate decisions.

## Markers

- `pz_manual_stop` — you stopped the server on purpose; watchdog stands
  down until you clear it
- `pz_autostart` — intent marker; its presence means resurrect on boot

## Manual-start-only doctrine (recommended)

Do NOT leave `pz_autostart` set. Reboots then boot PZ-free, and the
server runs only when you choose. Some live-console start flows set
the marker for you — remove it after starting:

    rm -f ~/hacking/run/pz_autostart   # running server unaffected; boot stays clean

## Monitoring honesty (the dog-war lesson)

A monitor that never passes on a healthy system converts uptime into
scheduled downtime. Our RakNet probe went deaf on B42 while clients
played fine — every fail-limit strike restarted a POPULATED server,
manufacturing the very disconnect pattern I was hunting.

**Resolution:** the probe runs WARN-ONLY. **Process death is the sole
restart trigger.** Validate probes against known-good state after every
rebuild, not just when written — a probe that was correct last month
can be silently wrong after a server update.
