# Step 0 — Preflight Checks

Before touching anything:

1. **Back up the phone.** Anything you care about, somewhere else.

2. **Confirm aarch64:**

        getprop ro.product.cpu.abi

   Must print `arm64-v8a`.

3. **Check storage:**

        df -h /data

   Need 16 GB+ free.

4. **Enable Developer Options** (tap Build Number 7x), then:
   - USB debugging ON
   - Wireless debugging ON (note: the port rotates per reboot)
   - Stay awake / keep device on while charging (recommended)

5. **The 15-minute soak — this decides everything:**

        termux-wake-lock
        sleep 900 && echo "SOAK PASSED"

   If the process is dead after 15 minutes untouched, you MUST fix the
   survival stack (see TROUBLESHOOTING/) before the server will ever
   stay up. Do not skip this and "debug later" — every silent-kill war
   in this guide traces back to skipping exactly this test.

**Receipt to keep:** paste of all the outputs above, timestamped.
You are building a paper trail from step zero — get used to it now.
