# Final Exam & Project — IT 375 Wireless Network Administration

**Course:** IT 375 — Wireless Network Administration
**Weeks:** 14 — End-to-End Review · 15 — Final Project Work · 16 — Final Exam
**Points:** 300 points
**Format:** Comprehensive hands-on capstone repository + video walkthrough
**Coverage:** Weeks 1–16 (entire course)

> **Objectives & Outcomes:** Refer to the course syllabus for course objectives and grading criteria.
>
> This capstone integrates **every** skill covered in the course — RF fundamentals, 802.11 frame analysis, WLAN architecture, wireless security (WPA2/WPA3/802.1X), site survey and design, troubleshooting, enterprise management, and wireless security operations — into a single enterprise WLAN design-and-defend deliverable.

---

## Final at a Glance

You will build **one** capstone GitHub repository — `it375-final-<your-username>` — that designs, secures, simulates, and defends a complete enterprise wireless deployment, plus your **Wireless Design / Open-Source Project** work carried through Weeks 7–15 of the syllabus.

| Section | Points | Coverage | Slide Deck Reference |
|---|---|---|---|
| 1. Enterprise WLAN Design Document | 30 | Weeks 3, 5, 8, 12 | RF · Architectures · Site Surveys · Advanced Topics |
| 2. Packet Tracer Build (Controller-Style, Segmented, Roaming) | 45 | Weeks 5, 7, 8 | Architectures · 802.1X · Surveys |
| 3. Wireless Security Architecture (802.1X/WPA3/Policy) | 35 | Weeks 6, 7, 13 | Security I & II · Sec Ops |
| 4. RF Analysis & Frame Evidence Pack | 30 | Weeks 3, 4 | RF Fundamentals · Frame Analysis |
| 5. Site Survey Report (Passive + Predictive) | 30 | Week 8, 10 | Surveys · Troubleshooting |
| 6. Troubleshooting & Operations Runbook | 30 | Weeks 10, 11, 13 | Troubleshooting · Enterprise Mgmt · Sec Ops |
| 7. Wireless Design / Open-Source Project Integration | 30 | Weeks 7–15 | Project weeks |
| 8. `FINAL.md` — Course-Wide Analysis | 30 | Weeks 1–16 | All weeks |
| 9. `.wmv` Video Walkthrough (20–30 min) | 30 | — | — |
| 10. Reproducibility (grader can open/run everything) | 10 | Weeks 5, 8 | Architectures · Surveys |
| **Total** | **300** | | |

---

## Scenario

**Huron Valley Community College — "Harbor Campus" expansion.** A new 2-story wing:

- **Floor 1:** Welcome center + 4 classrooms (each seats 30), open study commons
- **Floor 2:** 6 classrooms + faculty offices + a small server/IDF room
- **Users:** ~800 concurrent devices peak (student BYOD heavy), staff laptops, IP phones over Wi-Fi, IoT door locks and cameras on a dedicated segment, guest visitors
- **Requirements:** Campus-wide SSIDs (`HVCC-Student`, `HVCC-Staff`, `HVCC-IoT`, `HVCC-Guest`), eduroam-style enterprise auth on staff/student (WPA2/WPA3-Enterprise conceptually; PSK acceptable where PT limits apply), full guest isolation, voice-friendly SNR in classrooms, documented channel plan, monitoring/logging posture, and an operations runbook.

---

## Rules

- Individual work only. AI-generated work is **prohibited** per syllabus.
- Everything lives in a single public GitHub repo: `it375-final-<your-username>`.
- Analysis uses provided captures or your own network only (syllabus Wireless Lab Ethics).
- Final submission = **Repo URL + `.wmv` walkthrough + single-slide summary PDF** to Canvas by the due date.

> **CRITICAL SECURITY WARNING:** The final repository will be **public**. Never commit personal information, passwords, real credentials, or precise identifying details of your home network. Mask BSSIDs/SSIDs where appropriate. This is a professional portfolio piece — do not compromise it by leaking sensitive information.

**Git Operations:** You may use either GitHub Desktop (preferred) or command line. See [Git Workflow Guide](../git-workflow-guide.md).

**Repository Setup:** Create a public repo `it375-final-<your-username>`; enable **Dependabot alerts**, **Dependency graph**, and **Secret scanning + push protection** under *Settings → Code security and analysis*.

---

## Section 1 — Enterprise WLAN Design Document (30 pts)

Reference: Weeks 3, 5, 8, 12.

Write `final/section1/design-doc.md` (2–3 pages):

1. **Requirements table** — business need → technical requirement → where it's proven in your repo.
2. **SSID/VLAN/security matrix** — all four SSIDs: VLAN ID, subnet plan, auth method per SSID, band steering approach.
3. **AP count & placement** — floor-by-floor AP plan with capacity math (device density per classroom, airtime assumptions).
4. **Channel plan** — 2.4 GHz reuse plan + 5/6 GHz plan including whether you use DFS channels and why.
5. **Wired underlay** — AP trunking, PoE budget assumptions, uplink sizing, controller/cloud-management choice (justify autonomous vs. controller vs. cloud for this org).
6. **Assumptions & risks** — what your simulation can't model (building materials, real RF behavior) and how a real survey would adjust the design.

