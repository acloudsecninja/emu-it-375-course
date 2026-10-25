# Lab 1 — GitHub Configuration & Wireless Toolkit Setup

**Course:** IT 375 — Wireless Network Administration
**Week:** 2 — GitHub Configuration & Wireless Tooling Setup
**Points:** 75 points
**Submission:** Upload your screen-recording video (`.wmv`) and required files via Canvas

> **Objectives & Outcomes:** Refer to the course syllabus (distributed via the course Google Shared Drive) for course objectives and grading criteria.
>
> This lab aligns to the Week 2 slide deck: *IT375 — Week 2 — GitHub Configuration & Wireless Tooling Setup*.

---

## Lab Overview

Your GitHub environment and wireless toolkit are the foundation for every lab, midterm, and final in IT 375. Before you can submit any work, you must have:

1. A properly configured GitHub.com account (with 2FA).
2. Access to both the **course reference repo** (public) and the **student upload org repo** (private, invite-only).
3. A local Git installation that is verified across Git Bash, PowerShell, and Command Prompt.
4. The wireless toolchain installed: **Wireshark**, **Cisco Packet Tracer**, and one **WLAN scanner**.
5. A working push/pull cycle so your commits are correctly attributed and end up in the right repo.

**Key course repositories:**

| Repo | Purpose | Link |
|------|---------|------|
| Course Reference (public) | Lab instructions, midterm, final, syllabus | https://github.com/acloudsecninja/emu-it-375-course |
| Student Upload (private) | Your assignment submissions via PR | https://github.com/acloudsecninja-emu-org/emu-it-375-fall-2026 |

---

## Prerequisites

Before starting this lab, ensure you have:

