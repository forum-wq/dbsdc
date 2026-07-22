# DBS Website — Gate 0 Report

**Date:** 2026-07-21 (work) · **Repo:** `github.com/forum-wq/dbsdc`
**Scope:** Gate 0 production reconciliation + Release 1 Phase 1 quick fixes.
**Gate 0 status:** **CLOSED — READY FOR HUMAN REVIEW.** No live deployment,
no Tally/Cloudflare/DNS changes.

---

## 1. Repository & branch state before work

```
* release-1/phase-1-quick-fixes  d5c3065  docs: Phase 0 current-state inventory
  main                           25537dd  Add files via upload  (= origin/main)
  origin/claude/add-contact-persona-9dlZa  3759141  (6 commits ahead of main)
  tag backup/pre-release-1-2026-07-20 -> 25537dd
```
Untracked (preserved, untouched): `.DS_Store`, `choose Brno.png`,
`choose Olomouc.png`, `linda fsf.png`, `logo_DBS.jpg`.
`gh` authenticated as `forum-wq` (repo + workflow scopes).

## 2. Why `main` was not production truth

Old `main` (`25537dd`, 2026-03-23) is a 2,299-line **inline-CSS** build.
Live `dbsdc.com` serves the **persona-refactor** build (external `styles.css`,
1,100-line `index.html`, Katarína Činčurová, `office@dbsdc.com`, plural voice,
`og-image.png`, social meta, `robots.txt`, `sitemap.xml`). `main` ≠ production.

## 3. Production backup status

- **Authored/origin deployment package:** NOT recovered. No FTP/CI/hosting
  artifact locally (no `netlify.toml`, `wrangler.toml`, `.github/`, deploy
  config). Stated explicitly.
- **Forensic public snapshot:** captured 2026-07-21, branch
  `audit/live-snapshot-2026-07-21` (`c20818b`), tag `backup/live-public-2026-07-21`.
  Raw served bytes for both hosts, HTTP headers, redirect matrix, external-URL
  inventory, SHA-256 manifest. `openresty` origin; homepage `last-modified`
  2026-03-23.
- **`backup/pre-release-1-2026-07-20`** (`25537dd`): historical **stale-main**
  checkpoint — preserved, NOT a valid production rollback point.
- **Actual rollback candidate:** `origin/claude/add-contact-persona-9dlZa`
  (`3759141`) — verified byte-identical to production, full git history.

## 4. Production ↔ persona ↔ main comparison (buckets)

Verified by SHA-256 across all served files + full normalized content review.

- **A — Identical (production == persona `3759141`):** `index.html`,
  `styles.css`, `thank-you.html`, `robots.txt`, `sitemap.xml`, `og-image.png`,
  `favicon.ico`, `favicon-32.png`, `favicon-16.png`, `apple-touch-icon.png`,
  `DavidBoruta_edited_FINAL_K4A9852_copy.jpg`, `vector-logo-DBS-review1.png`
  → **12/12 MATCH.**
- **B — Production ahead of persona:** **NONE.** `ensentia`, `RSBC`,
  `Schönfeld`, `2004` range all exist in persona (and in `main`) — not
  live-only.
- **C — Persona ahead of production:** **NONE** (identical).
- **D — Server/CDN transformations excluded from source:** one baked-in
  `/cdn-cgi/.../email-decode.min.js` `<script>` (removed in Phase 1; emails
  already clean `mailto:`). No other edge transformation in served HTML.

### Item-by-item verification

