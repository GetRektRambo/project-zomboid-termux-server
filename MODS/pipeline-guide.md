# Mod Pipeline

## Local-tree doctrine

Clients load mods from their own `~/Zomboid/mods`, NOT Steam Workshop —
subscriptions are irrelevant to loading. Your tailnet is the
distribution channel.

## The pipeline

Update mods on a PC with Steam (workshop master) → rsync to the phone →
stage → lowercase-normalize → regenerate the `Mods=` line from
`mod.info` files → restart the server.

## Gotchas that WILL bite you

- Workshop ships **MixedCase** names; Android fs is case-sensitive →
  lowercase-normalize on ingestion, or checksum walks break mysteriously
- B42 mods may carry a legacy top-level `mod.info` with a different
  `id=` beside the version folder's — the game honors the version-folder
  one; rename the stub to `mod.info.legacy`
- Mod ids may carry CRLF/padding → sanitize with `tr -d ' \t\r'`
- **Client mod scan happens at game LAUNCH** — mods copied while the
  client runs are invisible until a full restart
- Folder name ≠ mod id (a folder `CarriableItems` can hold id `CVI`)

## Distribution to players

Pack the client mods into a tarball + ONE-LINE sha256 sidecar, serve
over simple HTTP on your LAN/tailnet:

    sha256sum modpack.tar > modpack.tar.sha256

The `.sha256` beside the tarball is always the live truth — players
verify with `sha256sum -c modpack.tar.sha256` before extracting.
