# Hardware Requirements

## Minimum Viable vs Recommended

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| RAM | 6 GB | 8 GB+ |
| Free storage | 16 GB | 32 GB+ |
| CPU | aarch64, 8-core | SD 778G-class or better |
| Storage type | UFS 2.2 | UFS 3.1+ |

- **RAM:** a 2-3 GB JVM heap + Android baseline + system services is about
  5 GB committed. Below 6 GB total you will fight the OS constantly.
- **UFS matters:** world saves need sustained random writes. eMMC tanks it.
- **Custom ROM is a bonus, not a requirement** — see below.

## Is LineageOS Required?

**No — but stock Android ships its own ambushes:**

### 1. OEM process killers are more aggressive than AOSP
Samsung/One UI, Xiaomi MIUI/HyperOS, Oppo and others layer proprietary
battery optimizers that ignore the standard workarounds and kill
long-running apps on their own schedule. Symptom: JVM dies every few
minutes even after the phantom-war fix, with no logcat evidence.
**Workaround:** find the OEM-specific exemption (Samsung: "Never sleeping
apps"; MIUI: autostart + lock-in-recents + no battery saver). It exists,
but every OEM hides it somewhere different.

### 2. GApps/bloat RAM tax
Stock ROMs ship 300-400+ packages. Expect 300-600 MB of background RAM
gone to services a headless server will never use. The pm-based debloat
in this guide works on stock too, but vendor "protected" packages refuse
uninstall — fall back to `pm disable-user --user 0`.

### 3. OTA updates reset your workarounds
The phantom-prop, freezer whitelist, and battery exemptions can all be
reset by an eager OTA. Re-verify the full survival stack after EVERY
system update.

### 4. No clean-slate path
On LineageOS you can reflash vanilla without GApps. On locked stock
devices the ceiling is per-user uninstall, permanently.

**Bottom line:** stock works if you win the OEM-killer lottery for YOUR
device + ROM version. Test with the 15-minute soak BEFORE investing hours.

## Pre-setup Benchmarks

Soak test (phantom/freezer survival) — before anything else:

    termux-wake-lock
    sleep 900 && echo "SOAK PASSED" || echo "SOAK FAILED"

Sustained write test (world-save proxy):

    dd if=/dev/zero of=~/tmp/testfile bs=1M count=500; rm ~/tmp/testfile
