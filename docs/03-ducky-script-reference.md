# 03 — Ducky Script Reference

> For authorized testing and education only. See the repository [README disclaimer](../README.md#-legal--ethical-disclaimer).

A quick reference for the BadUSB / Ducky Script commands used by the payloads in this repository. Flipper Zero's BadUSB app (USB and Bluetooth HID) interprets this syntax.

---

## Commands used in this project

| Command | Meaning |
|---------|---------|
| `REM <text>` | Comment — ignored during execution. Used for headers/notes. |
| `DELAY <ms>` | Pause for `<ms>` milliseconds. Critical for Bluetooth timing. |
| `STRING <text>` | Type `<text>` literally (no trailing Enter). |
| `STRINGLN <text>` | Type `<text>` **and** press Enter. |
| `ENTER` | Press the Enter/Return key. |
| `TAB` | Press Tab (field/element navigation). |
| `GUI <key>` | Press the OS/Windows/Meta key, optionally + another key. `GUI r` = Run dialog; `GUI b` = browser shortcut on some targets. |
| `CTRL <key>` | Press Ctrl + `<key>`. `CTRL l` focuses the browser address bar. |

---

## Common additional commands (reference)

| Command | Meaning |
|---------|---------|
| `ALT <key>` | Press Alt + key. |
| `SHIFT <key>` | Press Shift + key. |
| `CTRL-ALT <key>` | Modifier combo. |
| `DEFAULTDELAY <ms>` / `DEFAULT_DELAY <ms>` | Default pause inserted between every command. |
| `REPEAT <n>` | Repeat the previous command `n` times. |
| `UP` / `DOWN` / `LEFT` / `RIGHT` | Arrow keys. |
| `ESC` / `DELETE` / `SPACE` / `BACKSPACE` | Named keys. |
| `CAPSLOCK` / `PRINTSCREEN` | Named keys. |

---

## Timing cheat-sheet (Bluetooth HID)

Bluetooth needs more slack than USB. Rules of thumb used here:

| Action | Suggested delay |
|--------|-----------------|
| After opening the Run dialog (`GUI r`) | `DELAY 500` |
| After launching an app (Notepad, browser) | `DELAY 3000` |
| After navigating to a URL | `DELAY 1500`–`2200` |
| After each `ENTER` in multi-line text | `DELAY 700` |

If characters drop or reorder, **increase the delays** before anything else.

---

## Worked example (annotated)

```ducky
DELAY 1000                              REM let pairing/focus settle
GUI r                                   REM open Run dialog
DELAY 500
STRING cmd /c start https://flipperzero.one/   REM open URL via cmd
ENTER
DELAY 3000                              REM wait for browser to open
GUI r
DELAY 500
STRING notepad                          REM launch Notepad
ENTER
DELAY 3000
STRING Hello from Flipper Zero!         REM type a line
ENTER
DELAY 700                               REM pause after ENTER
```

See the full payloads in [`../payloads/`](../payloads/).

---

Next: [04 — Security Implications »](04-security-implications.md)
