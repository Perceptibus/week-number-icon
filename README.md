# Week Number Icon

A tiny Windows tray app that shows the current **ISO week number** in the notification area.

[![CI](https://github.com/rito1998/week-number-icon/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/rito1998/week-number-icon/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

![The Week Number Icon showing the current week in the Windows system tray](./assets/showcase.png)

## Features

- **Always current** — the icon refreshes on its own and flips to the new week number without a restart.
- **Styled your way** — six color presets, or fully custom text/background colors via hex input and the standard Windows color picker, with a live preview swatch.
- **Remembers everything** — settings are persisted in the registry and restored on every start.
- **Genuinely small** — plain C, no frameworks or runtimes; a single self-contained executable.
- **Every architecture** — x86, x64 and ARM64 builds, shipped as one MSIX bundle.

## Install

This app is available through the [Microsoft Store](https://apps.microsoft.com/detail/9P8Z35ZBQQKW?hl=en-us&gl=CH&ocid=pdpshare).

[![Get it from Microsoft](https://get.microsoft.com/images/en-us%20dark.svg)](https://get.microsoft.com/installer/download/9p8z35zbqqkw?referrer=appbadge)

Alternatively, install it with winget in PowerShell:

```pwsh
winget install --id 9P8Z35ZBQQKW --source msstore
```

## Settings

Right click the tray icon to open the menu:

- **Colors ▸ presets**: Default (white, transparent), White on black, Black on white, Amber on black, Green on black, Blue on white.
- **Colors ▸ Custom...**: type the text and background color as hex, or press **Pick...** to use the standard Windows color picker (the chosen color is written back into the hex field). A preview swatch updates while you type.
- **Transparent background**: keeps the original look, where the taskbar shows through the icon.

Hex colors accept `#RRGGBB`, `RRGGBB` or `0xRRGGBB` (case-insensitive), plus the `#RGB` shorthand. Invalid values are rejected with a red preview and a hint line, and the dialog stays open.

Choices are saved under `HKCU\Software\WeekNumberIcon` and restored on the next start, so the icon keeps its colors after a reboot. The icon is repainted immediately when a color changes, and automatically once a minute so it flips to the new week number on its own.

## Build

Requires [Zig](https://ziglang.org/) 0.17.0 or newer on Windows. Building the MSIX
package additionally needs `makeappx.exe` from the Windows 10/11 SDK.

### Compile

```pwsh
zig build --release=safe
```

Binaries land in `zig-out/x86`, `zig-out/x86_64` and `zig-out/aarch64`.

### Test

```pwsh
zig build test
```

### Package MSIX

Build MSIX bundle:

```pwsh
zig build msix --release=safe
```

For local testing, install the unsigned msixbundle from an elevated PowerShell:

```pwsh
Add-AppxPackage -Path '.\zig-out\msix\*-unsigned.msixbundle' -AllowUnsigned
```

## Releasing

1. Update [CHANGELOG.md](CHANGELOG.md) and bump `.version` in `build.zig.zon`.
2. Commit, then tag and push the tag: `git tag v0.2.0 && git push origin v0.2.0`.
3. CI builds the MSIX bundle and attaches it to the GitHub release.
4. Upload the bundle to Microsoft Partner Center — the Store signs it for you.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
