# Midterm Exam — IT 375 Wireless Network Administration

**Course:** IT 375 — Wireless Network Administration
**Week:** 9 — Midterm Review & Midterm Exam
**Points:** 200 points
**Format:** Hands-on practical exam + video walkthrough (no in-class written portion)
**Coverage:** Weeks 1–8

> **Objectives & Outcomes:** Refer to the course syllabus for course objectives and grading criteria.
>
> This exam is a comprehensive, hands-on assessment of everything covered from Weeks 1–8 of the course. You will analyze, secure, and document a small wireless deployment end-to-end in a single GitHub repository.

---

## Exam at a Glance

You will build a single **midterm** GitHub repository — `it375-midterm-<your-username>` — containing the full wireless administration story: RF/channel analysis, frame inspection, security posture, and a validated WLAN design.

| Section | Points | Coverage Week | Slide Deck Reference |
|---|---|---|---|
| 1. RF & Channel Plan Worksheet | 25 | Week 3 | RF Fundamentals & 802.11 Standards |
| 2. 802.11 Frame Analysis (Wireshark) | 30 | Week 4 | Frames & Wireless Packet Analysis |
| 3. WPA2/WPA3 Security Analysis | 30 | Week 6 | Wireless Security Fundamentals |
| 4. Rogue AP / Posture Audit Report | 25 | Weeks 6–7 | Wireless Security I & II |
| 5. Packet Tracer WLAN Build (Multi-SSID, Segmented) | 40 | Weeks 5, 7, 8 | Architectures · 802.1X · Site Surveys |
| 6. Site Survey & Channel-Plan Justification | 20 | Week 8 | Site Surveys & WLAN Design |
| 7. `MIDTERM.md` — Written Analysis | 15 | Weeks 1–8 | All weeks |
| 8. Video Walkthrough (`.wmv`) | 15 | — | — |
| **Total** | **200** | | |

---

## Rules

- Individual work only. AI-generated work is **prohibited** per syllabus.
- All artifacts must be committed and pushed to a **single midterm repo** on your GitHub account: `it375-midterm-<your-username>`.
- Analysis must use **provided captures or your own network only** — no live capture/injection/cracking against any network (see syllabus Wireless Lab Ethics).
- Submit the repo URL + `.wmv` walkthrough to Canvas by the due date shown in Canvas.

> **CRITICAL SECURITY WARNING:** The midterm repository will be **public**. Never commit personal information, API keys, passwords, credentials, full MAC addresses tied to your real household, or any sensitive data. Partially mask your own BSSID/MACs (e.g., `AA:BB:CC:*:*:*`) and your home SSID if you prefer (e.g., `HomeNet-<redacted>`). GitHub's Secret scanning will help, but you must remain vigilant.

**Git Operations:** You may use either GitHub Desktop (preferred) or command line for all git operations. See [Git Workflow Guide](../git-workflow-guide.md).

**Repository Setup:** Create a public GitHub repo named `it375-midterm-<your-username>` and enable **Dependabot alerts**, **Dependency graph**, and **Secret scanning + push protection** under *Settings → Code security and analysis*.

---

## Section 1 — RF & Channel Plan Worksheet (25 pts)

Reference: Week 3 RF Fundamentals & 802.11 Standards.

Answer in `midterm/section1/rf-plan.md` for a fictional **3-AP office deployment**:

1. Assign 2.4 GHz channels to 3 APs covering a single-floor office. Justify the reuse plan and name the interference class you'd be creating if two APs shared channel 8.
2. Given a client at -70 dBm and a noise floor of -95 dBm, compute the SNR and state whether it's suitable for reliable voice-over-Wi-Fi (state your threshold assumption).
3. Explain DFS channels in 5 GHz: one benefit, one operational risk (radar event behavior), and whether you'd use them here.
4. For each scenario pick a band (2.4 / 5 / 6 GHz) and defend the choice in one sentence each: (a) warehouse barcode scanners 60 m from the AP, (b) video editing suite with 8 workstations, (c) IoT sensors that only ship with 2.4 GHz radios.
5. dB/dBm/dBi: define each in one sentence and use each correctly in a sentence about this deployment.

