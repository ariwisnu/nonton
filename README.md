# nonton — watch YouTube in your terminal, not a browser tab

![shell: bash](https://img.shields.io/badge/shell-bash-4EAA25?logo=gnubash&logoColor=white)
![platform: macOS / Apple Silicon](https://img.shields.io/badge/macOS-Apple%20Silicon-000000?logo=apple&logoColor=white)
![player: mpv + yt-dlp](https://img.shields.io/badge/player-mpv%20%2B%20yt--dlp-purple)
![license: MIT](https://img.shields.io/badge/license-MIT-blue)

A tiny command-line **YouTube player** that streams video through **mpv + yt-dlp** instead of a browser tab — built for **low-RAM machines**. Play by URL, search query, or a channel's live stream, with quality selection and optional cookie auth.

> A browser YouTube tab can eat **3–4 GB**. On an 8 GB Mac that means swap, heat, and crashes. `nonton` plays the same video at roughly **~500 MB**.

```bash
nonton "lofi hip hop"                                   # search, pick from a menu
nonton https://www.youtube.com/watch?v=aqz-KE-bpKQ      # play a URL
nonton -q 4k @WindahBasudara                            # a channel's live stream, in 4K
```

## Why nonton

- **Light on RAM** — mpv uses a fraction of a browser's memory. No tab, no renderer, no 4 GB.
- **Free 2K / 4K** — full-resolution playback. 2K/4K on YouTube is free; no Premium required.
- **Codec-aware** — prefers **AV1** on Apple Silicon (M3+ decodes AV1 in hardware), avoiding hot, heavy software VP9 decode.
- **Live-channel shortcuts** — `nonton windah` or `nonton @handle` jumps straight to a channel's livestream.
- **Cookie auth, no OAuth** — borrow cookies from your logged-in browser to dodge bot throttling and reach age-restricted / members-only / Premium-bitrate streams. No password, no token, no ban risk.

## Install

Needs [`mpv`](https://mpv.io) and [`yt-dlp`](https://github.com/yt-dlp/yt-dlp); a JS runtime ([`deno`](https://deno.com)) for yt-dlp's challenge solver; and optionally [`fzf`](https://github.com/junegunn/fzf) for the search picker.

```bash
brew install mpv yt-dlp deno fzf

git clone https://github.com/ariwisnu/nonton.git
ln -s "$PWD/nonton/nonton" /opt/homebrew/bin/nonton   # or any dir on your PATH
```

Without `deno` some videos fail to resolve. Without `fzf` the top search result plays instead of showing a picker.

## Usage

```bash
nonton <url|search query>      # play
nonton <name|@handle>          # play that channel's live stream
nonton -q 4k <url>             # quality: 4k | 2k | 1080 | 720
nonton --login [browser]       # set up cookie auth (default: chrome)
nonton --no-cookies <url>      # play once without cookies
nonton --help
```

**Search** — `nonton retro synthwave` opens an [fzf](https://github.com/junegunn/fzf) picker of the top results; pick one and it plays. No fzf? The top hit plays.

**Live channels** — a bare name or `@handle` resolves to that channel's `/live`. Register short names by adding a case to `channel_handle()` in the script:

```bash
windah) printf '@WindahBasudara' ;;
```

## Cookie auth

`nonton --login` borrows cookies from your logged-in browser via `yt-dlp --cookies-from-browser`. Nothing is written to disk — cookies are read live from the browser's own encrypted store at play time. On macOS the first run triggers a Keychain prompt for the browser's safe-storage key; choose **Always Allow**.

> **Chrome v127+** encrypts cookies behind a Keychain key. If the first `--login` seems to hang, an approval dialog is waiting on screen — click **Always Allow**. If Chrome cookies ever break after an update, fall back to another browser: `nonton --login firefox`.

What login actually buys you: not resolution (2K/4K is free), but **anti-throttle** playback plus access to **age-restricted**, **members-only**, and **Premium-bitrate** streams.

## Configuration

Settings live in `~/.config/nonton/config` (written by `--login`):

```bash
COOKIE_BROWSER=chrome   # chrome | firefox | brave | edge | ...  (empty = no cookies)
QUALITY=1080            # default quality keyword: 4k | 2k | 1080 | 720
```

## How it works

`nonton` resolves your input to a URL (search → top/fzf result, name/`@handle` → channel live, URL → as-is), builds an AV1-preferred yt-dlp format string capped at the chosen height, then hands it to mpv with `--cookies-from-browser` when a browser is configured. One self-contained Bash script, no install step beyond a symlink.

## License

MIT — see [LICENSE](LICENSE).

---

<sub>Keywords: terminal YouTube player, command-line YouTube, watch YouTube in terminal, mpv YouTube, yt-dlp player, low-RAM YouTube, macOS Apple Silicon M3, AV1 hardware decode, YouTube CLI, lightweight video player.</sub>