| Item | Status |
|---|---|
| Katarína Činčurová | present (persona==live) |
| CZ + SK office / contact | present (Praha seat; CZ Šumavská, SK Lubinská) |
| `office@dbsdc.com`, `david@dbsdc.com`, `katarina@dbsdc.com` | clean `mailto:` |
| Singular "I" vs plural "we" | plural throughout |
| Response promises | were mixed (24h/one business day) → unified in Phase 1 |
| `og-image.png` + `og:image`/`:width`/`:height`/`:alt`/`og:site_name` | present, non-www |
| Twitter card | full `summary_large_image` |
| canonical | was MISSING both pages → **added** in Phase 1 (non-www) |
| JSON-LD | present (`ProfessionalService`), non-www |
| `robots.txt` / `sitemap.xml` | present; were **www** → switched to non-www |
| ensentia / RSBC / Schönfeld | present in persona+main+live (not live-only) |
| track-record 2004–2026 | present → removed (unverified end year) in Phase 1 |
| footer email links | clean `mailto:` |
| committed `/cdn-cgi/` markup | 1 script → **removed** in Phase 1 |
| `www` vs non-www references | mixed (og non-www, robots/sitemap www) → all non-www |
| external PDF/report URLs | 5 usrfiles.com PDFs (see §9) — unchanged |

## 5. Reconciliation (Decision 1 — modified Option A)

- `reconciliation/live-source-2026-07-21` (`37defb4`) = `origin/main` +
  `--no-ff` merge of persona `3759141`. Reconciled tree **verified identical**
  to persona/production. **No reconstruction commits needed.**
- `release-1/phase-1-quick-fixes` rebased `--onto` reconciliation; the docs-only
  inventory commit is preserved (`d5c3065` → replayed). Phase 1 edits anchored
  **semantically** to the 1,100-line refactored structure (no reliance on old
  2,299-line line numbers).

## 6. Approved Phase 1 changes applied

1. **Research badge** "Q1 2026 Research Available" → "2026 Data Center Research".
   (Report-edition label "Q1 2026 | 16 pages | IC-ready" retained — it factually
   names the specific report PDF, not a stale availability badge.)
2. **Response promise** unified to "We'll respond within one business day."
   (all "within 24 hours" and reply/respond variants removed; 4 instances).
3. **Hero/about metric descriptors** — approved evidence-register framing:
   "€500M+ founder career track record", "20+ years across CEE"; DBS block
   "€130M+ in DBS mandates since 2015". Founder-career and DBS-entity metrics
   are no longer presented in the same category.
4. **Track record** — removed unverified "2004–2026"; now "a selected track
   record spanning 20+ years across Central & Eastern Europe". (The `2004 – 2007`
   career-period entry is a real employment date and is retained.)
5. **Trust bar** — final 12-name, two-group, accessible structure (see §11).
6. **SEO / social** — canonical added to both pages (non-www); `og:url`
   normalised; `robots.txt` + `sitemap.xml` switched to non-www; og/twitter/
   JSON-LD already non-www; no duplicate metadata blocks.
7. **Authored email links** — removed baked-in `/cdn-cgi/` script; `mailto:`
   links kept.
8. **thank-you.html** — kept `noindex`; canonical added; links/assets verified.
9. **External PDFs** — inventoried only (see §9); no live migration, no Tally
   change.
10. **Scope boundary honoured** — no redesign, Insights/Work hub, Tally rebuild,
    GA4/GTM, consent banner, logo redesign, or Release 2 work.

## 7. Host / redirect matrix (infrastructure)

| Host | Final | HTTP | Redirects | Server |
|---|---|---|---|---|
| `https://dbsdc.com/` | `https://dbsdc.com/` | 200 | 0 | openresty |
| `https://www.dbsdc.com/` | `https://www.dbsdc.com/` | 200 | 0 | openresty |

**Approved host: non-www.** Required `www → 301 → dbsdc.com` is **MISSING** —
**manual hosting/Cloudflare action for Dávid.** Repo metadata is prepared for
the non-www target regardless.

## 8. QA & preview results

Rendered locally with Chrome 150 (headless + CDP mobile emulation).

