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
## Getting the server files (the part nobody documents)

Every route below requires a legitimate purchase — pick whichever fits
your setup.

### Route 1: Steam on PC → rsync to phone (recommended)

Install the **Project Zomboid Dedicated Server** from Steam on your PC
(Library → filter dropdown → Tools). The full server tree lands in:

    steamapps/common/Project Zomboid Dedicated Server/

Then rsync it to the phone — same muscle memory as the mod pipeline:

    rsync -av --progress "/path/to/steamapps/common/Project Zomboid Dedicated Server/" \
        user@phone-ip:8022:~/pzserver/

(Termux sshd listens on 8022, not 22.) Keeps the ~7 GB download off
the phone and reuses your existing workshop-master workflow.

### Route 2: DepotDownloader inside Termux (one-machine option)

SteamCMD does NOT work on Android — the kernel lacks
`set_robust_list`, so it aborts with `futex robust_list not
initialized by pthreads` even under emulation. DepotDownloader is the
working alternative: log in with your Steam credentials, it pulls
the server depot (~7 GB) directly to the phone, Steam Guard prompts
on-device. Credit for discovering and documenting this route:
[nikiiiii-ii/zomboid-server-native-arm64](https://github.com/nikiiiii-ii/zomboid-server-native-arm64)
— also worth reading for its glibc-vs-bionic notes and the finding
that the official game depot ships ARM64 natives.

### Which route?

PC→rsync keeps heavy lifting off the phone and fits the mod pipeline;
DepotDownloader is the no-second-computer option. Both assume you own
the game — no route here distributes files.

### JDK version note (verify yours)

Newer PZ builds are compiled for newer Java (B42.20.3 reportedly
needs class file version 69 = Java 25; older builds ran on 17). If
the server dies at startup with `UnsupportedClassVersionError`, your
JDK is too old for your build — check `java -version` against the
build's requirements before assuming anything Android-related.
