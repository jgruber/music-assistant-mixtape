# Mixtape — Spotify catalog, YouTube audio for Music Assistant

A [Music Assistant](https://music-assistant.io) custom music provider that
reads Spotify's public web catalog (`open.spotify.com`) for search and
playlist metadata and plays the underlying audio through YouTube. No Spotify
account required, no Spotify audio is played.

Think of it as a complement to the official `spotify` provider, intended for
people who don't have a Spotify account. It offers none of the Premium-tier
features (library sync, personalized recommendations, native Spotify audio,
Spotify Connect, official support). If you have Premium, keep the official
provider for playback and use Mixtape only to browse.

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

## Domain

This provider's domain is `mixtape`, so it appears alongside — not in place
of — the official in-tree `spotify` provider. If you have Spotify Premium
you want to keep using in Music Assistant, keep the official provider for
playback; Mixtape only reads the public web catalog, with YouTube supplying
the audio.

## Install

Install through the [Custom Provider Installer](https://github.com/jgruber/music-assistant-provider-installer)
plugin on a Music Assistant instance running the
[`jgruber/music-assistant-server`](https://github.com/jgruber/music-assistant-server)
fork (which carries the custom-providers loader): add

```
https://github.com/jgruber/music-assistant-mixtape
```

to the installer plugin's **Repositories** list, save, and restart Music
Assistant. The plugin fetches this repository on each container start, drops
its `mixtape/` directory into `<storage_path>/custom_providers/mixtape/`,
and `uv pip install`s the requirements below into the Music Assistant venv.

## Dependencies

```
spotapi>=1.0
ytmusicapi==1.12.3
yt-dlp>=2025.09.26
bgutil-ytdlp-pot-provider==2.0.1
deno==2.9.6
redis
websockets
pymongo
```

Mixtape reuses Boombox's `yt-dlp` playback stack. If both are installed, they
share one copy of each dependency.

## Config

Mixtape has no mandatory configuration. Its config entries, if any, live
under `provider.mixtape.*` in Music Assistant's settings page.

## Safety

A custom provider runs with Music Assistant's full privileges — this one too.
The server does not sandbox it. Only install custom providers from sources
you trust.

## License

Apache License 2.0 — see `LICENSE`.
