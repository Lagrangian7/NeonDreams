# Neon Dreams

**By Brent A. Hutchens**

An 80s synthwave screensaver for macOS: a glowing neon grid racing toward a striped sunset, with wireframe mountains, palm trees and floating shapes drifting past. Written in Objective-C with legacy OpenGL, no external assets, and built for **macOS 10.13 High Sierra** (it should also load on later versions that still support `.saver` bundles and OpenGL).

## Features

- Scrolling neon grid floor with a glow pass and distance fade, plus a gently swaying camera
- Striped retro sun with bands that slowly slide down it, and a soft additive glow
- Twinkling starfield and occasional shooting stars
- Wireframe mountains, palm trees, cubes, pyramids, octahedrons, spheres and spinning portal rings
- Subtle CRT scanline overlay
- Retina support and 4x multisampling (falls back automatically if unavailable)

### What's random

| Element | Behaviour |
| --- | --- |
| Color palette | One of 5 (synthwave, Miami, red sunset, ultraviolet, terminal green) chosen at launch, then a smooth fade to a new one every 45-80 seconds |
| Speed, sun size, grid style | Re-rolled each time the screensaver starts (grid is single- or dual-colored) |
| Scenery | Type, size, position, color and spin of every object are re-rolled each time it recycles into the distance |
| Sky events | Shooting stars at random intervals |

## Install

### Prebuilt

1. Download `NeonDreams.saver.zip` and unzip it.
2. Double-click `NeonDreams.saver`; System Preferences will offer to install it.
3. Choose **Neon Dreams** in **System Preferences > Desktop & Screen Saver**.

The prebuilt bundle is **unsigned** and **Intel (x86_64) only**. If macOS blocks it:

- Right-click the file, choose **Open**, and confirm, or
- Clear the download quarantine flag: `xattr -cr ~/Downloads/NeonDreams.saver`

### Build from source

Requires the Xcode command line tools (`xcode-select --install`); a full Xcode project is not needed.

```bash
git clone <this-repo-url>
cd <repo>
chmod +x build.sh
./build.sh              # produces build/NeonDreams.saver
./build.sh --install    # also copies it to ~/Library/Screen Savers
```

`build.sh` compiles `NeonDreamsView.m` with `clang` as an x86_64 bundle (deployment target 10.13), assembles the `.saver` structure with `Info.plist`, and ad-hoc signs it when `codesign` is available.

## Project layout

| File | Purpose |
| --- | --- |
| `NeonDreamsView.m` | The whole screensaver: a `ScreenSaverView` subclass hosting an `NSOpenGLView`, the scene, palettes and drawing code |
| `Info.plist` | Bundle metadata; `NSPrincipalClass` is `NeonDreamsView` |
| `build.sh` | Command-line build and install script |

## Customizing

Most tweaks are constants near the top of `NeonDreamsView.m`, or in `resetScene`:

- `kPals`: add or edit color palettes (and bump `NPAL`)
- `MAXOBJ`, `NSTARS`: scenery and star density
- `FARZ`: draw distance
- `_speed`, `_sunR` (set in `resetScene`): travel speed and sun size ranges
- Palette switch timing: `frand(45, 80)` in `animateOneFrame`

Rebuild with `./build.sh --install` after changes. If a changed version doesn't appear, quit System Preferences (macOS caches loaded screensavers) and try again.

## Uninstall

Delete `~/Library/Screen Savers/NeonDreams.saver` (or `/Library/Screen Savers/NeonDreams.saver` if installed for all users).

## Notes

- OpenGL is deprecated by Apple on newer macOS versions. The source defines `GL_SILENCE_DEPRECATION` to hide the warnings; this project targets High Sierra, where OpenGL is fully supported.
- The prebuilt bundle was cross-compiled against the macOS 10.13 SDK rather than built in Xcode on a Mac. If you hit a problem, please open an issue, or rebuild from source with `build.sh`.

## Author

Created by **Brent A. Hutchens**.

## License

Copyright (c) 2026 Brent A. Hutchens. All rights reserved unless a license is added.

No license has been chosen yet. Add a `LICENSE` file (for example MIT) before publishing if you want others to be able to reuse the code.
