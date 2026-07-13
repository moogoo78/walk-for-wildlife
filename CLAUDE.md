# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Static website for 為野生動物而走聯盟 (Taiwan Wildlife Parade). Plain HTML/CSS/JS with **no build system, package manager, or test suite**. Deployed via GitHub Pages to https://walkforwildlife.org.tw (see `CNAME`). Content is Traditional Chinese.

## Running locally

Any static file server from the repo root:

```sh
python3 -m http.server 8000   # then open http://localhost:8000
```

There is nothing to build, lint, or compile — edit an HTML/JSON file and reload.

## Architecture

**Each page is fully self-contained.** Every `*.html` inlines its own `<style>` block and its own `<script>`; there are no shared CSS or JS files. As a direct consequence, the **navbar, dropdown menu, and footer are duplicated verbatim across all pages**. When changing shared UI (a nav link, the dropdown, footer contact info, or the nav/dropdown CSS), you must edit **every** page: `index.html`, `about.html`, `member.html`, `news.html`, `join.html`, `documents.html`, `donate.html`. The active page marks its own nav item with `class="active"`. `donate_demo.html` is an untracked scratch/demo copy — do not treat it as a real page.

Shared UI conventions worth matching when editing:
- Dropdown nav items use `<li class="dropdown">` with a `<ul class="dropdown-menu">`; a small script toggles `.open` on tap for mobile (`window.innerWidth <= 768`) and CSS opens it on hover for desktop.
- Brand green palette: `#2c5530` (dark), `#4a7c59` (mid), with light backgrounds like `#f8faf8`.

**`news.html` is the only data-driven page.** It `fetch()`es `news.json` at runtime and renders the list client-side, with topic-filter tabs built from the distinct `topic` values. It does no server calls beyond that static file.

## `news.json` (the news feed)

Array of objects, **sorted newest-first**, each with exactly these keys in this order:

```json
{ "date": "YYYY-MM-DD", "source": "…", "url": "https://…", "topic": "…", "title": "…" }
```

Conventions the render logic in `news.html` depends on:
- `date` must be zero-padded `YYYY-MM-DD`.
- The "查看連結" button only renders when `url` starts with `http`; otherwise it is hidden. Prefer a real article URL, but an entry with `url: ""` is valid and simply shows no link.
- If `title` starts with `http` (a bare link with no headline), the list displays `source` in its place instead of the raw URL.
- `topic` drives the filter tabs — reuse existing topic strings (e.g. 新聞, 記者會, 新聞稿, 定期宣導) rather than inventing near-duplicates, or a new tab appears.

## Updating the news feed from source data

`source/` holds the master spreadsheets (a Google-Sheets export "聯盟成績連結彙整", as `.csv` and `.xlsx`). These are the input used to extend `news.json`. Key gotchas learned from doing this:
- The CSV's 連結 column usually contains the **headline text, not a URL**. The real article URLs live as **embedded hyperlinks** in the `.xlsx` 連結 column (column D) — read them via the cell's hyperlink target (unzip the xlsx and map `xl/worksheets/_rels/sheet1.xml.rels`, or use openpyxl's `cell.hyperlink.target`), not the displayed text.
- Only prepend rows newer than the current top of `news.json`; older rows already exist there (with their real URLs) and should be left untouched.
- Watch for date typos in the source (e.g. year slips) and normalize to `YYYY-MM-DD`.
- Strip tracking params (`fbclid`, `utm_*`, Facebook `__cft__`/`__tn__`, MSN render junk) from URLs, but preserve functional query params that identify the article/video (e.g. `unid=`, YouTube `v=`, chinatimes `?chdtv`).

## Git / deploy

`main` is the deployed branch (GitHub Pages). Commits to `main` go live. The remote is SSH (`git@github.com:…`); in some environments outbound port 22 is blocked — if `git push` hangs, SSH over port 443 (`ssh.github.com`) is the workaround.
