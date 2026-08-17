# BHFC Season 1 — Points Tracker

A single-page website where BHFC players can look up their drop-in points and see exactly how they were earned. Built as one self-contained `index.html` file and hosted free on GitHub Pages.

## What it does

- **Leaderboard tab** — every player with a non-zero score, ranked by total points. Search by name, tap any player to expand their full breakdown: attendance, goals, assists, and bonuses, all summing to their total.
- **Socials tab** — links to the club Instagram and YouTube, with a featured Instagram post and an auto-updating YouTube latest-video embed.
- **Appreciation tags** — small badges shown next to standout players' names, awarded automatically from the data (see below).

## How scoring is shown

Each player's total is broken into four lines:

- **Attendance** — 3 points per game played
- **Goals** — named individually with a count
- **Assists** — named individually with a count
- **Bonuses** — every other tag (clean sheets, MVP, cards, and all the fun ones) folded into one number

This is deliberate: naming only goals and assists, while keeping everything else as a single "Bonuses" figure, quietly reinforces that points come from more than just scoring.

The site always mirrors the **TOTAL PTS** column from the master sheet exactly. It never invents or recalculates a total — if the sheet says 24, the site shows 24.

## Appreciation tags

Tags are computed automatically each time the data is updated. No manual upkeep.

- **Leader** — the current #1 on the board
- **Top scorer** — the most goals scored (ties allowed)
- **Playmaker** — the most assists (ties allowed)
- **Drop Ins Lover** — attended the most games so far (ties allowed)

As the season goes on and more players hit full attendance, more players will earn "Drop Ins Lover" — that's intended, since it rewards showing up.

## Updating the points

Scores now come from a separate `data.json` file, published straight from the Google Sheet by an Apps Script — no more manual CSV exports or HTML re-uploads. See `SETUP.md` for the one-time setup. After that, the weekly routine is:

1. Enter the week's scores in the master sheet.
2. In the sheet: **BHFC → Publish points to site.**

The site updates within a minute or two. `index.html` only needs re-uploading if the design or layout changes.

## Updating the socials

- **YouTube** updates itself — the embed always shows the newest upload from the channel. Nothing to do.
- **Instagram** shows one fixed featured post. To feature a different post, grab a fresh embed code (post → "..." menu → Embed → Copy embed code) and swap it into `index.html`.

## Deploying to GitHub Pages

1. Create a GitHub repository (public or private).
2. Upload `index.html` to the root of the repo. The main file **must** be named `index.html`.
3. Push to the default branch (`main` or `master`).
4. In the repo, go to **Settings → Pages**. Under **Source**, pick your branch and the `/root` folder, then **Save**.
5. Wait a few minutes. The site will be live at `https://<your-username>.github.io/<repository-name>/`.

## Biggest movers & announcements

The leaderboard tab shows a **biggest movers** strip — who gained the most points since last week — computed automatically each publish by comparing against the previously published totals. It also shows an **announcements** banner, which you fill from an optional `Announcements` tab in the sheet. Both are covered in `SETUP.md`.

## A note on testing

Open the **live GitHub Pages URL** to test the site, not the local file. Two things only work over `https://` from GitHub Pages, not when opening the file directly from your computer (`file://`): the Instagram/YouTube embeds on the Socials tab, and loading `data.json` (the leaderboard). If the page looks empty locally, that's expected — check it after deploying.

## Technical notes

- Two files: `index.html` (shell + logic) and `data.json` (the scores). The page fetches `data.json` on load.
- `Code.gs` is the Apps Script that reads the sheet and publishes `data.json` to the repo.
- Pure HTML, CSS, and vanilla JavaScript. No build step, no frameworks.
- The club logo is embedded in `index.html` as a base64 image, so the shell is self-contained.
- Players with zero points are hidden from the board.
- Works responsively on phones, which is how most players will view it.
