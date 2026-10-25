# Lab 4 — WLAN Design: Packet Tracer Deployment & Site Survey

**Course:** IT 375 — Wireless Network Administration
**Week:** 8 — Site Surveys & WLAN Design
**Points:** 75 points
**Submission:** Push your branch + open a PR in the student upload repo; submit PR link and `.wmv` video via Canvas

> **Objectives & Outcomes:** Refer to the course syllabus (distributed via the course Google Shared Drive) for course objectives and grading criteria.
>
> This lab aligns to the Week 5 and Week 8 slide decks: *WLAN Architectures & AP Deployment* and *Site Surveys & WLAN Design*.

---

## Lab Overview

This lab puts you in the role of a wireless network administrator delivering a small-business WLAN. You will:

1. Gather requirements and produce a lightweight design document.
2. Build a working multi-SSID, multi-VLAN wireless deployment in **Cisco Packet Tracer**.
3. Perform a **site survey** (predictive reasoning + a real passive scan of your own space) and justify AP placement and channel plan.
4. Deliver documentation a real admin could hand to a client.

Scenario: **Maple & Main Coffee Co.** — a two-zone small business: a public café floor (~1200 sq ft, up to 40 concurrent guests) and a back office (POS terminals, 2 staff laptops, 1 printer). Requirements:

- Guest Wi-Fi: isolated, bandwidth-capped conceptually, WPA2/WPA3, captive-portal-ready
- Staff/POS Wi-Fi: WPA2-Enterprise preferred (WPA2-PSK acceptable in Packet Tracer), segmented from guests
- POS terminals must not be reachable from guest clients
- Reasonable coverage across both zones with minimal co-channel interference

---

## Prerequisites

- [ ] Lab 1 completed — Packet Tracer installed and verified
- [ ] Lab 2 completed — WLAN scanner experience
- [ ] Both course repos cloned; PR workflow comfortable

---

## Part 1 — Requirements & Design Document (15 pts)

Write `Lab4/design-doc.md` (½–1 page) covering:

1. **Requirements table** — business need → technical requirement (e.g., "POS security → dedicated staff VLAN, guest isolation").
2. **SSID plan** — SSID names, security mode per SSID, VLAN ID mapping, band plan.
3. **Channel plan** — 2.4 GHz channels chosen and why (non-overlapping reuse); 5 GHz plan if used.
4. **AP count & placement** — how many APs and where, with one sentence of justification each (ceiling mount, coverage radius assumptions ~ indoor 2.4 GHz ≈ better range, 5 GHz ≈ better throughput).
5. **Backhaul** — how APs connect to the wired LAN (switch, trunk ports carrying both VLANs, PoE note).

---

## Part 2 — Build It in Packet Tracer (30 pts)

Create the topology in Packet Tracer and save the `.pkt` file as `Lab4/maple-main-wlan.pkt`.

### Required components

- 1 × router or L3 device for inter-VLAN routing / upstream (Packet Tracer's `Wireless Router` or a router + switch works)
- 1 × switch (e.g., 2960) connecting APs; ports configured appropriately
- ≥ 2 × Access Points (e.g., `AP-PT` / Access Point devices) covering the two zones
- End devices: laptop w/ wireless NIC (Linksys WPC300N module or wireless device), POS/PC, printer
- DHCP or static addressing for both VLANs/subnets (document your choice)

### Required configuration

| Item | Requirement |
|---|---|
| VLANs | At least 2: `10` STAFF, `20` GUEST (document actual IDs used) |
| SSIDs | `MM-Staff` and `MM-Guest` mapped to their VLANs/subnets |
| Security | Staff: WPA2-PSK or WPA2-Enterprise if your PT version supports it. Guest: WPA2-PSK (or open+captive note) |
| Segmentation | Guest clients must not reach staff devices — implement via ACL, separate subnet + no inter-VLAN route, or PT firewall rules; document method |
| Channels | Each AP on a distinct non-overlapping 2.4 GHz channel (or 5 GHz if available); set explicitly, not auto |
| DHCP | Working address assignment on both segments (or static + justification) |

### Validation tests (must all be evidenced)

1. Guest laptop associates to `MM-Guest` and gets an address — screenshot `ipconfig`.
2. Staff device associates to `MM-Staff` and reaches the printer/other staff device — screenshot successful ping.
3. Guest client ping to a staff/POS address **fails** — screenshot the failed ping (this is the point of the lab — a passing guest→POS ping means segmentation is broken).
4. Show each AP's config page: SSID, channel, security mode.

**Screenshots 1–4:** Topology overview, AP config page, successful staff ping, failed guest→staff ping. (More are welcome.)

---

## Part 3 — Mini Site Survey of Your Own Space (20 pts)

Perform a passive survey of **your own home/dorm** using your Lab 1 scanner:

1. Sketch or describe a floor plan (a simple markdown ASCII diagram or a photo of a hand sketch is fine).
2. From **3 different spots** (e.g., desk, far room, near router), record your AP's RSSI (dBm) and the neighbor networks visible at each spot.
3. In `Lab4/site-survey.md`:
   - Table: location → RSSI → SNR estimate → # neighboring networks → notes
   - What channel is your AP on, and would you move it? Why?
   - Where would you add an AP or move the existing one, and why?
   - One paragraph: how would this passive survey differ from a **predictive** survey for Maple & Main?

**Screenshot 5:** Scanner reading at your weakest spot.

---

## Part 4 — Commit, Push & Pull Request

On the **student upload repo**:

1. Branch: `lab4-<your-username>`
2. Files under `Lab4/`: `design-doc.md`, `maple-main-wlan.pkt`, `site-survey.md`, `screenshots/`
3. Commit: `"Lab 4: WLAN design and site survey"` → push → PR to `main`.

**Screenshot 6:** The open PR showing your `Lab4/` folder.

---

## Part 5 — Video Walkthrough

Record a single `.wmv` video (target 8–12 minutes):

1. Your design doc summary — SSIDs, VLANs, channel plan.
2. The Packet Tracer topology; each AP's config; **live demo of both ping tests** (staff success, guest→POS blocked).
3. Your home survey data and the reasoning you'd apply to Maple & Main's AP placement.
4. Your open PR.

---

## Submission Requirements

| Item | Format | Required |
|------|--------|----------|
| Video walkthrough | `.wmv` / `.mp4` | Yes |
| `design-doc.md`, `site-survey.md` | `.md` | Yes |
| `maple-main-wlan.pkt` | `.pkt` | Yes |
| Screenshots (in `Lab4/screenshots/`) | `.png` / `.jpg` | Yes |
| Pull request link | URL | Yes |

---

## Grading Rubric

| Item | Points |
|---|---|
| Design document (requirements, SSID/VLAN/channel plan) | 15 |
| Packet Tracer build + working segmentation (`.pkt` + evidence) | 30 |
| Site survey + AP placement reasoning | 20 |
| PR + video | 10 |
| **Total** | **75** |

---

## Tips & Resources

- **Packet Tracer:** https://www.netacad.com → download via your free Networking Academy account.
- If your PT version lacks enterprise auth, document that and use WPA2-PSK — the VLAN/segmentation work is what's graded.
- Keep the `.pkt` file under a few MB; don't commit Packet Tracer autosaves.

---

> **Reminder:** All work must be your own. AI-generated work is prohibited per syllabus. Surveys must be of networks you own/are authorized to survey.
