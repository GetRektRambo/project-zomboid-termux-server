# Step 1 — Termux Base Install (~15 min)

Install Termux from **F-Droid** (the Play Store build is outdated/broken —
this matters). Also install the **Termux:Boot** companion app from F-Droid
and open it once.

Then in Termux:

    termux-setup-storage
    pkg update && pkg upgrade -y
    pkg install -y openssh tmux git curl wget nano perl python \
        rsync jq tar openjdk-17
    mkdir -p ~/pzserver ~/Zomboid/mods ~/.termux/boot

**Wake-lock is mandatory.** Add to `~/.termux/boot/start-services`:

    #!/data/data/com.termux/files/usr/bin/bash
    termux-wake-lock
    # ... then any services you want at boot

Make it executable:

    chmod +x ~/.termux/boot/start-services

Boot scripts without the wake-lock = phone sleeps, everything dies.

**Verification receipts:**

    java -version          # must print an OpenJDK build
    tmux new -d -s test && tmux ls   # a session named 'test' must appear
    termux-wake-lock       # no error

If `java -version` fails or prints something unexpected, fix the JDK
install before anything else — everything downstream assumes it.
