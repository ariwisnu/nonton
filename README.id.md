# nonton — nonton YouTube di terminal, bukan di tab browser

[English](README.md) · **Bahasa Indonesia**

![shell: bash](https://img.shields.io/badge/shell-bash-4EAA25?logo=gnubash&logoColor=white)
![platform: macOS / Apple Silicon](https://img.shields.io/badge/macOS-Apple%20Silicon-000000?logo=apple&logoColor=white)
![player: mpv + yt-dlp](https://img.shields.io/badge/player-mpv%20%2B%20yt--dlp-purple)
![license: MIT](https://img.shields.io/badge/license-MIT-blue)

Pemutar **YouTube** mungil lewat command-line yang nge-stream video pakai **mpv + yt-dlp**, bukan tab browser — dibuat buat **mesin RAM kecil**. Putar lewat URL, kata kunci pencarian, atau livestream sebuah channel, lengkap dengan pilihan kualitas dan cookie auth opsional.

> Satu tab YouTube di browser bisa makan **3–4 GB**. Di Mac 8 GB itu artinya swap, panas, dan crash. `nonton` muter video yang sama di kisaran **~500 MB**.

```bash
nonton "lofi hip hop"                                   # cari, pilih dari menu
nonton https://www.youtube.com/watch?v=aqz-KE-bpKQ      # putar URL
nonton -q 4k @WindahBasudara                            # livestream channel, di 4K
```

## Kenapa nonton

- **Hemat RAM** — mpv pakai memori jauh lebih kecil dari browser. Tanpa tab, tanpa renderer, tanpa 4 GB.
- **2K / 4K gratis** — putar resolusi penuh. 2K/4K di YouTube itu gratis; gak butuh Premium.
- **Sadar codec** — utamakan **AV1** di Apple Silicon (M3+ decode AV1 di hardware), menghindari software VP9 decode yang berat dan bikin panas.
- **Shortcut live channel** — `nonton windah` atau `nonton @handle` langsung lompat ke livestream channel.
- **Cookie auth, tanpa OAuth** — pinjam cookie dari browser yang sudah login buat ngakalin throttle bot dan akses stream age-restricted / members-only / bitrate Premium. Tanpa password, tanpa token, tanpa risiko ban.

## Instalasi

Butuh [`mpv`](https://mpv.io) dan [`yt-dlp`](https://github.com/yt-dlp/yt-dlp); JS runtime ([`deno`](https://deno.com)) buat challenge solver yt-dlp; dan opsional [`fzf`](https://github.com/junegunn/fzf) buat menu pencarian.

```bash
brew install mpv yt-dlp deno fzf

git clone https://github.com/ariwisnu/nonton.git
ln -s "$PWD/nonton/nonton" /opt/homebrew/bin/nonton   # atau dir mana pun di PATH-mu
```

Tanpa `deno`, sebagian video gagal di-resolve. Tanpa `fzf`, hasil pencarian teratas langsung diputar tanpa menu.

## Pemakaian

```bash
nonton <url|kata kunci>        # putar
nonton <nama|@handle>          # putar livestream channel itu
nonton -q 4k <url>             # kualitas: 4k | 2k | 1080 | 720
nonton --login [browser]       # set up cookie auth (default: chrome)
nonton --no-cookies <url>      # putar sekali tanpa cookie
nonton --help
```

**Pencarian** — `nonton retro synthwave` buka menu picker [fzf](https://github.com/junegunn/fzf) berisi hasil teratas; pilih satu, langsung jalan. Gak ada fzf? Hasil teratas yang diputar.

**Live channel** — nama polos atau `@handle` di-resolve ke `/live` channel itu. Daftarkan nama pendek dengan menambah satu case di `channel_handle()` dalam script:

```bash
windah) printf '@WindahBasudara' ;;
```

## Cookie auth

`nonton --login` minjam cookie dari browser yang sudah login lewat `yt-dlp --cookies-from-browser`. Gak ada yang ditulis ke disk — cookie dibaca live dari penyimpanan terenkripsi browser saat play. Di macOS, run pertama memicu prompt Keychain buat kunci safe-storage browser; pilih **Always Allow**.

> **Chrome v127+** mengenkripsi cookie di balik kunci Keychain. Kalau `--login` pertama kelihatan hang, sebenarnya ada dialog persetujuan nunggu di layar — klik **Always Allow**. Kalau cookie Chrome rusak setelah update, pakai browser lain: `nonton --login firefox`.

Yang sebenarnya kamu dapat dari login: bukan resolusi (2K/4K gratis), tapi playback **anti-throttle** plus akses stream **age-restricted**, **members-only**, dan **bitrate Premium**.

## Konfigurasi

Setting ada di `~/.config/nonton/config` (ditulis oleh `--login`):

```bash
COOKIE_BROWSER=chrome   # chrome | firefox | brave | edge | ...  (kosong = tanpa cookie)
QUALITY=1080            # kualitas default: 4k | 2k | 1080 | 720
```

## Cara kerja

`nonton` me-resolve input jadi URL (pencarian → hasil teratas/fzf, nama/`@handle` → live channel, URL → apa adanya), menyusun format string yt-dlp yang mengutamakan AV1 dengan batas tinggi sesuai pilihan, lalu menyerahkannya ke mpv dengan `--cookies-from-browser` kalau ada browser yang diset. Satu script Bash mandiri, tanpa langkah install selain symlink.

## Lisensi

MIT — lihat [LICENSE](LICENSE).

---

<sub>Kata kunci: pemutar YouTube terminal, YouTube command-line, nonton YouTube di terminal, mpv YouTube, yt-dlp player, YouTube hemat RAM, macOS Apple Silicon M3, AV1 hardware decode, YouTube CLI, video player ringan.</sub>
