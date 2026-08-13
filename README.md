# FTE Vendor Priority Chart

The daily priority order FTE Vendors work from. One page, one source of truth, hosted at a URL you
can share or embed.

**Live page:** https://aascroft.github.io/fte-priority-chart/
_(goes live once Pages is switched on — see [Step 1](#step-1--switch-on-the-live-url) below)_

---

## Step 1 — Switch on the live URL

A one-time setting only a repo admin can flip:

1. Go to **[Settings → Pages](https://github.com/aascroft/fte-priority-chart/settings/pages)**
2. Under **Build and deployment → Source**, choose **Deploy from a branch**
3. Set the branch to **`main`** and the folder to **`/ (root)`**
4. **Save**

Wait a minute or two, then open https://aascroft.github.io/fte-priority-chart/. From then on, every
change pushed to `main` appears on the live page within about 30 seconds — no deploy step, nothing
to click.

> The chart must be on `main` for Pages to serve it. Work happens on a branch first, so it needs
> merging into `main` before it shows up.

## Step 2 — Share it

**As a link** — paste the URL anywhere: Slack, an onboarding doc, a bookmark, a Google Doc. This
works everywhere and is the recommended default.

**As an embed** — anywhere that allows iframes, paste this:

```html
<iframe src="https://aascroft.github.io/fte-priority-chart/"
        width="100%" height="780" style="border:0;" loading="lazy"
        title="FTE Vendor Daily Priorities"></iframe>
```

`height="780"` fits the current seven priorities on a full-width page. In a narrow column the text
wraps to more lines and needs more room — if you see a scrollbar inside the box, raise the number
to `1000` or so. The height can't adjust itself; that's an iframe limitation, not a bug.

Where embedding works:

| Where | Works? | What to do |
|---|---|---|
| Confluence | Usually | Insert an **HTML** or **Iframe** macro and paste the snippet. Some instances have these macros disabled by an admin — if yours does, use the plain link instead. |
| Google Docs | **No** | Google Docs blocks all live embeds. Paste the link, or paste a screenshot of the page. |
| Notion, most wikis, internal portals | Usually | Paste the URL and pick the "embed" option, or use the snippet. |

---

## Making changes

Everything that appears on the page lives in **one clearly-marked block at the top of
[`index.html`](index.html)** — look for the `EDIT BELOW` banner. Below the second banner is the
drawing code, which you shouldn't need to touch.

Easiest route: open this repo in Claude Code and describe the change in plain English. The repo
carries its own context in [`CLAUDE.md`](CLAUDE.md), so there's no need to re-explain what the chart
is or who it's for.

If you'd rather edit it yourself, GitHub can do it in the browser: open `index.html`, click the
pencil icon, make the change, and commit.

### Add a ClickUp link to a priority

Each priority has a `url:` line. Priority 1 is filled in; the rest say `null`. Swap the `null` for
the link, in quotes:

```js
url:     null   /* ← paste this priority's ClickUp link here */

// becomes:

url:     'https://app.clickup.com/t/9003147350/86e2tyev2'
```

That card then becomes clickable, opens in a new tab, and shows a small arrow so vendors can see
it goes somewhere. Cards left as `null` stay as plain text with no hover, so nobody clicks at
something dead.

### Change wording, a deadline, or a badge

| To change | Edit |
|---|---|
| The instruction a vendor reads | `title:` |
| The small grey line underneath (e.g. `Due before 3 PM EST`) | `meta:` — or `null` for none |
| Whether the blue **FTE Only** badge shows | `fteOnly:` — `true` or `false` |
| The colour of the left edge and number | `accent:` — `'red'`, `'amber'` or `'green'` |
| Which section heading it sits under | `group:` |
| The grey footnote at the bottom | the `NOTE` line below the priorities |

### Add, remove or reorder a priority

Copy an existing `{ ... }` block, paste it where you want it, and edit the fields. **Don't renumber
anything** — the numbers are generated from the order of the list, so they always come out right.
To remove a priority, delete its whole block.

---

## Notes on how it's built

One file, no dependencies, no build step, nothing loaded from the internet. It works if you
double-click it on your desktop with no connection at all, which is what makes it safe to embed and
means it can't quietly break years from now.

The cards are drawn by a short script from the data at the top of the file, which is what keeps the
numbering automatic and edits to a single line. The trade-off is that JavaScript needs to be on —
true in every normal browser, but worth knowing.

This is a **read-only** chart by design: no checkboxes, no progress saving, no FTE/non-FTE filter,
no live task data from ClickUp. See [`CLAUDE.md`](CLAUDE.md) for the ideas parked for later and
where they'd hook in.
