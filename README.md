# nonton

Watch YouTube in a real video player (mpv) instead of a browser tab — built for low-RAM machines.

A browser YouTube tab can eat **3–4 GB**. On an 8 GB Mac that means swap, heat, and crashes. `nonton` streams the same video through **mpv + yt-dlp** at roughly **~500 MB**, with optional cookie auth for throttle-free playback and locked content.

## Why

- **Light** — mpv uses a fraction of a browser's memory.
- **Quality** — 2K / 4K playback (free on YouTube; no Premium required).
- **Codec-aware** — prefers AV1 on Apple Silicon (M3+ has hardware AV1 decode), avoiding heavy software VP9 decode.
- **Cookie auth** — borrow cookies from your logged-in browser to dodge YouTube's bot throttling and reach age-restricted / members-only / Premium-bitrate streams. No OAuth, no password, no ban risk.

## Requirements

- [`mpv`](https://mpv.io) and [`yt-dlp`](https://github.com/yt-dlp/yt-dlp) — `brew install mpv yt-dlp`

## Usage

```bash
nonton <url|search query>     # play
nonton -q 4k <url>            # force quality: 4k | 2k | 1080 | 720
nonton --login               # set up cookie auth (one time)
```

## Status

Early WIP. Scaffolding in progress.

## License

MIT
