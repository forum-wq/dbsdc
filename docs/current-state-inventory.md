# DBS Website — Current State Inventory

**Date:** 2026-07-20
**Baseline:** `main` @ `25537dd` ("Add files via upload")
**Backup tag:** `backup/pre-release-1-2026-07-20`
**Scope:** Phase 0 of DBS Web Upgrade Plan v1.1. Read-only inventory — no live changes.

---

## 1. Internal pages

| Page | Lines | Indexed | Purpose |
|---|---|---|---|
| `index.html` | 2299 | yes | Single-page site: hero → trust bar → problem → services → research/reports → track record → about → contact → footer |
| `thank-you.html` | 392 | **no** (`<meta name="robots" content="noindex, nofollow">`) | Post-form-submission report delivery page |

**Relationships**

- `index.html` → Tally form `LZ7qW2` (research/reports gate) → Tally redirects to `thank-you.html`
- `index.html` → Tally form `xXMLBo` (project contact) → Tally-side confirmation
- `thank-you.html` → `index.html` (2 links: line 307 nav/back, line 372 CTA)
- `index.html` has **no** link to `thank-you.html` — it is reachable only via the Tally redirect

There are no other HTML files in the repo. No CSS or JS files: all styles are inline in a `<style>` block in each page's `<head>` (duplicated, not shared).

---

## 2. External dependencies

### 2.1 Tally forms (production — do not modify)

| Form | URL | Location | Purpose |
|---|---|---|---|
| Research / reports | `https://tally.so/r/LZ7qW2` | `index.html:1984` | Gates the 3 PDF reports; redirects to `thank-you.html` |
| Project contact | `https://tally.so/r/xXMLBo` | `index.html:2186` | Project enquiry |

Both are embedded/linked from `index.html` only. `thank-you.html` contains no Tally references.

### 2.2 usrfiles.com PDFs (5 total — Wix-hosted, migration candidates)

Host prefix for all five: `https://80496fad-0a78-43cd-b632-94e9cfa5a1f9.usrfiles.com/ugd/`

| # | Document | File | Referenced at |
|---|---|---|---|
| 1 | Data Center Industry Research Report (Q1 2026, 16 pages, IC-ready) | `80496f_a78f6432d9314524b93d8a2c944b93ea.pdf` | `thank-you.html:333` |
| 2 | CEE Data Centers: The Growth Frontier | `80496f_bdb19809ed8142c880d4f519c0fbfe70.pdf` | `thank-you.html:348` |
| 3 | Data Center Real Estate & Land Development in CEE | `80496f_b8df826ca90845b6b01d7acf91cdb4ae.pdf` | `thank-you.html:363` |
| 4 | Privacy Policy | `80496f_453918c236944cfd9da68d6db45b71da.pdf` | `index.html:2232`, `thank-you.html:385` |
| 5 | Terms of Service | `80496f_8b1628df28bb4866baf5a6c0e6335b28.pdf` | `index.html:2233`, `thank-you.html:387` |

**7 link instances across 2 files.** Privacy Policy and Terms of Service each appear in both footers — a migration must update both.

All 5 use `target="_blank" rel="noopener"`.

### 2.3 Google Fonts

| Page | Line | Families |
|---|---|---|
| `index.html` | 16–18 | `Instrument Serif` (ital 0;1), `DM Sans` (opsz 9..40, weights 300–700) |
| `thank-you.html` | 10–12 | `DM Sans` (weights 400;500;600;700), `Instrument Serif` (ital 0;1) |

Both pages `preconnect` to `fonts.googleapis.com` and `fonts.gstatic.com` (crossorigin), then load one `css2` stylesheet with `&display=swap`. **The two requests differ** — `index.html` uses the variable-optical-size DM Sans axis, `thank-you.html` uses static weights. Two separate font payloads across a single user journey.

### 2.4 Other external references

| URL | Location | Note |
|---|---|---|
| `https://www.linkedin.com/in/davidsboruta` | `index.html:2149` | Footer social |
| `/cdn-cgi/l/email-protection#...` | `index.html:2136`, `index.html:2223` | Cloudflare email obfuscation — **injected by Cloudflare, not authored**. Present in the committed source, meaning the repo copy was saved from a served page. Requires Cloudflare to resolve; will render as a broken link if the site is ever served without it. |
| `tel:+420732390870` | `index.html:2142`, `index.html:2224` | Phone, twice |

