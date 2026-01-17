# Damsel in Distress Dashboard: Standard Operating Procedure (SOP)

**Version:** 1.0  
**Last Updated:** January 17, 2026  
**Repository:** `mukhtadirsyed/damsel-dashboard`

---

## 1. Overview

The **Damsel in Distress Dashboard** is a gamified performance tracking system for the APAC PM Team. It visualizes sprint compliance, velocity, and documentation quality using a fantasy kingdom metaphor.

**Core Philosophy:** 
- **Lower Score = Better Performance** (Distress Score).
- **Compliance is King:** Velocity matters, but following team laws (deadlines & documentation) is non-negotiable.
- **Theme:** "Enchanted Realm" (Castles, Dragons, Scrolls).

---

## 2. The Royal Decree (Scoring Methodology)

Scores are calculated per person based on their active Sprint and Operations tasks.

### A. The Laws

#### ⏰ Law I: Accountable Deadlines (+5 pts per violation)
Deadlines must be respected.
- **Late Completion:** Task resolved *after* due date → **+5 pts**
- **Overdue:** Task unresolved and due date passed → **+5 pts**
- **Unjustified Move:** Deadline moved without comment/reason → **+5 pts**
- **Early Bonus:** Task resolved *before* due date → **-5 pts** (Reward!)

#### 📜 Law II: Documentation Quality (+5 pts per violation)
Jira is the single source of truth.
- **Missing Description:** Empty or too short (<50 chars) → **+5 pts**
- **Silent Progress:** Status is "In Progress" but 0 comments → **+5 pts**
- **Stale Task:** No updates (comments/status change) for 5+ days → **+5 pts**
- **Silent Closure:** Moved to "Done" without a closure comment → **+5 pts**

#### 🚀 Velocity Score
Base score determined by completion rate.
- **Formula:** `100 - (Completion Rate %)`
- Example: 80% completion = 20 pts base score.
- Example: 0% completion = 100 pts base score.

---

### B. Distress Levels (Badges)

The Final Distress Score maps to one of 5 tiers. **Do not change these thresholds.**

| Range | Title | Emoji | Theme Color | Description |
|-------|-------|-------|-------------|-------------|
| **0-25** | **Happily Ever After** | ✨ | Emerald (Safe) | Perfect harmony. The kingdom is safe. |
| **26-40** | **Frog Kisser** | 🐸 | Lime (Frog) | A few warts, but manageable. |
| **41-60** | **Glass Slipper Missing** | 👠 | Cyan (Glass) | Things are getting chaotic. |
| **61-80** | **Distress Signal Lit** | 🆘 | Amber (Warning) | Send help immediately. |
| **81-100+** | **Total Meltdown** | 🫠 | Rose (Danger) | The dragon has won. |

---

## 3. Audit Process (How to Analyze)

When running a review for a new sprint, follow this data collection process.

### Step 1: Query Jira (Live Data)
Use the Jira MCP or JQL to fetch tasks.

**Sprint Tasks Query:**
```jql
project = EDO AND sprint in openSprints()
```

**Ops Tasks Query (for Khushi/Ops Lead):**
```jql
project = EDO AND component = "APAC Domain Operations" AND status != Done
```

### Step 2: Analyze Each Task
For every task found, check:
1.  **Dates:** `duedate` vs `resolutiondate` (Law I).
2.  **Activity:** `updated` date vs Today (Law II - Staleness).
3.  **Content:** `description` length and `comment` count (Law II).

### Step 3: Calculate Individual Scores
1.  **Velocity:** `(Done Tasks / Total Tasks) * 100`. Then `100 - Result`.
2.  **Penalties:** Sum all Law I and Law II points.
3.  **Final Score:** Velocity + Penalties.
4.  **Rank:** Sort by Final Score (Lowest is #1).

---

## 4. Dashboard Architecture

The dashboard is a static React app hosted on GitHub Pages.

### File Structure
```
/ (Root)
├── index.html          # HOME PAGE (Archive List)
└── sprint-1/
    └── index.html      # REPORT PAGE (Detailed View)
└── sprint-2/           # Future sprints go here...
    └── index.html
```

### Creating a New Sprint Report
1.  Copy the latest sprint report file (e.g., `sprint-1/index.html`).
2.  Create a new folder (e.g., `sprint-2/`).
3.  Paste the file as `sprint-2/index.html`.
4.  **Edit the Data:** Find the `TEAM_DATA` constant in the script and update it with the new audit results.
5.  **Edit the Date:** Update `REPORT_DATE` and `SPRINT_NAME`.

### Updating the Home Page
1.  Edit the root `index.html`.
2.  Find the `SPRINT_ARCHIVE` constant.
3.  Add the new sprint to the array:
    ```javascript
    {
      id: "sprint-2",
      name: "Sprint 2 Review",
      date: "January 31, 2026",
      winner: "Name",
      winnerScore: 25,
      avgScore: 50,
      link: "sprint-2/index.html"
    }
    ```

---

## 5. Deployment Checklist

1.  **Verify Local Files:**
    - Open `index.html` in browser → Check links work.
    - Open new sprint report → Check scores and badges match the SOP.

2.  **Push to GitHub:**
    ```bash
    git add .
    git commit -m "feat: Add Sprint X Review"
    git push
    ```

3.  **Verify Live Site:**
    - Go to `https://mukhtadirsyed.github.io/damsel-dashboard/`
    - Check that the new sprint appears in the archive.
    - Click through and verify the report data.

---

## 6. Theme Reference (Enchanted Realm)

**Colors (Tailwind):**
- Background: `parchment-50` (#fdfbf7)
- Safe: `emerald-500` (Green)
- Warning: `amber-500` (Gold/Orange)
- Danger: `rose-500` (Red)

**Fonts:**
- Headings: `Cinzel` (Serif, Royal feel)
- Accents: `Dancing Script` (Handwritten)
- Body: `Inter` (Clean sans-serif)

**Icons:**
- 🏰 Castle (Sprint/Home)
- 🛡️ Shield (Safe)
- 👑 Crown (Winner)
- 🐉 Dragon (Danger)
