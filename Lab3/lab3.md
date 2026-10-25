# Lab 3 — Wireless Security Analysis: Handshake Inspection & Rogue AP Detection

**Course:** IT 375 — Wireless Network Administration
**Week:** 6 — Wireless Security I: WPA2, WPA3 & Authentication
**Points:** 75 points
**Submission:** Push your branch + open a PR in the student upload repo; submit PR link and `.wmv` video via Canvas

> **Objectives & Outcomes:** Refer to the course syllabus (distributed via the course Google Shared Drive) for course objectives and grading criteria.
>
> This lab aligns to the Week 5–7 slide decks: *WLAN Architectures & AP Deployment*, *Wireless Security Fundamentals*, and *Enterprise Wireless Security & 802.1X*.

---

## Lab Overview

Wireless security failures are usually visible in the air before they're visible in a log. In this lab you will:

1. Dissect the **WPA2 4-way handshake** in a provided capture and explain what each message proves.
2. Evaluate cipher suites across a capture and a live scan (WPA2/WPA3, CCMP/GCMP, PMF).
3. Perform a **defensive** rogue-AP / misconfiguration audit using your WLAN scanner and beacon data.
4. Explain how an attacker abuses these primitives and which controls stop them.

**Instructor-provided capture files** are in `Lab3/captures/` (also linked in Canvas), including a capture that contains a full 4-way EAPOL handshake. If files are missing, use the 802.11 WPA sample captures at https://wiki.wireshark.org/SampleCaptures and note which files you used.

> **CRITICAL ETHICS NOTE:** This lab is **analysis-only**. You will inspect provided captures and your own scanner output. Do NOT run deauthentication, handshake-capture, or cracking tools (aircrack-ng, hcxdumptool, hashcat against real captures, etc.) against any live network — including your own for this lab. The point is to recognize the *evidence* of these attacks, not to perform them. Violations fall under the syllabus Wireless Lab Ethics policy and academic integrity rules.

---

## Prerequisites

- [ ] Labs 1–2 completed (Wireshark + WLAN scanner working)
- [ ] Both course repos cloned; comfortable with the branch → commit → push → PR workflow
- [ ] Screen recording software ready (`.wmv` export)

---

## Part 1 — The 4-Way Handshake, Dissected (20 pts)

Open the provided WPA handshake capture in Wireshark.

Useful filters:

```
eapol                                  # All EAPOL key frames
wlan.fc.type == 0                      # Management frames (find assoc first)
eapol && wlan.addr == <client-mac>     # Handshake for one client
```

1. Locate the association + authentication that precedes the handshake.
2. Identify **Message 1–4** of the 4-way handshake (EAPOL-Key frames). For each message record:
   - Direction (AP→client or client→AP)
   - Which key/nonce material it carries (ANonce, SNonce, MIC, GTK install)
3. In `Lab3/security-analysis.md`, answer:
   - What does each party prove to the other by the end of message 4?
   - Why does message 2's MIC matter — what would a mismatch mean?
   - Where does the **PTK** come from and what inputs build it (PMK, nonces, MACs)?
   - Why is capturing this handshake *not* the same as knowing the passphrase — and what offline attack does it enable? (Describe the concept; do not perform it.)
4. If the capture contains WPA3/SAE instead of or in addition to WPA2, note the differences (SAE commit/confirm vs. 4-way).

**Screenshot 1:** Packet list showing the four EAPOL handshake messages.
**Screenshot 2:** One EAPOL-Key frame detail pane expanded (key info, nonce, MIC fields).

---

## Part 2 — Security Posture Audit of Your Own Environment (20 pts)

Using your WLAN scanner (and `netsh wlan show interfaces` / `netsh wlan show networks mode=bssid`), audit what you can see:

