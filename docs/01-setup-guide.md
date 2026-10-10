# 01 — Setup & Deployment Guide

> For authorized testing and education only. See the repository [README disclaimer](../README.md#-legal--ethical-disclaimer).

This guide covers preparing the Flipper Zero for **Bluetooth HID (BadUSB over Bluetooth)** and deploying the payloads in this repo.

---

## 1. Prerequisites

| Item | Notes |
|------|-------|
| Flipper Zero | With a microSD card inserted |
| Firmware | Official firmware supports USB BadUSB; **Bluetooth BadUSB/HID** is best supported on custom firmware such as Momentum, Unleashed, or RogueMaster |
| Target device | Windows 11 and/or Android 14 (owned by you or in-scope for testing) |
| Scripting tool | Notepad++ or VS Code (save as **ANSI / UTF-8**, **CRLF** line endings) |

> ⚠️ Flash custom firmware only if you accept the risks. Always follow the firmware project's official instructions and use qP-flipper / the Flipper mobile app.

---

## 2. Understanding the transport

- **USB BadUSB** — Flipper plugged in via USB-C, enumerated as a USB keyboard.
- **Bluetooth HID** — Flipper advertises as a *wireless* keyboard; the target pairs once, then trusts it. **This project uses Bluetooth HID.**

The Ducky Script syntax is identical for both transports. Only the delivery channel changes.

---

## 3. (Optional) Rename the Flipper's Bluetooth name

Part of this project's research was **trust-based social engineering**: the Flipper was renamed to `Sony Headphones`.

- Flipper menu: **Settings → Bluetooth → (device name)**, or set it via the firmware's configuration where supported.
- A trusted-sounding name reduces the chance a user questions the pairing request.

---

## 4. Copy payloads to the SD card

1. Connect the Flipper to your computer (or use the mobile app's file browser).
2. Copy the files from [`../payloads/`](../payloads/) into the `badusb/` directory on the SD card:
   - `windows_notepad_demo.txt`
   - `android_open_youtube.txt`
3. Safely eject.

> Keep **CRLF** line endings. In Notepad++: **Edit → EOL Conversion → Windows (CR LF)**. The repo's [`.gitattributes`](../.gitattributes) enforces this for anyone who clones.

---

## 5. Pair with the target

1. On the Flipper: **Apps → GPIO → Bad USB** (or the BadUSB app), then choose the **Bluetooth** connection mode.
2. Select the payload file.
3. On the target, open **Bluetooth settings** and pair with the advertised device.
   - Windows 11: *Settings → Bluetooth & devices → Add device*.
   - Android 14: *Settings → Connected devices → Pair new device*.
4. Once paired, the OS trusts the Flipper as a keyboard — **no further prompts on reconnect**.

---

## 6. Run the payload

1. Focus the correct window/app on the target (the payloads assume a fresh desktop / browser).
2. On the Flipper, start the payload.
3. Watch the keystrokes execute. The Flipper's screen shows payload progress (e.g. `36%` → `100%`).

---

## 7. Troubleshooting

| Symptom | Fix |
|---------|-----|
| Dropped / jumbled characters | Increase `DELAY` values; Bluetooth needs more slack than USB |
| Wrong characters typed | Check keyboard layout mismatch (payloads assume US layout) |
| Nothing happens | Confirm pairing is active and the right window has focus |
| Browser shortcut `GUI b` fails on Android | The browser-launch key varies by device; adjust to the target's shortcut |

---

Next: [02 — Methodology »](02-methodology.md)
