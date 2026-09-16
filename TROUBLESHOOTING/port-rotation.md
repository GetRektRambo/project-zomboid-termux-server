# Wireless-Debug Port Rotation

Without root, `persist.adb.tcp.port` cannot be set — the
wireless-debugging port rotates every reboot. Plan for it; don't fight
it.

## Recovery ritual

    adb mdns services                       # discover current port
    adb connect <phone-ip>:<discovered-port>
    adb devices                             # state 'device' = truth

## Lessons

- `adb connect` lies optimistically about dead ports — trust only
  `adb devices` state
- Setting adb props restarts adbd mid-command — transient, the prop
  usually sticks
- Bridging scripts that hardcode the port break every boot — source
  the port from mDNS discovery at run time instead

Fighting the rotation with hardcoded ports is a recurring ghost: every
reboot your tooling "fails" and you waste ten minutes re-proving the
bug you already diagnosed. Discover, don't hardcode.