---

## 3. Assets

| Asset | In repo | Referenced by | Lines |
|---|---|---|---|
| `favicon.ico` | yes | both pages | `index:21`, `ty:15` |
| `favicon-32.png` | yes | both pages | `index:22`, `ty:16` |
| `favicon-16.png` | yes | both pages | `index:23`, `ty:17` |
| `apple-touch-icon.png` | yes | both pages | `index:24`, `ty:18` |
| `vector-logo-DBS-review1.png` | yes | **not referenced by either page** | — |
| `DavidBoruta_edited_FINAL_K4A9852_copy.jpg` | yes | **not referenced by either page** | — |

Both pages carry an identical, complete favicon set. Two committed image assets are orphaned: neither the logo nor the founder photo is used in any HTML. The site's visual identity is currently type-only.

**Untracked working-tree files** (not in git, not referenced): `.DS_Store`, `choose Brno.png`, `choose Olomouc.png`, `linda fsf.png`, `logo_DBS.jpg`.

---

## 4. Missing items (verified)

| Item | `index.html` | `thank-you.html` | Notes |
|---|---|---|---|
| `<link rel="canonical">` | **MISSING** | **MISSING** | Not present on either page |
| `og:image` | **MISSING** | n/a | No image tag; no `og-image.png` in repo on `main` |
| `og:image:width` / `:height` / `:alt` | MISSING | n/a | |
| `og:site_name` | MISSING | n/a | |
| `twitter:card` and Twitter tags | MISSING | n/a | No social card at all |
| `meta name="description"` | present (`:7`) | **MISSING** | Acceptable given `noindex`, but incomplete |
| `og:title` / `og:description` / `og:type` / `og:url` | present (`:10–13`) | **MISSING** | Acceptable given `noindex` |
| `robots.txt` | — | — | **Not in repo on `main`** |
| `sitemap.xml` | — | — | **Not in repo on `main`** |
| Structured data (JSON-LD) | MISSING | MISSING | No `application/ld+json` |

**Consequence:** any share of `dbsdc.com` on LinkedIn, Slack, or WhatsApp currently renders with no image — a bare text card.

### Domain inconsistency

`index.html:13` declares `og:url` as `https://www.dbsdc.com` (**with** `www`). The upgrade plan specifies canonical `https://www.dbsdc.com/`, which is consistent. Note that branch `claude/add-contact-persona-9dlZa` uses `https://dbsdc.com` (**without** `www`) throughout its meta and JSON-LD. **These must not be mixed** — a canonical and an `og:url` pointing at different hosts is worse than neither. Decide one host before either change lands.

### Title tags

Unique, verified:

- `index.html` — `DBS Development & Consulting – Stakeholder Alignment & Delivery`
- `thank-you.html` — `Thank You – Your Reports Are Ready | DBS Development & Consulting`

---

## 5. Copy inconsistencies found (inputs to Phase 1)

### 5.1 Response promise — 4 instances, 2 different promises

| Line | Text |
|---|---|
| `index.html:1569` | "I'll respond within **24 hours**." |
| `index.html:2128` | "…I'll reply within **one business day**." |
| `index.html:2176` | "I respond to all inquiries within **24 hours**" |
| `index.html:2184` | "…I'll respond within **24 hours** with initial thoughts…" |

### 5.2 Metric claims — locations of bare/unlabelled figures

| Claim | Locations |
|---|---|
| €500M+ | `:7` (meta description), `:11` (og:description), `:1566` (hero subtitle), `:1765` (track record intro — already carries "(2004–2025)"), `:2065` (about intro) |
| €130M | `:2073` ("Transactions exceeding €130M", inside DBS career description) |
| 711 units | `:1771` (case heading), `:1795` (bullet), `:1806` (standalone `stat-value`), `:2065` (about intro), `:2091` (HB Reavis career description) |
| 20+ years | `:1566` ("20+ years of experience"), `:2065` ("20+ years leading investments…") |

### 5.3 Stale badge

`index.html:1967` — `<span class="research-badge">Q1 2026 Research Available</span>`

### 5.4 Trust bar

`index.html:1575–1590`. Single label `Selected Clients & Mandates` (`:1577`), one flat list of 9 entries:

`HB Reavis`, `BZ Group`, `ColosseoEAS`, `PRANGL`, `K CERO Invest`, `Government of Slovakia`, `BKS Bank`, `Cornerstone IM`, `Braun Holding`

