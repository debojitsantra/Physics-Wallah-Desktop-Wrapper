# PW Desktop

A distraction-free desktop wrapper for [Physics Wallah](https://www.pw.live), built with [Pake](https://github.com/tw93/Pake). No browser tabs, no other websites.

> **Disclaimer**: This is an unofficial, community-built tool. It is not affiliated with, endorsed by, or sponsored by Physics Wallah. All PW content, trademarks, and branding belong to their respective owners. This project only packages the existing PW website into a standalone window - it does not modify, host, or redistribute any of PW's content.

## What this is

This wrapper turns the PW web app into a single-purpose desktop application. Instead of opening PW in a browser tab next to twenty other tabs, you get one window that does exactly one thing: PW, and nothing else.

It's built on [Pake](https://github.com/tw93/Pake), an open-source Rust/Tauri tool that wraps any website into a lightweight native app — typically a few MB, instead of the 100+ MB an Electron-based wrapper would cost.

## Features

- Opens directly to the study dashboard
- Videos, notes, PDFs, and the books section all work and stay inside the app window
- No other websites can be opened from within the app — every link stays on PW
- Builds for Windows, macOS, and Linux from the same source
- Lightweight: a few MB installer, not a bundled browser
- Open source end to end (this repo, Pake, and Tauri)

## Download

See the [Releases](../../releases) page for pre-built installers:

- **Windows**: `.exe` or `.msi`
- **macOS**: `.dmg`
- **Linux**: `.deb` or `.AppImage`

A new release is published automatically every time the build workflow runs.

If you'd rather build it yourself (recommended if you don't trust pre-built binaries from strangers on the internet, which is a reasonable instinct), see below.

## Building it yourself

You don't need Rust, Node, or any local toolchain installed. This repo builds the app for all three platforms entirely on GitHub's servers using GitHub Actions, and publishes the result straight to this repo's Releases page.

1. Fork or clone this repository.
2. Go to the **Actions** tab on your copy of the repo.
3. Select the **Build PW App** workflow.
4. Click **Run workflow**.
5. Wait 5–10 minutes for the first build (subsequent builds are faster).
6. Check the **Releases** page — `.exe`/`.msi`, `.dmg`, `.deb`, and `.AppImage` will all be attached automatically once the workflow finishes, no manual artifact download needed.

### The build command

If you want to tweak the app yourself, this is the core command, run on each platform's runner (see [`.github/workflows/build.yml`](.github/workflows/build.yml)):

```bash
pake https://www.pw.live/study-v2/study \
  --name PW \
  --width 1280 \
  --height 800 \
  --force-internal-navigation \
  --new-window \
  --wasm
```

What each flag does:

| Flag | Why it's here |
|---|---|
| `--force-internal-navigation` | Keeps every link inside the app window, regardless of which domain it points to. Nothing escapes to your system browser. |
| `--new-window` | PW opens PDFs and the books viewer via a new tab/window. Without this flag, that action is silently blocked and those features appear broken. |
| `--wasm` | Adds cross-origin isolation headers required by PDF.js-based viewers that use WebAssembly for rendering. |


## Credits

- [Pake](https://github.com/tw93/Pake) by [tw93](https://github.com/tw93) — the actual engine behind this, GPL-3.0 licensed.
- [Tauri](https://tauri.app) — the framework Pake is built on.

## License

MIT for this repository's build configuration and documentation. This project claims no rights over Physics Wallah's content, branding, or platform.