**Deliverables:** `final/section1/design-doc.md`, `screenshots/` or diagrams as needed.

---

## Section 2 — Packet Tracer Build (45 pts)

Reference: Weeks 5, 7, 8. Save as `final/section2/hvcc-harbor-wlan.pkt`.

Required:

1. ≥ 4 APs representing the two-floor deployment, all on a defensible non-overlapping channel plan.
2. All four SSIDs (`HVCC-Student`, `HVCC-Staff`, `HVCC-IoT`, `HVCC-Guest`) mapped to distinct VLANs/subnets, trunked to switching, routed by an L3 device.
3. **Segmentation evidence:**
   - Guest can reach upstream/DHCP but **cannot** ping student/staff/IoT devices (screenshot failed ping).
   - IoT devices isolated to their segment; only the NVR/admin host (or documented equivalent) can reach them.
   - Staff and Student appropriately reachable per your design doc.
4. Security set per SSID: strongest PT supports (document any platform limits; if WPA2-Enterprise exists in your version, use it on Staff).
5. DHCP working on all four segments.
6. A second-AP **roaming demonstration** concept: same SSID on two APs, document expected client behavior in `final/section2/roaming-notes.md`.
7. `final/section2/config-notes.md` — every non-default config and why.

**Deliverables:** `hvcc-harbor-wlan.pkt`, `config-notes.md`, `roaming-notes.md`, `screenshots/` (min. 8: topology, per-SSID configs, successful pings, blocked guest ping, DHCP lease).

---

## Section 3 — Wireless Security Architecture (35 pts)

Reference: Weeks 6, 7, 13.

`final/section3/security-architecture.md` plus artifacts:

1. **802.1X/EAP plan** — chosen EAP method per population (EAP-TLS for staff devices, PEAP vs. TTLS trade-offs for BYOD students), RADIUS placement, certificate strategy, and onboarding flow for BYOD.
2. **PSK governance** — rotation policy for IoT SSID, minimum length, storage/handling.
3. **PMF & management-plane protections** — PMF posture per SSID, management-SSID avoidance, AP management access controls, WPS prohibited.
4. **WIPS/rogue-detection plan** — what you'd baseline, detection logic, response workflow.
5. **Wireless security policy** (`policy.md`, 1 page) — cipher minimums, guest terms, authorized-testing clause.
6. **Compliance crosswalk** — map your controls to PCI DSS wireless requirements (§4.1.1 rogue scans, encryption) and note which apply vs. don't for a college.

**Deliverables:** `security-architecture.md`, `policy.md`, `compliance-crosswalk.md`, `screenshots/` as needed.

---

## Section 4 — RF Analysis & Frame Evidence Pack (30 pts)

Reference: Weeks 3, 4. Provided captures + your own scan.

`final/section4/` evidence pack:

1. **Frame capture analysis** (`frame-evidence.md`) — full client connection sequence, ≥ 8 management subtypes, beacon RSN extraction, radiotap metadata, as applied to troubleshooting a Harbor Campus ticket (write it as the ticket resolution: "students in Rm 204 connect but drop").
2. **Channel-utilization analysis** — from your own scan data: congestion findings, and how the Harbor plan avoids the same failure modes.
3. **Capacity math** (`capacity-math.md`) — show the arithmetic: per-classroom device count → per-AP load → airtime/throughput estimate at a chosen PHY.

**Deliverables:** `frame-evidence.md`, `capacity-math.md`, `screenshots/` (min. 4).

---

## Section 5 — Site Survey Report (30 pts)

Reference: Weeks 8, 10.

`final/section5/survey-report.md` — a consultant-grade report (2 pages):

1. **Passive survey (your space):** 3-spot measurement table (RSSI, SNR, neighbors), lessons applied to Harbor.
2. **Predictive survey narrative for Harbor:** assumed wall materials/attenuation, coverage-radius assumptions per band, AP count derivation, where you'd validate post-install.
3. **Heatmap description:** text/ASCII floor-plan with AP locations and expected -67 dBm coverage boundaries (a real graphic is a bonus, not required).
4. **Post-deployment validation plan:** what you'd measure after install (active vs. passive checks) and the acceptance criteria (e.g., "≥ -65 dBm and SNR ≥ 25 in every classroom seat").

**Deliverables:** `survey-report.md`, `screenshots/` (min. 2).

---

## Section 6 — Troubleshooting & Operations Runbook (30 pts)

Reference: Weeks 10, 11, 13.

`final/section6/` operations package:

