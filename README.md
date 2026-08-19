# SaaS notes: a permanent study archive

A hand-built static site. No framework, no build step, no dependencies, no tracking.
Every page is a single self-contained HTML file that renders correctly offline, in any
browser, with no server, and will still do so in ten years.

---

## What's in here

```
index.html                      the hub, lists every section, filterable by topic
sections/                       one self-contained HTML file per study section
  cac-cost-of-arr-payback-ltv.html
_template/
  section-template.html         copy this to start a new section
.nojekyll                       tells GitHub to serve files exactly as written
README.md                       this file
```

**The architecture in one sentence:** each section is an independent, complete document;
the hub is just a page of links to them. Nothing shares code, so nothing can break
anything else.

That choice is deliberate. A shared stylesheet would save some duplication, but it would
also mean that one bad edit breaks every page at once, and that a section saved to disk
or emailed to someone renders as unstyled text. Self-contained files trade a little
repetition for the guarantee that each piece works forever, on its own.

---

## Publishing it (GitHub Pages, free, no command line)

1. Create a free account at **github.com** if you don't have one.

2. Create a new repository named exactly:

   ```
   joshyoungjae-fpa.github.io
   ```

   That exact name (matching your username) is what puts the site at the root URL. Set it to **Public**;
   GitHub Pages requires public repos on the free plan.

3. On the empty repository page, click **"uploading an existing file"**.

4. Drag the *contents* of the `site` folder into the browser: `index.html`,
   the `sections` folder, the `_template` folder, `.nojekyll`, and this README.
   Drag the contents, not the `site` folder itself, or every URL gains an extra
   `/site/` in it.

5. Click **Commit changes**.

6. Go to **Settings → Pages**. Under *Source*, choose **Deploy from a branch**,
   pick branch `main` and folder `/ (root)`, then **Save**. For a
   `username.github.io` repo this is often already configured.

7. Wait about a minute, then visit:

   ```
   https://joshyoungjae-fpa.github.io
   ```

If you see a 404 at first, give it two or three minutes. The first deploy is the
slow one.

### Custom domain (optional, later)

Buy a domain anywhere, then in **Settings → Pages → Custom domain** enter it and
follow the DNS instructions GitHub shows. Add a file named `CNAME` (no extension)
containing just your domain. HTTPS is free and automatic. Your existing
`github.io` URL keeps working and redirects.

---

## Personalising it

Open `index.html` in any text editor and change these three things. They're near
the top, right after `<div class="hero">`:

| Find | Change it to |
|---|---|
| `<p class="who">Josh Kim · FP&A</p>` | your name and how you want to be described |
| `<h1>Working notes on SaaS economics</h1>` | your title for the archive |
| the two paragraphs below it | your own framing of what this is and why |

Also update `<title>` in the `<head>`. That's what shows in the browser tab and in
search results.

---

## Adding a new section

1. Copy `_template/section-template.html` into `sections/` and rename it to a
   descriptive, hyphenated, lowercase filename, for example
   `retention-nrr-cohort-survival.html`. **That filename becomes the permanent
   public URL, so choose it once and don't rename it later.** Renaming breaks every
   link anyone has saved.

2. Replace the placeholders: `SECTION TITLE`, `TOPIC`, `NN`, the meta description,
   and the body content. The template contains one of every available component:
   callouts, formula blocks, stat tiles, strength/weakness pairs, tables, self-test
   questions, so you can delete what you don't need rather than write markup from
   scratch.

3. Add a card for it on the hub. Open `index.html`, find the existing
   `<a class="scard" ...>` block, copy the whole thing, and edit the fields:
   section number, filename, topic, title, summary, what it demonstrates, the
   concept tags, the date, and the reading time.

   The topic filter needs nothing extra; it reads `data-topic` and
   `data-concepts` straight off the cards. But if the new section introduces a
   topic that doesn't exist yet, add a matching chip in the `<div class="filters">`
   row:

   ```html
   <button class="chip" data-f="Your Topic" aria-pressed="false">Your Topic</button>
   ```

4. Upload the changed `index.html` and the new section file to GitHub the same way
   as before. GitHub will ask whether to replace `index.html`. Say yes.

---

## Design conventions

Worth keeping consistent, since consistency is most of what makes an archive feel
like one thing rather than a pile:

- **Series colours are fixed and ordered.** Blue, then orange, then aqua
  (`--s1`, `--s2`, `--s3`). Assign them in that order and never skip or reorder;
  the sequence is chosen so that adjacent pairs stay distinguishable under every
  common form of colour blindness. Never introduce a fourth colour for a fourth
  series; fold it into "Other" or split into two charts.
- **Dark mode is a separate set of colours, not an inversion.** Both are already
  defined in the template's `:root` blocks. Don't add colours to one mode only.
- **Text never wears a data colour.** Labels and values stay in the text tokens;
  a small coloured dot beside the text carries identity instead.
- **Every chart needs a table view.** Wrap it in `<details><summary>Table view</summary>`.
  This is what makes the content usable by screen readers and by anyone printing it.
- **Case companies are Company A, Company B, Company C.** No invented brand names.
  Descriptive sub-labels (segments, channels, cohorts) keep their real names when the
  meaning is load-bearing, e.g. "Company A's enterprise segment".
- **No em dashes anywhere.** Use a colon when a clause explains, a comma when it is
  parenthetical, brackets for a genuine aside, or a full stop. This applies to prose,
  headings, chart labels and page titles alike.
- **Say what is measured, what is assumed, and what is stipulated.** Every section
  ends with a methodology note doing exactly that. It's the habit that makes the
  archive trustworthy.

---

## Accessibility

The pages already do the following, and new sections should preserve it: semantic
headings in order, `aria-label` on every chart, a table view behind every graphic,
keyboard-operable disclosure widgets, text that reflows to a phone without
horizontal scrolling, tables that scroll within their own container rather than
breaking the page, and colour never used as the only carrier of meaning.

Contrast and colour-blind separation were validated numerically rather than by eye,
in both light and dark mode.

---

## Backing it up

GitHub is the backup: every version of every file is kept permanently, and you can
restore any earlier state from the repository's history. For a second copy, click
**Code → Download ZIP** on the repository page every so often and keep it somewhere
else.
