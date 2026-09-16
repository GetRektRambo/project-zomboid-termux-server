# The Phantom Process Killer (War 1)

**Symptom:** the JVM dies every ~2 minutes. No OOM logs. Nothing in
Termux tells you why. Just dead, silently.

Android's phantom-process monitor kills apps that spawn subprocesses
heavily — a classic Java server pattern. It leaves no standard crash
trail.

## The fix (no root)

On many builds you can neuter it at uid 2000:

    adb shell device_config put activity_manager \
      max_phantom_processes 2147483647
    adb shell device_config set_sync_disabled_for_tests persistent

## OS updates reset this

**Verify after every system update.** A boot script (Termux:Boot)
re-asserting this every boot is the robust pattern.

## Receipt

Start the server, walk away 10+ minutes, come back:

    pgrep -f zomboid && echo "SURVIVED"

If it's dead with no logs, adb logcat is the ONLY instrumentation
that names Android's killers. Check it before assuming a JVM problem.
