# Windhawk — Perform Speed Test Redirect

A [Windhawk](https://windhawk.net/) mod that redirects the **"Perform speed test"** option in the Windows taskbar network right-click menu away from Microsoft's default page to a URL of your choice (defaults to [speedtest.net](https://www.speedtest.net)).

## The problem

Right-clicking the network icon in the Windows 11 taskbar and clicking **"Perform speed test"** opens:

```
https://go.microsoft.com/fwlink/?linkid=2324916
```

This mod intercepts that and sends you somewhere better.

## Installation

1. Install [Windhawk](https://windhawk.net/)
2. In Windhawk, go to **Find mods** and search for `perform-speedtest-redirect`, **or** manually install:
   - Click **Create new mod**
   - Paste the contents of [`perform-speedtest-redirect.wh.cpp`](./perform-speedtest-redirect.wh.cpp) into the editor
   - Click **Compile & Run**
3. Right-click the taskbar network icon → **Perform speed test** — it now opens speedtest.net

## Settings

In the Windhawk mod settings panel you can change the **Redirect URL** to any speed test site:

| Site | URL |
|------|-----|
| Speedtest.net (default) | `https://www.speedtest.net/` |
| Fast.com (Netflix) | `https://fast.com` |
| Cloudflare | `https://speed.cloudflare.com` |

## How it works

The mod injects into `explorer.exe`, `ShellExperienceHost.exe`, and `RuntimeBroker.exe` and hooks three functions in the Windows API:

- `ShellExecuteW` — classic shell URL open
- `ShellExecuteExW` — extended shell URL open
- `CreateProcessW` — catches the browser being spawned directly

Any call to these functions that contains `linkid=2324916` in the URL has that URL replaced with the configured redirect before the browser is launched.

## Building / contributing

The mod is a single self-contained `.wh.cpp` file compiled by Windhawk's built-in Clang toolchain — no external build system needed.

## License

MIT
