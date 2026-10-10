# 05 — Defense & Mitigation

> For authorized testing and education only. See the repository [README disclaimer](../README.md#-legal--ethical-disclaimer).

How to defend against wireless Bluetooth HID (BadUSB-over-Bluetooth) keystroke injection. Grouped by audience.

---

## For individual users

- **Turn Bluetooth off when you don't need it.** No radio, no pairing.
- **Never approve an unexpected pairing request** — even from a trusted-sounding name like "Sony Headphones". Verify the device physically.
- **Audit paired devices regularly** and remove anything unfamiliar:
  - Windows 11: *Settings → Bluetooth & devices*.
  - Android 14: *Settings → Connected devices*.
- **Lock your screen when you step away** (`Win + L`). Injected keystrokes need an interactive, unlocked session to do damage.
- **Make Bluetooth non-discoverable** when idle where the OS allows it.

---

## For IT / security teams

- **Endpoint controls / EDR** — deploy tooling that can detect anomalous HID behavior (e.g. a "keyboard" typing faster or more scripted than a human).
- **Device-control policy** — on managed fleets, restrict or alert on enrollment of new HID devices (USB and Bluetooth).
- **Least privilege** — standard users, no local admin. A typed command should have limited blast radius.
- **Session hygiene** — enforce short auto-lock timeouts via policy (GPO / MDM).
- **Bluetooth policy** — disable Bluetooth on endpoints that don't need it; where needed, restrict to approved devices.
- **Physical security** — reception desks, kiosks, and shared terminals should auto-lock and sit out of arm's reach of the public.
- **User awareness training** — teach staff that a "pair?" prompt from an unknown device is a security event, not a convenience.

---

## Detection ideas

| Signal | Why it helps |
|--------|--------------|
| New HID device enrolled outside change windows | Flags rogue keyboards (BT or USB) |
| Keystroke cadence that is too fast/too regular | Humans don't type at perfect intervals |
| Run dialog / shell spawned immediately after a BT pairing | Classic injection sequence |
| Bluetooth pairing events on servers / kiosks | Those rarely need new keyboards |

---

## Defense-in-depth checklist

- [ ] Bluetooth disabled when unused
- [ ] Paired-device list audited and cleaned
- [ ] Auto-lock enforced (short timeout)
- [ ] Users run as non-admin (least privilege)
- [ ] EDR / device-control monitoring HID enrollment
- [ ] Staff trained to reject unexpected pairing requests
- [ ] Kiosks/shared terminals physically secured and locked

---

## Key takeaway

You can't patch "keyboards are trusted" — so defense relies on **hygiene** (don't pair blindly, audit often), **least privilege** (limit what a keystroke can do), and **monitoring** (spot the device and the behavior). Treat Bluetooth peripherals with the same suspicion as an unknown USB stick.

---

« Back to [README](../README.md)