---

## 6. Branch state

| Branch | Last commit | Date | Relationship to `main` |
|---|---|---|---|
| `main` | `25537dd` | 2026-03-23 | baseline |
| `origin/claude/add-contact-persona-9dlZa` | `3759141` | 2026-03-23 | 6 commits ahead, 0 behind — **fast-forwardable** |
| `release-1/phase-1-quick-fixes` | — | 2026-07-20 | this branch, off `main` |

Note the branch name is `…-9dlZa` (lowercase L), not `…-9dIZa`.

See the Gate 0 report for the full assessment of the persona branch and its overlap with Phase 1.

---

## 7. Gate 0 Reconciliation Addendum (2026-07-21)

This addendum records the production-truth reconciliation performed at Gate 0.
It supersedes any earlier assumption in this document that `main` represents
production.

### 7.1 `main` is NOT production truth

The old `main` (`25537dd`, 2026-03-23, "Add files via upload") is a 2,299-line
single-file build with **inline CSS**. **Live `dbsdc.com` does not serve this.**
Live production serves the persona-refactor build (external `styles.css`,
1,100-line `index.html`, Katarína Činčurová, `office@dbsdc.com`, plural voice,
`og-image.png` + social meta, `robots.txt`, `sitemap.xml`).

### 7.2 Production == persona branch (verified byte-identical)

A forensic public snapshot of live `dbsdc.com` (both hosts) was captured on
2026-07-21 and compared to `origin/claude/add-contact-persona-9dlZa @ 3759141`.
**Every served file is byte-for-byte identical (12/12, SHA-256):** `index.html`,
`styles.css`, `thank-you.html`, `robots.txt`, `sitemap.xml`, `og-image.png`, all
favicons, founder photo, logo. Server is `openresty`; homepage `last-modified`
is 2026-03-23 (matches the persona commit date).

**Consequence:** there is **no live-only authored content**. The previously
suspected "production-ahead" items — `ensentia`, `RSBC`, `Schönfeld`, and the
`2004` range — are all present in the persona branch already (and in `main`).
`Tatra banka`, `Česká spořitelna`, `Procter & Gamble` are absent from live
(they are the three NEW approved trust-bar names). **No source reconstruction
was required.**

### 7.3 Backup / rollback status

| Tag | Points to | Meaning |
|---|---|---|
| `backup/pre-release-1-2026-07-20` | old `main` `25537dd` | **Historical stale-main checkpoint.** NOT a valid production rollback point (not what production serves). Preserved; not moved/deleted. |
| `backup/live-public-2026-07-21` | audit snapshot commit | **Forensic public snapshot** of served bytes. Emergency recovery evidence. |
| `origin/claude/add-contact-persona-9dlZa @ 3759141` | authored source | **Actual rollback candidate** — verified byte-identical to production, with full git history. |

- **Authored/origin deployment package:** NOT recovered (no FTP/CI/hosting
  artifact available locally). Stated explicitly, not assumed. The persona
  branch is the verified authored equivalent and serves this role.

### 7.4 CDN / edge

Production carries one baked-in Cloudflare artifact in the static HTML — a
`/cdn-cgi/.../email-decode.min.js` `<script>`. Emails are already clean
`mailto:` links, so this script is removed from authored source in the Phase 1
layer. No other edge transformations were found in the served HTML (openresty
serves the static file verbatim).

### 7.5 Host / redirect (infrastructure)

Both `https://dbsdc.com/` and `https://www.dbsdc.com/` return `200` with
**identical bodies and NO redirect**. The approved canonical host is
**non-www** (`https://dbsdc.com/`); repo metadata is set accordingly. The
required `www → 301 → dbsdc.com` redirect is **missing** and is a **manual
hosting/Cloudflare action for Dávid** (out of scope of this repo work).

### 7.6 Branch topology produced at Gate 0

| Branch | Role |
|---|---|
| `audit/live-snapshot-2026-07-21` | forensic snapshot + evidence (tag `backup/live-public-2026-07-21`) |
| `reconciliation/live-source-2026-07-21` | `origin/main` + merged persona `3759141`; tree == production. No reconstruction commits needed. |
| `release-1/phase-1-quick-fixes` | reconciliation + this inventory + Phase 1 quick fixes → **PR head** |

Gate 0 remains **CLOSED** pending human review.

