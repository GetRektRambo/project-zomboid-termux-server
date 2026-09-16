# Step 5 — Getting Friends Onto Your Private Server

A server hosting one player is a diary. This is the actual onboarding
procedure we hand to new players — replace the placeholders with your
own values before sending it.

## Why a tailnet (Tailscale)

- No router changes, no port forwarding, no dynamic-DNS dances
- The server never exposes a port to the public internet
- Works across NATs, CGNAT, and mobile data — friends connect from
  their house, their uni, wherever
- Free tier covers a private fleet easily

## Server side (the phone)

Install Tailscale from F-Droid or run the static binary in Termux in
a tmux session, then:

    tailscale up

The hostname it reports is the address friends can rely on — tailnet
IPs can change when a device rejoins, the name doesn't.

## Player onboarding (hand them this)

1. **Mods** — send the modpack link (`pz-modpack-b42.tar.gz` plus its
   `.sha256` sidecar). Extract AT THE HOME DIRECTORY, not inside
   `Zomboid/mods` — the archive builds its own path; extracting in
   the wrong place is the classic nesting footgun. The game only
   reads mods at LAUNCH, so if the game is open, restart it after
   copying files.
2. **Launch options (once)** — Steam → right-click Project Zomboid →
   Properties → Launch Options: `-nosteam`. AND in the game's server
   browser settings, UNTICK "Use Steam Relay".
3. **Connect** — Join Server / Direct Connect:
   `<your-tailnet-ip-or-hostname>` port `16261`. Username is
   whatever they like (display name only).
4. **Steam Deck friends (optional Gaming Mode)** — install the
   Tailscale plugin from the Decky Plugin Store and switch it on
   when they want to connect.

## Honesty section (put this in THEIR hands too)

- The server is manual-start by doctrine — it does not come back
  after a reboot until the owner chooses. "Up yesterday, down today"
  means ask the owner to start it, not that it's broken.
- With `DoLuaChecksum=false`, mismatched mods still connect with
  silent desync possible — the modpack tarball is the parity
  guarantee, not the ini.
- A failed join can wedge the RakNet listener — if nobody can
  connect after one failed attempt, the server needs a restart
  (see TROUBLESHOOTING/raknet-wedge.md).

## Receipt

Friend connects, plays 20 minutes, logs show continuous session, no
checksum kicks, no wedges. That's the whole test.
