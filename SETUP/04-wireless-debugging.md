# Step 4 — Wireless Debugging (the adb layer)

You need adb-level access (uid 2000 shell) for the Android survival
stack — the phantom fix, freezer whitelist, and standby-bucket rescue
all run through adb. Without it you are blind to the wars.

## Without root, the port ROTATES

`persist.adb.tcp.port` cannot be set at uid 2000 — expect the
wireless-debugging port to change on every reboot. The recovery ritual:

    adb mdns services        # discover the current port for adbd
    adb connect <phone-ip>:<port>
    adb devices             # truth gate: state must be 'device'

## Lessons baked in blood

- `adb connect` reports success **optimistically** for dead ports —
  only `adb devices` showing state `device` is truth
- Setting adb props restarts adbd mid-command — transient, the prop
  usually sticks; just reconnect
- If loopback `127.0.0.1:5555` is refused, go straight to mDNS
  discovery — the legacy port is dead on modern Android

## What uid-2000 adb shell CAN do

settings, device_config, dumpsys, logcat, pm, am — everything the
survival stack needs.

## What it CANNOT do

Raw sockets, tcpdump, dmesg, root-only props, other apps' /proc. If a
guide tells you to "just setprop persist...", it assumed root you
don't have.

## Bridging tip

Scripts that hardcode the adb port break every boot. Source the port
from mDNS discovery at run time instead.
