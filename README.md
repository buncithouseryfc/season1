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

When there's a new week of scores:

1. Export the master sheet as CSV (same column layout as before).
2. The data is regenerated and baked directly into `index.html` — there are no separate data files, images, or dependencies. Everything (scores, logo, styling, scripts) lives in that one file.
3. Replace `index.html` in the repo and push.

The layout is resilient to the usual changes: added players, added bonus columns, changed point weights, and name corrections are all handled automatically. Only a major restructure of the sheet would need a closer look.

## Updating the socials

- **YouTube** updates itself — the embed always shows the newest upload from the channel. Nothing to do.
- **Instagram** shows one fixed featured post. To feature a different post, grab a fresh embed code (post → "..." menu → Embed → Copy embed code) and swap it into `index.html`.

## Deploying to GitHub Pages

1. Create a GitHub repository (public or private).
2. Upload `index.html` to the root of the repo. The main file **must** be named `index.html`.
3. Push to the default branch (`main` or `master`).
4. In the repo, go to **Settings → Pages**. Under **Source**, pick your branch and the `/root` folder, then **Save**.
5. Wait a few minutes. The site will be live at `https://<your-username>.github.io/<repository-name>/`.

## A note on testing

Open the **live GitHub Pages URL** to test the Socials tab, not the local file. Instagram and YouTube embeds are blocked when opening the HTML directly from your computer (`file://`), but they work correctly once served over `https://` from GitHub Pages. If the Socials tab looks empty locally, that's expected — check it after deploying.

## Technical notes

- Pure HTML, CSS, and vanilla JavaScript. No build step, no frameworks, no external data files.
- The club logo is embedded as a base64 image, so the file is fully portable.
- Players with zero points are hidden from the board.
- Works responsively on phones, which is how most players will view it.
