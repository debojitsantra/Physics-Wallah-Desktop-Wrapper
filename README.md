# PW

A distraction-free desktop wrapper for [Physics Wallah](https://www.pw.live), built with [Pake](https://github.com/tw93/Pake). No browser tabs, no other websites, no excuse to "just quickly check something else" mid-lecture.

> **Disclaimer**: This is an unofficial, community-built tool. It is not affiliated with, endorsed by, or sponsored by Physics Wallah. All PW content, trademarks, and branding belong to their respective owners. This project only packages the existing PW website into a standalone window — it does not modify, host, or redistribute any of PW's content.

## What this is

this wrapper turns the PW web app into a single-purpose desktop application. Instead of opening PW in a browser tab next to twenty other tabs, you get one window that does exactly one thing: PW, and nothing else.

It's built on [Pake](https://github.com/tw93/Pake), an open-source Rust/Tauri tool that wraps any website into a lightweight native app — typically a few MB, instead of the 100+ MB an Electron-based wrapper would cost.

## Features

- Opens directly to the study dashboard
- Videos, notes, PDFs, and the books section all work and stay inside the app window
- No other websites can be opened from within the app - every link stays on PW
- Lightweight: a few MB installer, not a bundled browser
- Open source end to end (this repo, Pake, and Tauri)

## Download

Pre-built Windows installer: see the [Releases](../../releases) page. A new release is published automatically every time the build workflow runs.

If you'd rather build it yourself (recommended if you don't trust pre-built binaries from strangers on the internet, which is a reasonable instinct), see below.

## Building it yourself

You don't need Rust, Node, or any local toolchain installed. This repo builds the app entirely on GitHub's servers using GitHub Actions, and publishes the result straight to this repo's Releases page.

1. Fork or clone this repository.
2. Go to the **Actions** tab on your copy of the repo.
3. Select the **Build PW App** workflow.
4. Click **Run workflow**.
5. Wait 5–10 minutes for the first build (subsequent builds are faster).
6. Check the **Releases** page — the `.exe`/`.msi` will be attached automatically once the workflow finishes, no manual artifact download needed.

There's no tag-push step required. Since this project has no source code to version (it just wraps a live URL), each run generates its own release tagged `build-N` based on the workflow run number.

### The build command

If you want to tweak the app yourself, this is the core command (see [`.github/workflows/build.yml`](.github/workflows/build.yml)):

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

## Why GitHub Actions instead of building locally

Setting up Rust and the Tauri toolchain locally takes 30–60 minutes and several GB of disk space — a bad trade-off for a tool you'll touch once or twice. GitHub Actions runs the entire build on a disposable cloud machine, so your own disk and install history stay untouched.

## Contributing

Found a page on PW that still breaks (opens externally, fails to load, blank screen)? Open an issue with the URL and what you see in DevTools (`Ctrl+Shift+I` inside the app, since this build includes `--debug`). Pull requests adjusting build flags or the workflow are welcome.

## Credits

- [Pake](https://github.com/tw93/Pake) by [tw93](https://github.com/tw93) — the actual engine behind this, MIT licensed.
- [Tauri](https://tauri.app) — the framework Pake is built on.

## License

MIT for this repository's build configuration and documentation. This project claims no rights over Physics Wallah's content, branding, or platform.