1. For each visible network, record the advertised security: Open / WEP / WPA / WPA2 / WPA3 / mixed transition modes.
2. Note any AP advertising **WEP or WPA-TKIP** — flag as a finding.
3. Note any **open SSIDs** (guest networks, captive portals) and which vendor OUIs the BSSIDs map to (use https://www.wireshark.org/tools/oui-lookup.html or your scanner's vendor column).
4. Check your own network's settings (router admin page or `netsh`): is WPA3 or WPA2/WPA3 transition mode enabled? Is PMF (802.11w) required, optional, or disabled? Is WPS enabled? (WPS on = a finding.)
5. Optional bonus: If you run an open or transition SSID you control, document how you'd migrate it to WPA3-only.

**Deliverable:** `Lab3/posture-audit.md` — a table of observed networks/security plus a findings list (minimum 3 findings or "no finding" justifications).

**Screenshot 3:** Scanner/netsh output showing security types per network.

---

## Part 3 — Rogue AP & Evil-Twin Recognition (Defensive) (20 pts)

Attackers impersonate legitimate BSSIDs. Defenders catch them through inconsistency. In `Lab3/rogue-analysis.md`:

1. Using your scanner data and/or the provided beacon capture, explain **three concrete indicators** you would use to detect a rogue AP impersonating a corporate SSID:
   - Examples: BSSID OUI mismatch, channel inconsistency, missing expected RSN/PMF capabilities in beacons, unexpected signal strength, duplicate SSID with different security settings.
2. Pick **one** network from your own scan and show how you would fingerprint its *legitimate* beacon (BSSID, channel, capabilities) so a rogue twin could be flagged later.
3. Describe how a **deauthentication attack** would appear in a capture or scanner — what frame subtype, what symptom would a client experience, and what mitigation exists (PMF/802.11w).
4. One paragraph: how would an enterprise WIPS surface a rogue AP differently than your manual scan?

**Deliverable:** `Lab3/rogue-analysis.md`.

---

## Part 4 — Wireless Security Policy Snippet (10 pts)

Write `Lab3/wlan-policy.md` — a short policy block (½–1 page) suitable for a company handbook covering:

- Minimum acceptable cipher/auth (e.g., "WPA2-AES/CCMP minimum; WPA3-SAE preferred; WPA3-only for admin VLAN")
- PMF requirement, WPS prohibition, PSK rotation interval, guest network isolation rule
- One sentence on approved testing: "wireless security testing requires written authorization from ___"

---

## Part 5 — Commit, Push & Pull Request

On the **student upload repo**:

1. Branch: `lab3-<your-username>`
2. Files under `Lab3/`: `security-analysis.md`, `posture-audit.md`, `rogue-analysis.md`, `wlan-policy.md`, `screenshots/`
3. Commit: `"Lab 3: wireless security analysis"` → push → PR to `main`.

**Screenshot 4:** The open PR showing your `Lab3/` folder.

---

## Part 6 — Video Walkthrough

Record a single `.wmv` video (target 5–10 minutes):

1. The 4-way handshake messages in Wireshark, with you verbally identifying each message's role.
2. Your posture audit findings — walk through your scanner/netsh evidence.
3. Your rogue-detection indicators explained.
4. Your open PR and committed files.

---

## Submission Requirements

| Item | Format | Required |
|------|--------|----------|
| Video walkthrough | `.wmv` / `.mp4` | Yes |
| `security-analysis.md`, `posture-audit.md`, `rogue-analysis.md`, `wlan-policy.md` | `.md` | Yes |
| Screenshots (in `Lab3/screenshots/`) | `.png` / `.jpg` | Yes |
| Pull request link | URL | Yes |

---

## Grading Rubric

| Item | Points |
|---|---|
| Handshake dissection + security-analysis.md | 20 |
| Security posture audit + findings | 20 |
| Rogue AP / evil twin analysis | 20 |
| WLAN policy snippet | 10 |
| PR + video | 5 |
| **Total** | **75** |

---

## Tips & Resources

- **EAPOL filter docs:** https://www.wireshark.org/docs/dfref/e/eapol.html
- **OUI lookup:** https://www.wireshark.org/tools/oui-lookup.html
- WPA3/SAE background: see Week 6 slide deck and assigned readings in Canvas.

---

> **Reminder:** All work must be your own. AI-generated work is prohibited per syllabus. Analysis only — do not attack or capture live networks.
