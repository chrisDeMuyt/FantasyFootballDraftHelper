# Fantasy Football Draft Helper

A single-file draft console for live fantasy football drafts. Paste in the draft
log or pull the board from a connected league, and it answers one question:
**who do I take right now?**

`draft-console.html` is the whole application — no build step, no dependencies,
no package manager. Open it and it runs. The only external request is to Google
Fonts.

## What it does

- **The pick.** One recommendation, with the reasoning underneath, for your
  current slot and detected positional need.
- **Tier health.** How many players remain in each analyst tier. When a tier
  goes red it is about to cliff — the moment when reaching is actually correct.
- **Next best.** The alternatives behind the recommendation, ranked.
- **Gone since you last checked.** What came off the board while you were away.
- **My team.** The roster as drafted so far, by position.
- **Draft log paste.** Paste ESPN's own pick list (`Justin Jefferson / MIN WR`
  on one line, `R1, P1 - Team Name` on the next) to sync state by hand.

Supports ESPN and Sleeper, any league size, snake order. A name-to-player-id map
for the 2026 draftable pool is baked into the file, because draft picks come back
carrying only a numeric id and drafted players leave the pool.

## Live data requires the Claude artifact runtime

The board itself is fetched through a fantasy connector via
`window.claude.use("mcp")`. That API exists only when the page is served as a
Claude artifact from claude.ai.

**Opened anywhere else — including the GitHub Pages copy of this repo — the page
renders, but "Who do I take?" will report that live data is unavailable.** The
draft-log paste path routes through the same recommendation step, so it is
limited the same way. Treat Pages as a look at the interface; use the published
artifact to actually draft.

## GitHub Pages

`index.html` is a redirect stub that forwards to `draft-console.html`, so Pages
has a root document to serve. The console keeps its own filename so that
republishing it to its existing artifact URL stays a same-path operation.

To turn Pages on: **Settings → Pages → Source: Deploy from a branch → `main` /
root**.

## Repo layout

This repository is a view onto a larger private working directory of draft prep
(analyst transcripts, strategy briefs, player notes). `.gitignore` ignores
everything by default and re-admits only the files listed here, so nothing
private is publishable by accident.

| File | What it is |
|---|---|
| `draft-console.html` | The console. The entire application. |
| `index.html` | Redirect stub for GitHub Pages. |
| `.gitignore` | Whitelist — ignores all, admits only the above. |

## A note on the league IDs

The file ships with ESPN league ids as default form values. These are not
secrets: an ESPN league id identifies a league but grants nothing, and private
leagues still require auth cookies, which are not in this file. Change the
**League ID** field to point the console at your own league.
