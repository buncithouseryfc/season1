# BHFC site — automated updates from Google Sheets

This connects your master sheet directly to the website. Once set up, updating scores is one click from inside the Sheet — no more exporting CSVs or re-uploading `index.html`.

## How it works

The website is split into two files:

- **`index.html`** — the shell and all the logic. You upload this once and rarely touch it again.
- **`data.json`** — just the scores. This is what changes every week.

An Apps Script bound to your Google Sheet reads the scores, builds `data.json`, and commits it straight to your GitHub repo. GitHub Pages serves it, and the page loads it fresh on every visit.

The page also computes **biggest movers** (using last week's published totals as the comparison) and shows any **announcements** you add.

## One-time setup

### 1. Put both files in your repo

Upload `index.html` **and** `data.json` to the root of your GitHub Pages repo. They must sit next to each other. Push. Your existing site keeps working exactly as before — it just now reads its data from `data.json`.

### 2. Create a GitHub token

The script needs permission to write `data.json` to your repo.

1. On GitHub: **Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token**.
2. **Repository access:** Only select repositories → pick your site repo.
3. **Permissions:** Repository permissions → **Contents → Read and write**.
4. Generate and copy the token (starts with `github_pat_...`). You won't see it again.

### 3. Add the script to your Sheet

1. Open the master sheet → **Extensions → Apps Script**.
2. Delete any placeholder code, paste in the contents of `Code.gs`, and save.
3. At the top of `Code.gs`, fill in the `CONFIG` block:
   - `GITHUB_OWNER` — your GitHub username
   - `GITHUB_REPO` — the repo name hosting the site
   - `GITHUB_BRANCH` — `main` or `master`
   - `SCORES_SHEET` — the exact tab name with the scores (e.g. `Season 1`)
4. Store the token: **Project Settings (gear icon) → Script Properties → Add script property**. Name it exactly `GITHUB_TOKEN`, paste the token as the value, save.

### 4. Publish

1. Reload the Google Sheet. A new **BHFC** menu appears next to Help.
2. **BHFC → Publish points to site.** The first run asks you to authorise the script (approve it).
3. Done. The site updates within a minute or two.

Use **BHFC → Preview data (no publish)** any time to sanity-check parsing without touching the live site — it logs how many players were read and how many have points.

## Weekly routine from now on

1. Enter the week's scores in the sheet as usual.
2. **BHFC → Publish points to site.**

That's the whole loop. No CSV, no re-upload, no editing HTML.

## Announcements (optional)

Add a tab named **Announcements** with three columns: **Title | Body | Date** (row 1 = those headers, one announcement per row after). On the next publish they appear in a banner at the top of the leaderboard. Clear the rows to remove them.

## Biggest movers

Every publish snapshots the totals. The next publish compares against them and shows who gained the most points that week. The first publish after setup has nothing to compare to yet, so the movers strip stays hidden until the second publish. (The `data.json` shipped with this setup already has last week's totals seeded, so movers show immediately.)

## Good to know

- **The site always mirrors your sheet's TOTAL PTS column.** The script never recalculates a player's total — it reads it. Bonuses on the site are shown as one figure (total minus attendance, goals, and assists), so everything reconciles even if you add new bonus columns.
- **New bonus columns are picked up automatically.** The script finds the TOTAL PTS column and treats everything between attendance and that column as scoring/bonus data. Add a column, give it a weight, keep going.
- **When a new game week starts (G6, G7, G8):** add the attendance columns in the sheet and bump `ATTEND_COLS` in the CONFIG block from `5` to the new count. Everything else is automatic.
- **If a publish fails**, the menu shows the error. The most common causes are a mistyped repo/owner, a token without Contents write access, or the wrong branch name.
