# Mixtape — Spotify catalog, YouTube audio for Music Assistant

A [Music Assistant](https://music-assistant.io) custom music provider that
reads Spotify's public web catalog (`open.spotify.com`) for search and
playlist metadata and plays the underlying audio through YouTube. No Spotify
account required, no Spotify audio is played.

Think of it as a complement to the official `spotify` provider, intended for
people who don't have a Spotify account. It offers none of the Premium-tier
features (library sync, personalized recommendations, native Spotify audio,
Spotify Connect, official support). If you have Premium, use the official
provider.

## What it is

- Anonymous access to the catalog endpoints that render `open.spotify.com` to
  a logged-out visitor, via [`spotapi`](https://pypi.org/project/spotapi/).
- Playback resolved through YouTube using the same `yt-dlp` pipeline as
  [Boombox](https://github.com/jgruber/music-assistant-boombox).
- Lets you paste a public Spotify playlist URL into the Music Assistant
  search box and have the playlist resolve to a playable queue.

## What it is not

- Not retrieving or playing any audio from Spotify.
- Not requiring, using or substituting for a Spotify account.
- Not shipped, listed, linked to or endorsed by Music Assistant upstream.
- Not a Spotify-branded product; "Mixtape" is the display name.

## Domain and shadowing

This provider claims the `spotify` domain so that pasted `open.spotify.com`
URLs are routed to it by Music Assistant's shareable-URL handling. **It
shadows the in-tree `spotify` provider.** If you have a Spotify Premium
account you want to keep using in Music Assistant, do not install this —
use the official provider instead.

## Install

### A. As a Music Assistant custom provider (requires upstream support)

Once Music Assistant core ships the `custom_providers/` loader (currently
proposed in [`feat/custom-providers`](https://github.com/jgruber/server/tree/feat/custom-providers)),
drop the `spotify/` directory from this repo into:

```
<storage_path>/custom_providers/spotify/
```

Install the Python dependencies into Music Assistant's venv (see
**Dependencies** below), then restart Music Assistant. Music Assistant will
emit a `WARNING` at startup noting that the in-tree `spotify` provider is
being shadowed by a custom provider — that is intentional.

### B. As a Home Assistant add-on (current working path)

Use the [Boombox + Mixtape add-on repo](https://github.com/jgruber/music-assistant-boombox-mixtape-sources) —
it bakes both providers plus all dependencies into the Music Assistant image.
This is the recommended install today; it will remain supported after the
custom-provider loader lands.

## Dependencies

Pin versions; unpinned requirements are ignored by Music Assistant's loader.

```
spotapi
redis
websockets
pymongo
```

Mixtape reuses Boombox's `yt-dlp` playback stack. If both are installed, they
share one copy of each dependency; if only Mixtape is installed, bring along:

```
yt-dlp>=2025.09.26
bgutil-ytdlp-pot-provider==2.0.1
deno==2.9.5
```

Also required alongside Music Assistant on the same host, for playback:

- A **PO Token provider sidecar** — the `ytm_po_token_generator` add-on from
  the Music Assistant repo, running on host network.

## Config

The provider has no mandatory configuration. The config entries live under
`provider.spotify.*` in Music Assistant's settings page.

## Safety

A custom provider runs with Music Assistant's full privileges — this one too.
The server does not sandbox it. Only install custom providers from sources
you trust.

## License

Apache License 2.0 — see `LICENSE`.