- [ ] A working computer running Windows 10/11 (WSL Ubuntu enabled — see the Week 1 Technology Requirements slides)
- [ ] Minimum 8 GB RAM and 30 GB free disk space (16 GB RAM recommended)
- [ ] Screen recording software installed (e.g., https://www.freescreenrecording.com) with `.wmv` export
- [ ] Your EMU email address accessible
- [ ] You have received (and accepted) the GitHub organization invite from Professor Weber

> **IMPORTANT:** Never commit personal information, API keys, passwords, or any sensitive data to any GitHub repository. All work submitted to public repositories is visible to anyone on the internet. Use environment variables or GitHub Secrets for sensitive configuration.

> Linux (Ubuntu) and macOS are supported but instructor support may be limited. See Week 1 slides.

---

## Git Operations: Two Methods Available

**Method 1: GitHub Desktop (Recommended for most students)**
- Visual interface for all Git operations
- Easier pull request management
- Built-in conflict resolution
- See [Git Workflow Guide](../git-workflow-guide.md) for detailed steps

**Method 2: Command Line (Alternative)**
- Traditional terminal-based Git operations
- Full control via commands
- Command examples provided throughout this lab

You can switch between methods anytime — both work with the same repositories.

---

## Part 1 — Install & Verify Git

### 1A — Install Git for Windows

#### GitHub Desktop (Preferred)
1. Download GitHub Desktop from https://desktop.github.com/
2. Run the installer and follow the prompts
3. Launch GitHub Desktop and sign in with your GitHub account
4. Configure your Git identity in **Options → Git**
5. Git for Windows is installed automatically with GitHub Desktop

#### Command Line (Alternative)
1. Navigate to https://git-scm.com/download/win — the 64-bit installer downloads automatically.
2. Run the installer. Recommended settings:
   - **Adjusting PATH:** *Git from the command line and also from 3rd-party software*
   - **HTTPS transport:** *Use the OpenSSL library*
   - **Line endings:** *Checkout Windows-style, commit Unix-style*
   - **Default branch name:** *Override the default* → `main`
   - Leave the remaining defaults (file system caching + Git Credential Manager enabled)

### 1B — Validate Git in Three Windows Shells

Open each of the following and run `git --version`:

```bash
# Git Bash
git --version
```

```powershell
# PowerShell (Win + X → Windows PowerShell)
git --version
```

```cmd
:: Command Prompt (Win + R → cmd)
git --version
```

**Validation Check:** All three shells should return a version number (e.g., `git version 2.45.0.windows.1`). If PowerShell or CMD does not recognize `git`, re-run the installer and select the correct PATH option.

**Screenshot 1:** All three shells side-by-side showing `git --version` output (or GitHub Desktop About screen).

---

## Part 2 — Create & Harden Your GitHub.com Account

1. Sign up at https://github.com with your **EMU email** (`@emich.edu`).
2. Choose a **professional username** — avoid random numbers or nicknames (this may appear on your resume).
3. Verify your email address.
4. **Enable Two-Factor Authentication (2FA)** immediately — required for organization membership.
   - `Settings → Password and Authentication → Enable 2FA` (use an authenticator app).
5. Accept the org invite you received from Professor Weber (check both your EMU email and the email tied to your GitHub account).

**Screenshot 2:** Your GitHub profile page showing your professional username.
**Screenshot 3:** `Settings → Password and Authentication` showing 2FA is **enabled**.

---

## Part 3 — Configure Your Local Git Identity

### GitHub Desktop
1. Open GitHub Desktop → **File → Options → Git**
2. Set **Name:** Your Full Name and **Email:** yourname@emich.edu
3. Click **Save**

### Command Line
```bash
git config --global user.name "Your Full Name"
git config --global user.email "yourname@emich.edu"
git config --global init.defaultBranch main
git config --list
```

**Screenshot 4:** Full `git config --list` output (or GitHub Desktop Git settings screen) showing `user.name` and `user.email`.

---

## Part 4 — Install the Wireless Toolkit

Install each tool and capture evidence that it runs.

### 4A — Wireshark

1. Download Wireshark from https://www.wireshark.org/download.html (Windows x64 installer).
2. Install with defaults (Npcap is installed automatically — required for capture).
3. Launch Wireshark and confirm your interfaces appear in the capture list.

```powershell
# Optional CLI verification:
"C:\Program Files\Wireshark\tshark.exe" --version
```

**Screenshot 5:** Wireshark main window showing available capture interfaces.

### 4B — Cisco Packet Tracer

1. Create a free account at https://www.netacad.com (Cisco Networking Academy).
2. Download Packet Tracer for Windows and install.
3. Launch it, sign in, and open a blank workspace.

**Screenshot 6:** Packet Tracer open with a blank workspace (or the WRT300N / Access Point device dragged onto the canvas).

### 4C — WLAN Scanner (pick one)

Install **one** of the following free tools:

- **WiFi Analyzer** (Microsoft Store — simplest on Windows)
- **NetSpot** (free tier — https://www.netspotapp.com)
- **inSSIDer** (if available to you)
- **Acrylic Wi-Fi Home** (free tier)
- On WSL/Linux: `iw list`, `nmcli dev wifi`, or `wavemon`

Run a scan of **your own** wireless environment. Record: visible SSIDs, channels used, bands (2.4/5/6 GHz), and approximate signal strengths.

**Screenshot 7:** Your WLAN scanner output showing discovered SSIDs and channels.

> **Legal note:** Scanning and listing broadcast SSIDs of nearby networks with these tools is standard practice, but all *hands-on* exercises in this course (association, capture, injection) must be performed only on networks you own or have written permission to test. See the syllabus Wireless Lab Ethics section.

---

## Part 5 — Clone Both Course Repositories

### 5A — Clone the public reference repo

#### GitHub Desktop
1. **File → Clone Repository → URL tab**
2. Enter: `https://github.com/acloudsecninja/emu-it-375-course.git`
3. Choose a local path and click **Clone**

#### Command Line
```bash
git clone https://github.com/acloudsecninja/emu-it-375-course.git
cd emu-it-375-course
```

### 5B — Clone the private student upload repo

#### GitHub Desktop
1. **File → Clone Repository → URL tab**
2. Enter: `https://github.com/acloudsecninja-emu-org/emu-it-375-fall-2026.git`
3. Choose a local path and click **Clone**

#### Command Line
```bash
git clone https://github.com/acloudsecninja-emu-org/emu-it-375-fall-2026.git
cd emu-it-375-fall-2026
git remote -v
```

Expected output:

```
origin  https://github.com/acloudsecninja-emu-org/emu-it-375-fall-2026.git (fetch)
origin  https://github.com/acloudsecninja-emu-org/emu-it-375-fall-2026.git (push)
```

**Screenshot 8:** Both cloned directories listed on disk plus the remote information (`git remote -v` output or GitHub Desktop Repository Settings).

---

## Part 6 — First Branch, Commit & Pull Request

Perform these steps on the **student upload repo**:

1. Create a branch named `lab1-<your-username>` (GitHub Desktop: **Current Branch → New Branch**; CLI: `git checkout -b lab1-<your-username>`).
2. Create a `Lab1/README.md` file answering in 3–5 sentences: *"What wireless network do you use daily, and what would you guess is its biggest weakness?"*
3. Commit with the message `"Lab 1: toolkit setup and initial submission"` and push your branch.
4. Open a **pull request** from your branch back to `main`.

**Screenshot 9:** The open pull request page in GitHub showing your `Lab1/` folder.

> Never push directly to `main`. Always work on a branch and open a PR — this is a course-graded workflow.

---

## Part 7 — Push Your Lab 1 Screenshots

Commit all screenshots to `Lab1/screenshots/` in the student upload repo on your branch. Name them descriptively:

- `01-git-versions-three-shells.png`
- `02-github-profile.png`
- `03-2fa-enabled.png`
- `04-git-config-list.png`
- `05-wireshark-interfaces.png`
- `06-packet-tracer.png`
- `07-wlan-scan.png`
- `08-both-repos-cloned.png`
- `09-pull-request-opened.png`

**Validation Check:** Navigate to your PR on GitHub — every screenshot should be visible in `Lab1/screenshots/`.

---

## Part 8 — Record Your Video Walkthrough

Using your screen recording software, record a single video demonstrating:

1. `git --version` succeeding in Git Bash, PowerShell, and CMD.
2. Your GitHub profile with 2FA enabled.
3. `git config --list` showing your correct name and EMU email.
4. Wireshark, Packet Tracer, and your WLAN scanner each launching and running.
5. Your WLAN scan results — name the bands and channels you observed.
6. Both course repos cloned locally with `git remote -v` verified.
7. Your open pull request in the student upload repo with `Lab1/` visible.
8. A short verbal walkthrough of what each step accomplished.

> **Critical:** Export your video in `.wmv` (or `.mp4`) format. Files in any other format cannot be graded.

---

## Submission Requirements

Submit the following to Canvas by the due date:

| Item | Format | Required |
|------|--------|----------|
| Video walkthrough | `.wmv` / `.mp4` | Yes |
| Screenshots (in `Lab1/screenshots/`) | `.png` / `.jpg` | Yes |
| `Lab1/README.md` written response | `.md` | Yes |
| Pull request link (student upload repo) | URL | Yes |

---

## Tips & Resources

- **Git Workflow Guide:** [git-workflow-guide.md](../git-workflow-guide.md)
- **Course syllabus (source of truth for policies):** distributed via the course Google Shared Drive and Canvas
- **Course reference repo:** https://github.com/acloudsecninja/emu-it-375-course
- **Student upload repo:** https://github.com/acloudsecninja-emu-org/emu-it-375-fall-2026
- **Wireshark docs:** https://www.wireshark.org/docs/
- **Git cheat sheet:** https://education.github.com/git-cheat-sheet-education.pdf
- Stuck? Post in the EMU IA Slack or email Professor Weber. Do not wait until class.

---

> **Reminder:** All work must be your own. AI-generated work is prohibited per syllabus. Refer to the syllabus for full course policies.
