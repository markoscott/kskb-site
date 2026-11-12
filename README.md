# kskb-site: the front end of kskb.dev

The KERISuite Knowledge Base (KSKB) at https://kskb.dev/ is one self-contained web page, generated from the base's
data. This repository holds the part of it a designer or a front-end developer edits: the page's template, its
stylesheet and its script. The data (the cards, the roles, the notes) is not here; it comes from the base at build
time and is not edited in these files.

## The three files

| File | What it is |
|---|---|
| `template.html` | The page's skeleton: the head (fonts), the ribbon, the navigation bar, the two drawers, one empty `<section>` per page, and three placeholders the build fills: `{{STYLE}}`, `{{DATA}}`, `{{SCRIPT}}`. |
| `site.css` | The whole stylesheet. The variables at the top (`--ink`, `--surface`, `--accent`, `--mono`, the widths) set the look; a dark theme follows the `data-theme` attribute. |
| `site.js` | The whole script: the page router (`go`, `PAGES`), the renderers (one `render…` function per page: home, the introduction, the entry pages, every card, roles, the card page, topics, timeline, meetings, products, notes, and the behind-the-scenes pages), the facets, the drawers and the browser history. It reads the data from the `kskb-data` JSON block the build inlines. |

`preview.html` is generated, not edited: the page with the current data inlined and the stylesheet and the script
*linked* (`<link rel="stylesheet" href="site.css">`, `<script src="site.js">`) rather than inlined, so that you can
open it in a browser from this folder, edit `site.css` or `site.js`, reload, and see the change at once. Mark's
builds regenerate it with fresh data; do not commit edits to it.

## How the page is built

Mark's generator (a Python script beside the base's database) reads the three files, inlines the stylesheet and the
script into the template, inlines the data as JSON, and writes `index.html`, one file with no library and nothing
fetched but the fonts. That file is what kskb.dev serves (a release repository deploys it on push, together with a
developer view at `/dev/` and the databases at `/data/`). So a change to these files reaches the site at the next
build and push, which Mark runs; nothing here deploys on its own.

Two facts about the page as it stands, on purpose and open to change if we test it on every page:

- There is no `<!doctype html>` at the top; the page renders as it does today without one. Adding it is a real
  change (standards mode), to be looked at on each page before it goes in.
- Everything is one file in production: no framework, no bundler, no external script. Keep it that way; it is what
  makes the page portable to any host (see the licenses below).
- The persona switch (the "Choose a persona" drawer and the Access page) is a demonstration of a possible future
  sign-in, labeled illustrative wherever it appears. It is a setting of this page, not authentication: every note in the shared copy is in the page data and the
  downloadable database,
  so nothing is hidden from anyone. Sign-in and subscriptions are planned, not running. Keep the label and that
  wording when you change those pages. The design of the personas is Janet Gonzales's.

## Working here

1. Make a branch from `main` (`janet/styling`, say). Edit `site.css`, `site.js` or `template.html`; open
   `preview.html` to see it. The browser's back and forward buttons move within the page; every page has an id in
   `PAGES` and a `render…` function; the navigation is built in `renderNav`.
2. Open a pull request. Mark or his assistant builds the site from your branch to check every page, then merges.
3. After the merge, the next build and push puts it on kskb.dev.

If you want a different structure for the files (a `css/` folder, several stylesheets, a build step), say so;
the generator can read whatever layout we agree on, as long as production stays one self-contained file.

## Credit and license

The navigation of 3 November 2026 (a reader's role first, the five-step introduction, the collections grid, four
to start with, the role start pages) follows Janet Gonzales's prototype; the base credits her on the page. The code
in this repository is under the Apache License 2.0 (`LICENSE`), the base's own content (abstracts, labels, notes)
is CC BY 4.0, and the sources the base indexes keep their own licenses. Contributions are licensed the same way
as the file they land in. KSKB is Mark Scott's work, with thanks to KERISSE, Kor Dwarshuis's and Henk van Cann's
search engine, whose dataset is the source of the documents the site shows.
