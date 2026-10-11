# 30nama for Samsung Tizen TV

A Netflix-style Samsung Tizen TV app: an unofficial client for your own 30nama.com subscription.

![30nama TV](docs/images/poster.png)

> **Disclaimer:** This is an unofficial, community project. It is not affiliated with, endorsed by, or supported by 30nama. You need your own valid 30nama subscription to use it. No content is bundled with this app.

## Features

- Sign in by scanning a QR code with your phone (no typing on the remote)
- Netflix-style TV UI: hero banner, rows, details pages, search, remote-friendly navigation
- Samsung AVPlay player with HLS playback at the highest available quality
- Persian (fa) and English (en) subtitles
- Persian UI with full right-to-left support (Vazirmatn font)
- Optional TMDB enrichment: trending rows, cast and crew, reviews, recommendations

## Screenshots

| Home | Browse |
|---|---|
| ![Home](docs/images/home-hero.png) | ![Browse](docs/images/home-browse.png) |
| **Details** | **Search** |
| ![Details](docs/images/details.png) | ![Search](docs/images/search.png) |

## Install

Every route needs the TV in Developer Mode: open Apps, type `12345`, switch Developer mode on, enter your PC's IP address, then reboot the TV.

### Option 1: TizenBrew (simplest)

1. Install [TizenBrew](https://github.com/reisxd/TizenBrew/blob/main/docs/README.md) on the TV with the TizenBrew Installer (one-time).
2. In TizenBrew open Module Manager, Add GitHub Module, and enter `msharifi99/30nama-tizenos`.
3. Launch "30nama" from the TizenBrew home screen.

Nothing is built or signed by you. Playback uses the TV's HTML5 player here (AVPlay is not available inside TizenBrew). See [docs/TIZENBREW.md](docs/TIZENBREW.md) for details and what is untested.

### Option 2: prebuilt .wgt

1. Download `30nama-v*.wgt` from the [latest release](https://github.com/msharifi99/30nama-tizenos/releases/latest).
2. Install [Apps2Samsung](https://github.com/Apps2Samsung/Apps2Samsung/releases/latest) (Windows, Linux, macOS or Android). It handles your Samsung certificate and re-signs the package for your TV.
3. In Apps2Samsung select your TV, choose a custom `.wgt`, pick the downloaded file and install. Tizen 7+ TVs ask you to sign in to a Samsung account once to create a certificate.

### Option 3: build from source

Requires [Tizen Studio](https://developer.tizen.org/development/tizen-studio/download) (CLI tools + TV extension), a Samsung certificate that includes your TV's DUID, and Git Bash (the scripts target Windows + Git Bash; `TIZEN_HOME` defaults to `/c/tizen-studio`).

```
./install-wizard.sh                              # guided: reads the DUID, creates certificate and profile
./build.sh <security-profile> [<tv-ip>:26101]    # manual build; with a target it also installs and launches
```

The wizard stores your answers in `.wizard.env` (git-ignored). Without a target, `build.sh` only produces a signed `.wgt` in `dist/`.

## Optional: TMDB setup

TMDB adds trending rows, cast, reviews and recommendations. It is off by default and the app works fully without it.

1. Create a free account at themoviedb.org and copy your **API Read Access Token** (v4) from Settings, API.
2. Copy the example file and paste the token:
   ```
   cp tizen-app/js/tmdb-token.example.js tizen-app/js/tmdb-token.js
   ```
   `tmdb-token.js` is git-ignored. If you skip this, `build.sh` and the dev server simply run with TMDB disabled.

This product uses the TMDB API but is not endorsed or certified by TMDB.

## Development

Run the app in a desktop browser with a small proxy (no dependencies, Node 18+):

```
cd tizen-app
node tools/devserver.js 8080
```

Then open http://localhost:8080/. The proxy forwards API and HLS requests to avoid CORS. See `tizen-app/README.md` for developer notes and `design/DESIGN.md` for the design spec.

## Structure

```
tizen-app/    the Tizen web app (config.xml, index.html, js/, css/, fonts/, tools/)
design/       design spec
docs/        README screenshots, TizenBrew notes
package.json TizenBrew module manifest
.github/     release workflow (builds the .wgt on v* tags)
media/        poster
build.sh      build, sign and install
install-wizard.sh  guided TV setup
```

## License

All rights reserved for now. The Vazirmatn font in `tizen-app/fonts/` is licensed under the SIL Open Font License (see `tizen-app/fonts/OFL.txt`).
