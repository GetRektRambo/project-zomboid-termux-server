# Client vs Server Mods

- **Server mods**: needed by the server to simulate world state. Listed
  in `Mods=` in servertest.ini.
- **Client mods**: render/UI/audio — each player needs them locally.
  Client FPS/render mods (e.g. render-distance optimizers) do nothing
  on a headless server; they can be removed from the server's `Mods=`
  line while players keep them locally with zero loss.
- If a mod is both, both sides need it.
- Mismatch under `DoLuaChecksum=false` still connects — with **silent
  desync possible** — the modpack tarball is your parity guarantee,
  not the ini.

## Why the distinction matters

Removing client-only mods from the server's `Mods=` line shrinks the
checksum surface, shortens the load chain, and removes failure modes
that look like server bugs but are really UI mods failing on a
headless box. Classify every mod before adding it — "does the server
need to simulate this?" is the whole test.
