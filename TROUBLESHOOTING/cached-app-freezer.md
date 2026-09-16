# Cached-App Freezer & Doze (War 2)

**Symptom:** server fine while you watch, dead or frozen minutes after
you leave. Port still bound, but nothing answers. Classic freezer
behavior — the process is technically alive and useless at once.

The phantom fix alone is NOT sufficient. Android has multiple silent
killers: cached-app freezer, doze, battery restrictions. The full
survival stack:

1. `device_config` phantom cap (previous doc)
2. `adb shell dumpsys deviceidle whitelist +com.termux`
3. `adb shell cmd appops set com.termux RUN_ANY_IN_BACKGROUND allow`
4. Settings → Apps → Termux → Battery → **Unrestricted**
5. `termux-wake-lock` in your boot script

## Discriminator for a frozen-but-alive process

    ps -o stat,time -p <pid>; sleep 20; ps -o stat,time -p <pid>

TIME advancing = alive. TIME stopped = frozen in place.

Live unfreeze:

    adb shell am set-standby-bucket com.termux ACTIVE

## Certification

The 15-minute untouched soak, then progressively longer soaks. No
drops = freezer-proof. A server that survives only while attended is
not a server, it's a demo.
