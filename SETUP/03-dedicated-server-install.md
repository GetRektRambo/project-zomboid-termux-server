# Step 3 — Dedicated Server Install + Natives

The stock PZ dedicated server ships **x86 natives**. They cannot run on
ARM64 Android — you must replace them with the rebuilt aarch64 engine
libs + FMOD pair from the zomdroid community (see ACKNOWLEDGMENTS).

**YOU MUST OWN THE GAME.** This guide does not distribute any Project
Zomboid files. Obtain the dedicated server files through your own
legitimate purchase/installation.

## Steps

1. Install the dedicated server to `~/pzserver/` using your own
   legitimately obtained server files, following the standard PZ
   dedicated-server procedure for your build.

2. Obtain the aarch64 natives pack from the zomdroid-lwjgl community
   releases (the `android-arm64-v8a` natives).

3. Place them in `~/pzserver/natives-arm64-test/`.

## Verification receipts

    ls ~/pzserver/natives-arm64-test/*.so | wc -l
    # expect ~26 libs including the lwjgl pair

    file ~/pzserver/natives-arm64-test/*.so | grep -c ARM
    # must be aarch64 ELF — if any file reports x86, the patch failed

## CRITICAL — game updates restore x86 natives

After ANY server update, re-check the natives dir and re-patch before
restarting. The updater does not know or care about your ARM libs; it
will silently restore the x86 originals and the server will fail to
boot. Make the natives check part of your post-update ritual.

## Additional launch flags (in the launcher)

    -Dzomboid.steam=0
    -Djava.net.preferIPv4Stack=true

## CLIENT SIDE

Every player needs `-nosteam` in their launch options and "Use Steam
Relay" unticked, or joins will fail. This applies to every client, not
just the host.
