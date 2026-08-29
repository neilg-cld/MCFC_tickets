# MCFC Season Tickets

A single-file web app for tracking four Manchester City FC season tickets — Colin Bell Stand, Block 031, Row 28, Etihad Stadium — across a Premier League season. Built for four seat holders (Neil, Sam, Maia, Sarah) to log who's using each seat per fixture, track money recovered, and share what's available.

Live at: `https://<your-username>.github.io/MCFC_tickets/`

## How it works

- **One file, no server.** `index.html` is the entire app — HTML, CSS and JS in one file, hosted free on GitHub Pages.
- **Data lives in a GitHub Gist**, not in this repo. A secret (unlisted) Gist holds the season data as JSON; the app pulls it on open and pushes changes automatically a moment after you make them. That's what makes your phone and laptop show the same thing.
- **localStorage is the offline fallback.** If the Gist is unreachable, the app still works from the last-synced copy on that device.
- **Nothing sensitive is ever in this repo.** Your GitHub token, Anthropic API key, and app-lock password all live only in each device's browser storage — never committed, never in the Gist, never in a backup export.

## Files

| File | Purpose |
|---|---|
| `index.html` | The app itself |
| `manifest.json` | Web app manifest — lets "Add to Home Screen" install it as a standalone app |
| `icon-192.png`, `icon-512.png`, `apple-touch-icon.png` | App icons |
| `README.md` | This file |

All five files sit together at the repo root. Every deploy = upload the changed file(s) and let GitHub Pages redeploy (a minute or two).

## First-time setup

1. **Deploy**: upload all five files to this repo. Settings → Pages → Deploy from branch → `main` / root.
2. **Seed the Gist**: on your primary device, open the live URL, Settings → Cloud sync → *Create gist & upload this device's data*. If you're migrating from an older local copy, export a backup from the old file first and import it here before creating the gist, so it seeds with real data.
3. **GitHub token**: github.com/settings/personal-access-tokens → fine-grained token → **Gists: Read and write** only, no repo access. Paste into Settings → Cloud sync on each device.
4. **Connect other devices**: same URL, Settings → Cloud sync → *Connect to existing gist*, paste the token and the gist ID.
5. **Add to Home Screen** on phones for a full-screen app icon rather than a browser tab.

Optional, set up whenever:

- **Fixture check (Settings → Fixture check)**: paste your own Anthropic Console API key (console.anthropic.com) to check mancity.com for date/kick-off/result changes from inside the app. Billed to your own Console credit. A no-key fallback ("copy a prompt for a Claude chat") also works, using the `mcfc-fixture-update` skill.
- **App lock (Settings → App lock)**: generate a username + password in-app, paste the resulting hash into `index.html` (replacing the `APP_AUTH_HASH` constant near the top), redeploy. Deterrent only — this is a public repo, so it stops someone stumbling on the link, not a determined attacker.
- **Contacts (Settings → Contacts)**: save people you regularly give tickets to. Syncs with everything else.

## What it tracks

Eight ticket statuses per seat per fixture — **Pending, Attending, Guest, Listed, Sold, Unused, Exchanged, Transferred** — each with its own financial and phone-usage logic. Attending, Guest and Sold count toward the phone's minimum-use target; the rest don't, since access happens via someone else's account or not at all.

Kick-off times are flagged **Confirmed** or **Provisional**, matching whether mancity.com still shows a "subject to change" notice.

## Sharing what's available

**🖶 Print** — full season on one A4 page: every fixture, every seat's status, running financials.

**📄 Share available** — a flyer listing all 19 home fixtures with their current situation (available count, "Played", or "0 available"), for sending to contacts. Uses the browser's print dialog — choose *Save as PDF*.

## Updating the app

Ask Claude for changes the same way as always — describe what you want, get a new `index.html` (and any other changed files), upload and overwrite. Versioning follows semantic versioning:

- **PATCH** (1.29.**1**) — bug fixes only
- **MINOR** (1.**30**.0) — any functional or cosmetic change
- **MAJOR** (**2**.0.0) — breaking changes to the data structure

Full history is in the in-app changelog (Settings → View changelog) — that array is the single source of truth for the current version number too.

## Security notes

- Secret Gist = **unlisted**, not private. Anyone with the URL could read it. Fine for fixture/seat data; don't let this app grow into holding anything more sensitive.
- GitHub token, Anthropic API key, and app-lock password are all **localStorage-only** — paste once per device, never synced, never backed up.
- App lock is a **deterrent**, not real security — the hash is sitting in a public file, readable by anyone who opens dev tools. It stops a stranger poking around, not someone determined.