- Homepage & thank-you load; **no console errors**; all local assets resolve.
- **Horizontal mobile overflow: NONE** — measured at 390px mobile emulation,
  `document.scrollWidth = innerWidth = 390`. (Off-canvas mobile menu + decorative
  gradient are `position:fixed/absolute`, clipped, no document scroll. A raw
  `--window-size` screenshot appears clipped for BOTH this build and live prod —
  a headless artifact, not overflow.)
- Desktop + mobile header render correctly (hamburger on mobile).
- **Both trust-bar groups legible**; all 12 names appear **exactly once** in the
  intended grouping; no organisation accidentally removed.
- Email/phone/CTA links resolve; report links inventoried.
- canonical non-www ✓; OG + Twitter valid ✓; **JSON-LD parses** ✓;
  `robots.txt`/`sitemap.xml` non-www + valid XML ✓; **no `/cdn-cgi/` in authored
  source** ✓.
- Static searches run: `www.dbsdc.com`=0, `/cdn-cgi/`=0 (authored), `24 hours`=0,
  `one business day`=4, `I'll respond`=0, `We'll respond`=4, `Q1 2026`=1
  (intentional report label), `2004–2026`=0, all 12 trust names=1 each.
- Screenshots: `docs/qa/homepage-desktop-1280.png`,
  `docs/qa/homepage-mobile-390.png`.

## 9. External PDF inventory (unchanged — not migrated)

5 Wix-hosted `usrfiles.com` PDFs (host prefix
`80496fad-0a78-43cd-b632-94e9cfa5a1f9.usrfiles.com/ugd/`): Q1 2026 DC research
report, CEE Growth Frontier, DC Real Estate & Land Development, Privacy Policy,
Terms of Service — 7 link instances across `index.html` + `thank-you.html`.
Full list in `audit/live-2026-07-21/external-urls.txt`. **No migration
performed** (needs verified source PDFs + human review; no Tally change).

## 10. Evidence-register status (new trust-bar names)

`docs/evidence/trust-bar-evidence-register.md`. **Tatra banka**,
**Česká spořitelna**, **Procter & Gamble** — register rows **COMPLETED and
approved by David Bořuta on 21.7.2026** (role/title, period, evidence source =
employment contract + paychecks, approved public wording = name only,
public-use permission granted). **Deploy gate for these names is CLEARED.**
Status is internal, never shown on page; the page shows name only.

## 11. Final trust-bar grouping

**Founder executive track record:** HB Reavis · BZ Group · Government of
Slovakia · Tatra banka · Česká spořitelna · Procter & Gamble
**Selected DBS-era mandates:** ColosseoEAS · PRANGL · K CERO Invest · BKS Bank ·
Cornerstone IM · Braun Holding
(9 originals kept, 3 founder names added, nothing removed, BKS Bank not
reclassified.)

## 12. Rollback procedure

- **Preferred authored rollback:** `git checkout origin/claude/add-contact-persona-9dlZa`
  (`3759141`) — verified byte-identical to production.
- **Forensic bytes:** tag `backup/live-public-2026-07-21` (branch
  `audit/live-snapshot-2026-07-21`).
- **Nothing is deployed by this work**; production is untouched, so no rollback
  is currently needed. The PR is not merged.

## 13. Remaining risks / blockers

- **Blocker (infra, not repo):** `www → 301 → non-www` redirect missing —
  manual Cloudflare/hosting action.
- **Deploy gate:** ~~3 new founder names need evidence-register completion~~
  **CLEARED** — all three approved by David Bořuta on 21.7.2026 (see §10).
- No authored origin deployment package exists; persona branch is the trusted
  authored equivalent.
- `usrfiles.com` PDFs remain Wix-hosted (future migration).

## 14. Gate 0 recommendation

**READY FOR HUMAN REVIEW.** Reconciliation is complete and verified, Phase 1
quick fixes are applied and QA-clean, evidence and rollback paths are recorded.
Gate 0 remains **CLOSED**: no merge, no deploy, no Tally/Cloudflare/DNS changes
until Dávid reviews.

**No live deployment was performed.**
