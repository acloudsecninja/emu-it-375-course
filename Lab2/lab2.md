# Lab 2 — 802.11 Frame Analysis & WLAN Discovery

**Course:** IT 375 — Wireless Network Administration
**Week:** 4 — 802.11 Frames, MAC Operations & Wireshark Analysis
**Points:** 75 points
**Submission:** Push your branch + open a PR in the student upload repo; submit PR link and `.wmv` video via Canvas

> **Objectives & Outcomes:** Refer to the course syllabus (distributed via the course Google Shared Drive) for course objectives and grading criteria.
>
> This lab aligns to the Week 3–4 slide decks: *RF Fundamentals & 802.11 Standards* and *802.11 Frames & Wireless Packet Analysis*.

---

## Lab Overview

In this lab you will move from theory to packets. You will:

1. Map the RF/channel landscape around you using a WLAN scanner.
2. Dissect real 802.11 frames in Wireshark using instructor-provided capture files (no monitor-mode adapter required).
3. Document the association/authentication exchange between a client and an AP.
4. Produce a short written analysis committed to your student upload repo.

**Instructor-provided capture files** are located in this repo under `Lab2/captures/` (also linked in Canvas). If a capture file is missing, download the sample `802.11` captures from https://wiki.wireshark.org/SampleCaptures (the "Wireless" section) and note which files you used.

> **Legal note:** All analysis in this lab uses provided captures or your own network. Do not capture traffic on networks you do not own. See the syllabus Wireless Lab Ethics section.

---

## Prerequisites

- [ ] Lab 1 completed — Wireshark and a WLAN scanner installed and verified
- [ ] GitHub Desktop or Git CLI configured; both course repos cloned
- [ ] Screen recording software ready (`.wmv` export)

---

## Part 1 — WLAN Discovery Scan (15 pts)

Using the WLAN scanner you installed in Lab 1 (WiFi Analyzer, NetSpot, Acrylic, inSSIDer, or `nmcli dev wifi`):