**Deliverables in `midterm/section1/`:** `rf-plan.md`, `screenshots/` as needed.

---

## Section 2 — 802.11 Frame Analysis (30 pts)

Reference: Week 4 Frames & Wireless Packet Analysis. Use the provided captures (`Midterm/captures/` or Wireshark sample captures — note which).

Required in `midterm/section2/frame-analysis.md`:

1. Full connection sequence for one client: probe → auth → assoc → EAPOL handshake (if present), with frame timestamps/deltas.
2. Table of **at least 8 distinct management subtypes** found in the capture with a one-line purpose for each.
3. Extract from a beacon: SSID, BSSID, channel, supported rates, RSN/capabilities (cipher suite + auth type advertised).
4. Radiotap metadata on two frames at different locations in the capture: signal (dBm), channel, data rate — comment on what changed.
5. Explain in 4–6 sentences how an admin would use this same capture-based workflow to diagnose a "clients associate but can't reach the network" ticket.

**Deliverables in `midterm/section2/`:** `frame-analysis.md`, `screenshots/` (min. 4: beacon detail, assoc sequence, radiotap, subtype filter output).

---

## Section 3 — WPA2/WPA3 Security Analysis (30 pts)

Reference: Week 6 Wireless Security Fundamentals. Analysis-only — provided captures.

Required in `midterm/section3/security-analysis.md`:

1. Dissect a provided 4-way handshake: label Messages 1–4, direction, and the material each carries (ANonce/SNonce/MIC/GTK).
2. Explain the PTK derivation inputs and why a captured handshake enables an **offline** guessing attack (concept only — no tooling).
3. Contrast WPA2-PSK vs. WPA3-SAE in this capture/context: what does SAE fix about offline guessing and forward secrecy?
4. Identify the cipher suite and auth suite advertised in the beacon (CCMP/GCMP, PSK/SAE) and whether PMF is required/capable/disabled — explain the deauth risk if PMF is off.
5. List the KRACK lesson in 2–3 sentences: what implementation detail did it abuse?

**Deliverables in `midterm/section3/`:** `security-analysis.md`, `screenshots/` (min. 3: handshake sequence, EAPOL detail, beacon RSN element).

---

## Section 4 — Rogue AP / Posture Audit Report (25 pts)

Reference: Weeks 6–7. Use your own environment scan + provided beacon data.

Write `midterm/section4/rogue-report.md` as a **consultant-style audit report** (1–2 pages):

1. **Methodology:** tools used, where/when you scanned, scope (your own network + provided captures only).
2. **Inventory table:** SSID (redact yours), masked BSSID, band, channel, security, OUI vendor.
3. **Findings:** minimum 4 findings ranked High/Med/Low — e.g., legacy security advertised, WPS enabled, congested channel, no PMF, rogue-suspect SSID pattern. Each finding: evidence + risk + remediation.
4. **Rogue-detection procedure:** the fingerprint fields you'd baseline (BSSID, channel, RSN caps, signal profile) and how a WIPS automates it.
5. **Policy recommendation paragraph** for the business.

**Deliverables in `midterm/section4/`:** `rogue-report.md`, `screenshots/` (min. 2).

---

## Section 5 — Packet Tracer WLAN Build (40 pts)

Reference: Weeks 5, 7, 8 — Architectures, 802.1X concepts, design.

Scenario: **Riverside Dental Clinic** — three zones: Reception/Waiting (guest), Operatories (staff), Records office (HIPAA-sensitive staff). Save as `midterm/section5/riverside-wlan.pkt`.

Required:

1. ≥ 3 APs on non-overlapping channels; 3 SSIDs: `RD-Guest`, `RD-Staff`, `RD-Records`.
2. ≥ 3 VLANs/subnets mapped 1:1 to SSIDs; trunked to a switch and routed.
3. Segmentation: Guest reaches internet (or upstream) only; cannot ping Staff/Records. Records reachable only from Records devices. **Evidence: successful in-zone ping + failed cross-zone ping screenshots.**
4. Security per SSID: strongest your Packet Tracer supports (WPA2-PSK acceptable; if WPA2-Enterprise/802.1X is available in your PT version, configure it on `RD-Records` and screenshot it).
5. DHCP on all segments (or documented static plan).
6. `midterm/section5/config-notes.md` — short doc listing every non-default config you applied and why.

