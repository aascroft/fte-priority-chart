# Context for Claude

This file exists so a fresh session can pick up this project without the owner re-explaining it.
**If you change how the project works, update this file in the same commit.**

## What this is

A single-page priority chart telling **FTE Vendors** what order to work their daily tasks in. It
replaces a chart that previously only existed as a screenshot, which meant every change required
re-explaining the whole thing and re-sharing an image.

Two goals, equally important:

1. **A hostable URL** vendors can open, and an embed snippet for places that allow iframes.
2. **A repo that carries its own context**, so changes are a one-line request rather than a
   re-briefing. That second goal is what this file serves.

- **Owner:** Allan Ascroft (allan.ascroft@flipp.com)
- **Audience:** FTE Vendors doing FQCs, Flyer Reviews, Page Swaps and Uploads at Flipp
- **Repo:** `aascroft/fte-priority-chart` (public), default branch `main`
- **Hosting:** GitHub Pages from `main` / root → https://aascroft.github.io/fte-priority-chart/

## The work the chart describes

Vendors handle four kinds of task. **FQCs** and **Uploads** are done by all vendors; **Flyer
Reviews** and **Page Swap / revision tasks** are marked `FTE Only` and are not for every vendor.

The chart splits into two sections. **Today's Deadlines** (priorities 1–4) are time-bound and must
land today, two of them against hard clock times — FQCs before 3 PM EST, Flyer Reviews before
4 PM EST. **Remaining Work** (5–7) is the same categories of task without today's deadline, worked
once the deadline items are clear.

The `Due date` is the date *before* a flyer goes live, and a flyer can go live as early as 12 AM
EST. So "due today" genuinely means today — there is no overnight slack, which is the reasoning
behind the footnote on the page and behind the ordering itself.

## Files

| File | What it is |
|---|---|
| `index.html` | The entire page. Content in a marked `EDIT BELOW` block at the top; drawing code below the second banner. |
| `README.md` | Written for Allan, not for developers: Pages setup, embed snippet, how to change things. |
| `CLAUDE.md` | This file. |

No build step, no dependencies, no package manager, no CI. Deployment is `git push` to `main`;
Pages serves it directly.

## Decisions, and why

Locked in a scoping conversation before the build. Don't quietly reverse these — check first.

| Decision | Reasoning |
|---|---|
| **Static page, no ClickUp API** | Explicitly out of scope. Vendors would hit a login wall and it adds a live dependency for no gain. |
| **GitHub Pages** | The repo was already public; Pages gives a URL for free with no accounts or deploy step. |
| **Read-only** | No checkboxes, no FTE/non-FTE filter, no time-based urgency. Chosen deliberately over building behaviour nobody had asked vendors about. |
| **No expand/collapse** | Wanted eventually (see below), but v1 uses a plain click-through instead. |
| **Wording transcribed verbatim** | The seven priorities had already been through review. Rewording them is a separate decision from building the page. |
| **Content in a data block, cards drawn by script** | Keeps numbering automatic and each edit to one line, and makes the deferred expand feature a template change rather than a rewrite. Trade-off accepted: the page needs JavaScript on. |
| **Self-contained single file** | No external CSS, fonts or scripts. Survives embedding, works offline, can't rot. **Keep it this way** — don't introduce a framework or a CDN link. |
| **Edits via Claude Code against this repo** | Rather than a hand-editable config file or an admin UI. Hence this file. |

Two deliberate departures from the original screenshot, both defensible:

- **A small arrow on linked cards.** The screenshot had no links. A clickable card has to look
  clickable before you touch it — phones have no hover.
- **A screen-reader-only `<h1>`.** The screenshot has no visible heading and the page keeps it that
  way, but an untitled page is bad for assistive tech and duplicating a heading would look wrong
  inside an embed.

## ClickUp links

Priorities are clickable only where a `url:` is filled in. One is done; the rest were to be added
later.

| # | Priority | Link |
|---|---|---|
| 1 | FQCs due Today | `https://app.clickup.com/t/9003147350/86e2tyev2` |
| 2 | Page Swap / revision, LIVE or due Today | outstanding |
| 3 | Flyer Reviews due Today | outstanding |
| 4 | Available uploads | outstanding |
| 5 | Other available FQCs | outstanding |
| 6 | Other available Flyer Reviews | outstanding |
| 7 | Additional Uploads | outstanding |

When Allan supplies more, paste each into that priority's `url:` field. Nothing else needs changing.

## Parked ideas, and where they'd hook in

Wanted, but not built. All were considered and consciously deferred — treat them as ready to build
on request, not as oversights.

- **Click a priority to expand a list of its ClickUp tasks.** The original ask. Now that cards are
  built from data, this is a change to `buildCard()` plus an extra field per priority (e.g. a
  `links: []` array, or a `detail` note) — not a rewrite. Still no API: curated links and guidance
  the chart maintains itself.
- **FTE / non-FTE toggle** so non-FTE vendors see only their own priorities. The `fteOnly` flag
  needed to drive it already exists on every priority.
- **Tick-off progress** saved per-vendor in their own browser. Rejected for v1 partly because it
  implies progress tracking that doesn't exist.
- **Time-based urgency** (e.g. priority 1 turning red past 2 PM EST). Needs a decision on time
  zones first — the deadlines are EST but vendors may not be.

## Things to know before changing this

- **Pages serves `main`.** Work on a branch, but nothing is live until it's merged.
- **Vendors read this to do their jobs.** Wording is operational instruction, not copy — don't
  "improve" the seven priorities unless asked. Ambiguity here costs someone a missed deadline.
- **Keep the file self-contained** and keep content separated from code, so a non-developer can
  find the words they want to change.
- **Check both widths after visual changes.** Headless Chromium clamps its window to a 500px
  minimum, so a `--window-size=390` screenshot silently crops and looks broken. To see a real phone
  render, load the page inside a 390px-wide iframe on a wrapper page and screenshot that.
