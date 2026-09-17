# Changelog

## v1.1.2 — 2026-09-16
- README: expanded Known Limitations from one item to six
  - perishable Android survival stack (OTA resets)
  - hardware ceiling (RAM/heap) as fleet-server constraint
  - manual-start doctrine as friend-facing behavior
  - single point of failure (no root, watchdog limits)
  - build-locked verification (42.20.4, updater restores x86 natives)

## v1.1.1 — 2026-09-16
- SETUP/05: added "Friends joining your server" — Tailscale tailnet
  route, client modpack delivery, RakNet-wedge reminder on failed joins

## v1.1.0 — 2026-09-16
- SETUP/03: added "Getting the server files" section
  - Steam PC install + rsync route (fits the mod pipeline muscle memory)
  - DepotDownloader in Termux, with credit to nikiiiii-ii/zomboid-server-native-arm64
  - JDK / class-version check note for newer builds

## v1.0.0 — 2026-09-15
- Initial release
- Full setup path: preflight → Termux → JDK/natives → server → mods
- Hardware guidance incl. stock-Android ambush list
- Four Android wars documented with fixes and receipts
- Honest limitation: 300-zombie tracking cap
- Open: horde-churn field-test results (issue #1)