**Deliverables in `midterm/section5/`:** `riverside-wlan.pkt`, `config-notes.md`, `screenshots/` (min. 5: topology, AP configs ×2, successful ping, blocked ping).

---

## Section 6 — Site Survey & Channel Plan Justification (20 pts)

Reference: Week 8. Passive survey of your own space + predictive reasoning for the clinic.

Write `midterm/section6/survey-and-plan.md`:

1. Your own-space survey: 3 measurement spots, RSSI, neighbor count, channel congestion (table).
2. Predictive plan for Riverside Dental: AP placement reasoning for 3 zones, assumed coverage radius, capacity estimate (guests + staff devices), and the channel plan you'd deploy — defend it using your Section 1 RF work.
3. Explain which one thing in a real predictive survey tool (e.g., building materials/attenuation) your Packet Tracer build can't model, and how it would change your AP count.

**Deliverables in `midterm/section6/`:** `survey-and-plan.md`, `screenshots/` (min. 1).

---

## Section 7 — `MIDTERM.md` Written Analysis (15 pts)

Write `midterm/MIDTERM.md` at the repo root:

1. **What this repo is** (one paragraph).
2. **Cross-domain summary** — how RF, frames, security, and design connect in one end-to-end wireless admin workflow (½ page).
3. **Wireless threat model** — top 5 threats to the Riverside clinic WLAN (rogue AP, deauth, weak PSK, lateral movement via guest, eavesdropping) and which of your controls/sections addresses each.
4. **Learning-objective crosswalk** — map Sections 1–6 to at least five course objectives from the syllabus.

---

## Section 8 — Video Walkthrough (15 pts)

Record a **single** `.wmv` video (target 10–15 minutes) demonstrating:

- Repo layout and security settings enabled
- Your frame-analysis highlights in Wireshark (filters live)
- The handshake dissection
- Packet Tracer: topology tour + **live** successful and blocked pings
- Your audit report findings
- A skim of `MIDTERM.md`

> **Critical:** Export as `.wmv`. Any other format cannot be graded.

---

## Submission Requirements

| Item | Format | Required |
|------|--------|----------|
| Midterm GitHub repo URL (`it375-midterm-<username>`) | URL | Yes |
| Video walkthrough | `.wmv` | Yes |

Grading is done against the **pushed contents of your repo** at the due-date timestamp on Canvas. Local files that are not pushed will not be graded.

---

## Grading Rubric

| Section | Excellent (all pts) | Partial | Zero |
|---|---|---|---|
| 1. RF plan (25) | All 5 answers correct & justified | Partially correct | Missing |
| 2. Frame analysis (30) | Full sequence, 8 subtypes, beacon + radiotap detail | Missing pieces | No real analysis |
| 3. Security analysis (30) | Handshake labeled, WPA3 contrast, PMF/KRACK explained | Missing pieces | Missing |
| 4. Audit report (30→25) | 4+ findings with evidence + remediation + rogue procedure | Fewer findings/thin evidence | Missing |
| 5. PT build (40) | All SSIDs/VLANs work; guest blocked; evidence included | Segmentation partial | No working build |
| 6. Survey + plan (20) | Real data + defensible clinic plan | Missing one | Missing |
| 7. MIDTERM.md (15) | Complete w/ crosswalk | Missing crosswalk | Missing |
| 8. Video (15) | Covers everything | Short/missing pieces | No video |

---

## Tips

- Redact/mask your own SSID and BSSIDs before pushing — practice real opsec.
- Grading looks at pushed files at the due timestamp; push early and often.

---

> **Reminder:** All work must be your own. AI-generated work is prohibited per syllabus. Analysis of provided captures or your own network only.
