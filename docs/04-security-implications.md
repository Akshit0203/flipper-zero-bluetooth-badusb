# 04 — Security Implications

> For authorized testing and education only. See the repository [README disclaimer](../README.md#-legal--ethical-disclaimer).

Why wireless Bluetooth HID keystroke injection matters from a defensive standpoint.

---

## 1. The core weakness: implicit HID trust

Operating systems trust keyboards unconditionally. A Human Interface Device that claims to be a keyboard is allowed to type immediately — no driver prompt, no confirmation, no rate limiting. BadUSB abuses this. Moving the vector to **Bluetooth** keeps the trust but removes the cable.

## 2. One-time pairing → long-term silent access

The most significant finding: **a single approved pairing grants persistent access.**

- After the first pairing, the OS **auto-trusts** the device on every reconnect.
- The attacker's device can re-inject keystrokes with **no prompt, no alert, and no re-authentication**.
- There is **no MFA/2FA** step in Bluetooth HID pairing to raise the bar.

This turns a momentary lapse (one careless "Pair" tap) into an ongoing exposure.

## 3. Stealth advantages over classic USB BadUSB

| Classic USB BadUSB | Bluetooth HID (this project) |
|--------------------|------------------------------|
| Requires physical USB insertion | **No cable, no physical contact** |
| Visible device plugged in | Device can be hidden / pocketed nearby |
| Limited by USB port access/policies | Bypasses USB port restrictions |
| Harder on locked-down phones | **Works on Android where USB is restricted** |

## 4. Social engineering amplifies it

Renaming the Flipper to **"Sony Headphones"** exploits human trust. People approve pairing requests from names that sound like ordinary peripherals. The technical weakness (implicit trust) and the human weakness (recognizable names) compound each other.

## 5. High-risk environments

- **BYOD / open-plan offices** — many Bluetooth devices around; a rogue pairing blends in.
- **Shared / public workstations** — unattended, unlocked sessions are prime targets.
- **Conferences, co-working spaces, reception desks** — transient physical proximity is enough.

## 6. Impact potential

The payloads here are benign, but the **same vector** can:

- Launch a terminal and run commands.
- Download and execute a second-stage payload.
- Exfiltrate data, change settings, or establish persistence — all at the speed of typing, as the logged-in user.

The limiting factor is the **privileges of the active session** — which is exactly why least privilege and screen locking matter (see [05 — Defense & Mitigation](05-defense-and-mitigation.md)).

---

## Summary

Bluetooth HID injection is **BadUSB without the "USB"** — wireless, cable-free, maskable as a trusted peripheral, and persistent after a single pairing. It deserves the same defensive attention as malicious USB devices, plus Bluetooth-specific hygiene.

---

Next: [05 — Defense & Mitigation »](05-defense-and-mitigation.md)
