# 02 — Methodology

> For authorized testing and education only. See the repository [README disclaimer](../README.md#-legal--ethical-disclaimer).

This document records exactly what was tested, how, and why — so the work is reproducible and transparent.

---

## Objective

Demonstrate that a Flipper Zero, acting as a **Bluetooth HID keyboard**, can perform **BadUSB-style keystroke injection without a USB cable**, and evaluate the security weaknesses that make the attack practical and stealthy.

---

## Threat model

- **Attacker capability:** Can place/operate a small Bluetooth device near the target and obtain a one-time pairing (via social engineering or an unattended, unlocked device).
- **Attacker goal:** Execute arbitrary keystrokes/commands on the target.
- **Victim assumption:** Treats Bluetooth peripherals (keyboards, headphones) as benign.

---

## Test matrix

| # | Platform | Payload | Expected behaviour | Result |
|---|----------|---------|--------------------|--------|
| 1 | Windows 11 | `windows_notepad_demo.txt` | Open Flipper site → open Notepad → type multi-line message | ✅ Reliable |
| 2 | Android 14 | `android_open_youtube.txt` | Open browser → load URL → trigger playback | ✅ Reliable |

---

## Procedure

1. **Prepare device** — Flipper Zero configured for Bluetooth HID; Bluetooth name set to `Sony Headphones` to test implicit trust.
2. **Author payloads** — Written in BadUSB / Ducky Script syntax, saved as ANSI with **CRLF** line endings for reliable character delivery.
3. **Pair once** — Each target paired with the Flipper a single time. No MFA/2FA was requested during HID pairing.
4. **Inject** — Payloads executed with the appropriate window focused. Timing controlled entirely with `DELAY`.
5. **Test persistence** — Disconnected and reconnected; confirmed the Flipper could re-inject **without any new prompt or re-authentication**.
6. **Document** — Captured video and stills (see [`../assets/`](../assets/)).

---

## Why timing matters (DELAY tuning)

Bluetooth HID has more latency and jitter than wired USB. Keystrokes sent faster than the target can process them get dropped or reordered. The payloads therefore:

- Wait after launching apps (`DELAY 3000` after opening Notepad / the browser).
- Wait after **every** `ENTER` (`DELAY 700`) when typing multi-line content.
- Give page loads time to settle (`DELAY 2200` after navigating to a URL).

**Reliability — not speed — is the design goal.**

---

## Social-engineering layer

Renaming the Flipper to `Sony Headphones` tests a human weakness rather than a technical one: users approve pairing requests from names they recognize. Combined with the HID trust model, a single approved pairing yields **persistent, silent keystroke access**.

---

## Observations

- Pairing involved **no second factor**.
- Post-pairing, the device was **auto-trusted on reconnect** with no alert.
- The **same Ducky Script syntax** worked across USB-style and Bluetooth delivery.
- Android's typical **USB restrictions were bypassed** because the vector is Bluetooth, not USB.

---

Next: [03 — Ducky Script Reference »](03-ducky-script-reference.md)