1. Run a scan of **your own** environment.
2. For each discovered network, record: SSID (or `<hidden>`), BSSID (may be partially masked by your OS — that's fine), band, channel, channel width (if shown), signal (dBm), and security type (WPA2/WPA3/Open).
3. Identify the **two most congested channels** in each band you can see.
4. Record whether any SSIDs appear to be hidden, open, or WEP/WPA-legacy.

**Deliverable:** `Lab2/wlan-survey.md` — a markdown table of your scan results plus 3–5 sentences interpreting what you saw (e.g., "channels 6 and 11 in 2.4 GHz are saturated; my AP sits on 6 with 12 neighbors").

**Screenshot 1:** Scanner output showing the neighboring networks table/heatmap.

---

## Part 2 — Channel & RF Worksheet (15 pts)

Answer the following in `Lab2/rf-worksheet.md`:

1. A client measures an AP at **-45 dBm**. A second AP reads **-78 dBm**. Which has the stronger signal, and roughly how many times more power does -45 dBm represent than -78 dBm?
2. List the three non-overlapping 20 MHz channels in the 2.4 GHz band and explain why only those three are used in dense deployments.
3. An AP and client negotiate an 80 MHz channel in the 5 GHz band. Give one benefit and one risk of wide channels in a crowded environment.
4. Define **SNR** in one sentence. What minimum SNR is generally needed for reliable high-rate data service, and what happens to MCS/data rates as SNR drops?
5. Explain the difference between **dB**, **dBm**, and **dBi**, and give one example of where each is used in WLAN work.

**Deliverable:** `Lab2/rf-worksheet.md` with numbered answers (2–4 sentences each is fine).

---

## Part 3 — Wireshark: 802.11 Frame Dissection (25 pts)

Open the instructor-provided capture (`Lab2/captures/`) or a Wireshark sample 802.11 capture.

### 3A — Frame types

Use display filters to isolate each frame type. For each, capture the packet list and the frame detail pane:

```
wlan.fc.type == 0    # Management frames
wlan.fc.type == 1    # Control frames
wlan.fc.type == 2    # Data frames
wlan.fc.subtype == 8 # Beacon frames only
```

### 3B — Association sequence

Find and document the connection sequence for one client:

```
wlan.fc.subtype == 0  # Association Request
wlan.fc.subtype == 1  # Association Response
wlan.fc.subtype == 11 # Authentication
wlan.fc.subtype == 4  # Probe Request
wlan.fc.subtype == 5  # Probe Response
```

In `Lab2/frame-analysis.md`, document for **one** client association:

- The exact frame subtype sequence observed (probe → auth → assoc → 4-way handshake if present)
- The client's MAC (or anonymized MAC), the AP's BSSID, and the SSID
- The supported rates advertised in the Association Request
- Whether the capture shows Open System or Shared Key auth, and whether a 4-way handshake (EAPOL) follows

### 3C — Radiotap/PHY metadata

Expand the **Radiotap header** (or 802.11 radio information) on one frame and record:

- Channel frequency and channel number the frame was captured on
- Signal strength (dBm) at capture time
- Data rate

**Screenshots:**

- **Screenshot 2:** Beacon frame detail pane expanded (SSID, supported rates, channel visible).
- **Screenshot 3:** Your association/authentication frame sequence in the packet list.
- **Screenshot 4:** Radiotap header detail showing channel + signal + data rate.

**Deliverable:** `Lab2/frame-analysis.md` containing the items in 3B and 3C plus a 3–5 sentence explanation of **why beacon, probe, and association frames exist** in the 802.11 MAC.

---

## Part 4 — Your Own Wireless Interface (10 pts)

Identify how your own machine's wireless connection presents itself:

```powershell
# Windows — PowerShell or CMD:
netsh wlan show interfaces
```

```bash
# WSL/Linux (if adapter visible):
iw dev
nmcli -f IN-USE,SSID,BSSID,CHAN,RATE,SIGNAL,SECURITY dev wifi
```

Record: your SSID, BSSID, channel, band, negotiated PHY type (e.g., 802.11ax/be/ac), and receive/transmit rate.

**Screenshot 5:** `netsh wlan show interfaces` (or `iw dev`/`nmcli`) output.

**In `Lab2/frame-analysis.md`**, add 3–5 sentences comparing your live interface report to what you saw in the capture (band, channel width, rates).

---

## Part 5 — Commit, Push & Pull Request

On the **student upload repo**:

1. Branch: `lab2-<your-username>`
2. Add your files under `Lab2/` (`wlan-survey.md`, `rf-worksheet.md`, `frame-analysis.md`, `screenshots/`)
3. Commit: `"Lab 2: 802.11 frame analysis and WLAN discovery"`
4. Push and open a PR to `main`.

**Screenshot 6:** The open pull request showing your `Lab2/` folder.

---

## Part 6 — Video Walkthrough

Record a single `.wmv` video (target 5–10 minutes) demonstrating:

1. Your WLAN scanner results and your interpretation of congestion.
2. Wireshark: applying each display filter, one beacon frame expanded, and your association sequence.
3. The radiotap metadata on one frame.
4. Your `netsh wlan show interfaces` output.
5. Your open PR and committed files.

---

## Submission Requirements

| Item | Format | Required |
|------|--------|----------|
| Video walkthrough | `.wmv` / `.mp4` | Yes |
| `wlan-survey.md`, `rf-worksheet.md`, `frame-analysis.md` | `.md` | Yes |
| Screenshots (in `Lab2/screenshots/`) | `.png` / `.jpg` | Yes |
| Pull request link | URL | Yes |

---

## Grading Rubric

| Item | Points |
|---|---|
| WLAN survey table + interpretation | 15 |
| RF worksheet (5 correct answers) | 15 |
| Wireshark dissection + frame-analysis.md | 25 |
| Live interface report + comparison | 10 |
| PR opened correctly with all files | 5 |
| Video walkthrough | 5 |
| **Total** | **75** |

---

## Tips & Resources

- **802.11 display filter reference:** https://www.wireshark.org/docs/dfref/w/wlan.html
- **Sample captures:** https://wiki.wireshark.org/SampleCaptures
- Stuck? Post in the Slack channel or email Professor Weber.

---

> **Reminder:** All work must be your own. AI-generated work is prohibited per syllabus. Only analyze provided captures or your own network.
