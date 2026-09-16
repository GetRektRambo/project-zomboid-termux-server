# The Launcher (single-door doctrine)

**ONE launcher script starts the server.** No alternates, no copies — a
stale launcher is a rogue instance waiting for its moment: two servers
fighting over ports, half-written saves, mystery crashes nobody can
reproduce. See SCRIPTS/pz-launch.sh.template for the pattern.

## Core properties

- Heap via one env-overridable variable: `PZ_HEAP="${PZ_HEAP:-3072m}"`
- G1GC hardcoded (ZGC banned — see SETUP/02)
- Natives dir + flags set explicitly
- Runs the server inside a tmux session so it survives terminal death
  and you can attach to the console later

## Verification receipt after ANY launcher edit

    bash -n launcher.sh                  # syntax

    grep -n 'PZ_HEAP' launcher.sh        # the variable dodge check

    pgrep -f zomboid | head -1 | xargs -I{} cat /proc/{}/cmdline \
      | tr '\0' ' ' | grep -o 'Xmx[0-9]*m'   # the running-JVM truth gate

Three receipts, not one. The syntax check catches typos, the grep
catches the variable dodge, and the cmdline readout catches everything
the first two miss. Skipping the third because the first two passed has
cost more debugging time than any single bug in this whole project.
