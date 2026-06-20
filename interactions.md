# Interaction Log

## 2026-06-19

**Request:** Create a Marp presentation about 44Net Connect and WireGuard tunnels on Debian, including Windows setup and EchoLink-L/R node configuration for VHF repeater connectivity.

**Actions:**
- Created CLAUDE.md with logging instructions
- Created interactions.md (this file)
- Created Marp presentation: 44net-echolink-presentation.md

**Files Created:**
- CLAUDE.md
- interactions.md
- 44net-echolink-presentation.md

---

## 2026-06-19 (2)

**Request:** Reminder to track all interactions in interactions.md

**Actions:**
- Confirmed interactions.md exists and is being maintained
- Added this log entry

**Outcome:** Interaction logging confirmed active

---

## 2026-06-19 (3)

**Request:** Set up GitHub Pages to automatically update static website on file changes

**Actions:**
- Created `.github/workflows/deploy-pages.yml`
- Workflow triggers on push to main branch
- Uses Marp CLI to convert presentation markdown to HTML
- Deploys output to GitHub Pages

**Files Created:**
- .github/workflows/deploy-pages.yml

**Notes:** After committing, enable GitHub Pages in repo settings (Settings > Pages > Source: GitHub Actions)