1. **`runbook.md`** — five named incident plays with concrete steps/tools: (a) user can't associate, (b) co-channel interference event, (c) suspected rogue AP, (d) deauth storm symptoms, (e) DHCP exhaustion on guest VLAN.
2. **`monitoring-plan.md`** — what telemetry you'd collect (AP CPU/channel util, client RSSI distribution, auth failures, roams), thresholds that page someone, and where logs flow (syslog/SIEM narrative — no real SIEM required).
3. **`known-issues.md`** — sticky clients, hidden nodes, band-steering edge cases, IoT 2.4 GHz-only quirks — each with detection signal + fix.
4. **Config management** — how controller/cloud config changes are versioned, peer-reviewed, and rolled back.

---

## Section 7 — Wireless Design / Open-Source Project Integration (30 pts)

Reference: Syllabus (200-pt Wireless Design / Open-Source Project) · Weeks 7–15.

Move your semester-long project into `final/section7/opensource-project/`:

- A working subproject demonstrating at least **one** open-source wireless-adjacent tool you integrated or evaluated — e.g., Kismet (detection), Wireshark dissector workflow, hcxtools on *provided/own* captures, Wazuh wireless-log rules, an open-source WLAN planner/heatmap tool, or a Packet Tracer/ZTP-style automation script.
- `README.md`: what the tool is, why you chose it, how to run it, how it plugs into the Harbor deployment.
- Evidence it runs — logs / screenshots / video segment.
- `integration-challenges.md` — what broke, what you fixed.

> The syllabus lists the Wireless Design / Open-Source Project as a separate 200-point deliverable (submitted per Canvas). This section is the *reference/integration copy* included with the Final so the grader can see the whole story end-to-end in one place.

---

## Section 8 — `FINAL.md` (30 pts)

Write `final/FINAL.md` at repo root:

1. **Executive summary** (½ page) — could be read by Harbor's IT director.
2. **Course-wide threat model** — top 10 threats to this WLAN (rogue AP, evil twin, deauth, weak PSK, guest lateral movement, IoT pivot, misconfig, no PMF, DHCP/RF denial, firmware vulns): for each, attacker technique + which control/section stops it.
3. **Wired-vs-wireless comparison table** — physical, MAC, authN, segmentation, monitoring, and residual-risk differences.
4. **Learning-objective crosswalk** — every syllabus objective mapped to the section(s) that prove it.
5. **What you'd build next** — 3–5 improvements for real production (real WIPS, NAC, certificate-based guest, etc.).

---

## Section 9 — `.wmv` Video Walkthrough (30 pts)

Record a **single** `.wmv` video (target 20–30 minutes) covering:

- Design doc tour + capacity math
- Packet Tracer build: SSIDs, VLANs, per-AP channels, **live** successful and blocked pings, DHCP
- Security architecture: 802.1X plan, PMF, WIPS plan, policy highlights
- Frame-evidence walkthrough in Wireshark
- Survey report + runbook highlights
- Open-source project running
- Skim of `FINAL.md`

> **Critical:** Export as `.wmv`. Any other format cannot be graded.

---

## Section 10 — Reproducibility (10 pts)

A grader must be able to clone your repo and open everything without you:

- `final/bootstrap.md` — exact steps: PT version needed, which `.pkt` file, which captures, which tools to install.
- All `.md`, `.pkt`, `.pcap`, screenshots referenced must exist at the paths you cite.
- The Packet Tracer file must open cleanly and demonstrate the ping tests without extra config.

---

## Submission Requirements

| Item | Format | Required |
|------|--------|----------|
| Final repo URL (`it375-final-<username>`) | URL | Yes |
| Video walkthrough | `.wmv` | Yes |
| `summary.pdf` (single-slide executive summary: repo URL, what it is, 3 most important controls, top residual risk) | `.pdf` | Yes |

Grading is done against the pushed contents of your repo at the Canvas due-date timestamp.

---

## Grading Rubric (High-Level)

| Section | Points | Excellent |
|---|---|---|
| 1. Design document | 30 | Requirements→requirements-traceable, defensible capacity & channel plan |
| 2. Packet Tracer build | 45 | All SSIDs/VLANs work, segmentation proven with evidence |
| 3. Security architecture | 35 | Coherent 802.1X/PMF/WIPS/policy story + compliance map |
| 4. RF & frame evidence | 30 | Correct dissection tied to a real troubleshooting narrative |
| 5. Survey report | 30 | Real passive data + defensible predictive plan |
| 6. Runbook & ops | 30 | Concrete, actionable plays + monitoring plan |
| 7. Open-source project | 30 | Working tool, documented integration |
| 8. FINAL.md | 30 | Complete with threat model + crosswalk |
| 9. Video | 30 | Covers everything clearly |
| 10. Reproducibility | 10 | Grader can open and verify without help |

---

> **Reminder:** All work must be your own. AI-generated work is prohibited per syllabus. Analysis of provided captures or your own network only.
