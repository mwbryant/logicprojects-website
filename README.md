# logicprojects.net

A Jekyll site for GitHub Pages. GitHub builds it — you push markdown, the site
updates. No CI to configure, no build step to run locally.

---

## 1. Push it to GitHub

Create a repository (any name — the custom domain makes the repo name
irrelevant), then from this folder:

```bash
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/mwbryant/YOUR-REPO.git
git push -u origin main
```

## 2. Turn on Pages

**Settings → Pages** in the repo.

- **Source:** Deploy from a branch
- **Branch:** `main`, folder `/ (root)`

The `CNAME` file already contains `logicprojects.net`, so GitHub picks up the
custom domain on its own. If it doesn't, type it into the **Custom domain** box.

## 3. Fix the DNS at Squarespace

**Domains → logicprojects.net → DNS Settings → Custom records.**

There are **no MX records on this domain** — confirmed against live DNS — so
nothing below can break email. There is no email.

**Status (8 Oct): DNS is functionally complete.** All four GitHub A records are
in, and `www` points at `mwbryant.github.io`. Five of the eleven dead Bluehost
records are deleted; the remaining six are inert and can be cleared whenever
convenient. Nothing below blocks the site from working.

**Delete these.** All of it points at a Bluehost server that no longer serves
you (`box5321.bluehost.com`):

| Type | Name | Data | Status |
|---|---|---|---|
| A | `@` | `162.241.226.58` | deleted |
| A | `cpcalendars` | `162.241.226.58` | deleted |
| TXT | `_caldav._tcp` | `path=/` | deleted |
| TXT | `_carddavs._tcp` | `path=/` | deleted |
| A | `whm` | `162.241.226.58` | deleted |
| CNAME | `cpanel` | `logicprojects.net` | leftover, harmless |
| CNAME | `ftp` | `logicprojects.net` | leftover, harmless |
| SRV | `@` | `0 2079 box5321.bluehost.com` | leftover, harmless |
| SRV | `@` | `0 2080 box5321.bluehost.com` | leftover, harmless |
| TXT | `_caldavs._tcp` | `path=/` | leftover, harmless |
| TXT | `_carddav._tcp` | `path=/` | leftover, harmless |

**Keep this one.** It's a Google domain-verification token — harmless, and
deleting it breaks Search Console if you ever set it up:

| Type | Name | Data |
|---|---|---|
| CNAME | `eeoicddkd6lh` | `gv-kamcz4vzc3oosk.dv.googlehosted.com` |

Leave the `_domainconnect` CNAME alone too — that's Squarespace's own registrar
plumbing, listed under DNS Presets rather than custom records.

**Add four A records**, all named `@`:

| Data | Status |
|---|---|
| `185.199.108.153` | added |
| `185.199.109.153` | added |
| `185.199.110.153` | added |
| `185.199.111.153` | added |

Optionally the IPv6 equivalents, also named `@`:

```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

**Change the `www` record.** It currently CNAMEs to `logicprojects.net`:

| Type | Name | Value | Status |
|---|---|---|---|
| CNAME | `www` | `mwbryant.github.io` | done |

## 4. Turn on HTTPS

**Settings → Pages → Enforce HTTPS.** The checkbox can take up to 24 hours to
appear while GitHub issues the certificate. The old A record carries a 4-hour
TTL, so expect a few hours before the switchover is complete everywhere.

---

## Editing

### Identity and links

Everything you'll change routinely lives at the top of `_config.yml`:

```yaml
title: Logic Projects
designation: LP-00          # the drawing number in the footer title block
tagline: >-
  Field notes from the machine...

facts:                      # the readout under the masthead, and the title block
  - label: Subject
    value: FPGA · embedded · sensing
  - label: Status
    value: Active

links:                      # builds the Archives table and the header nav
  - name: GitHub
    url: https://github.com/YOUR-USERNAME
    host: github.com        # the mono data field in the left column
    blurb: HDL, firmware, and the tooling around it.
```

All three links are set: `mwbryant`, `@logicprojects` on YouTube, and
`logicprojects.itch.io`.

### Adding a log entry

Drop a file in `_posts/` named `YYYY-MM-DD-slug.md`:

```markdown
---
layout: post
title: "Getting a PLL to lock on the iCE40"
summary: "One line, shown in the table and the feed."
tag: Tutorial
---

Markdown from here.
```

Push it. It appears on the home page and at `/logs/slug/`. Both `summary` and
`tag` are optional.

Markdown gets the full treatment: headings rule off, code blocks take an oxide
left edge, tables set in mono with tabular figures, blockquotes inset, bullets
marked with oxide squares.

### Images

`assets/img/`, referenced as `/assets/img/name.jpg`. They get a hairline border.

---

## The design

Dark only — no light variant, deliberately. The layout is an engineering
drawing sheet: a bordered plate with crop marks at the corners, content set as
datasheet tables, and a title block at the foot.

Every color is a variable at the top of `assets/css/style.css`:

```css
--ground:  #14100e;   /* deep oxide-black, warm not blue */
--plate:   #1a1512;
--inset:   #221b17;
--bone:    #dcd3c4;   /* text — aged paper, never pure white */
--oxide:   #b33a1e;   /* rust red: marks, rules, emphasis */
--brass:   #c2913c;   /* brass: data values, links */
```

Change `--oxide` and `--brass` and the whole site follows. Push `--ground`
toward `#0d0d10` for something colder and more clinical; toward `#1a1210` for
more rust.

Type is Archivo Narrow (display, caps), Archivo (prose), Space Mono (data
fields), all from Google Fonts.

To dial the theme back, the thematic words are all in content you control:
"Archives" and "Logs" in `index.html` and `logs.html`, and the title-block
labels in `_layouts/default.html`.

---

## Local preview (optional)

Not required — GitHub builds the site. But if you want it:

```bash
gem install jekyll
jekyll serve
```

`http://localhost:4000`.

---

## Files

```
_config.yml           identity, facts, links
index.html            masthead, Archives table, Logs table
logs.html             the full log index at /logs/
feed.xml              Atom feed, no plugins
_layouts/default.html page frame, header strip, title block
_layouts/post.html    log entry
_layouts/page.html    plain page
_posts/               your markdown
assets/css/style.css  the whole design, ~370 lines
CNAME                 logicprojects.net
```
