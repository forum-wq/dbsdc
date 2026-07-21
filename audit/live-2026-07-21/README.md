# DBS Production — Forensic Public Snapshot (2026-07-21)

**Purpose:** Gate 0 evidence + emergency recovery material. Forensic capture of
the *publicly served* production site. This is **not** authored source; it is a
byte-for-byte record of what `dbsdc.com` served at capture time.

- **Captured:** 2026-07-21 (UTC), via `curl`, User-Agent `GateAudit`.
- **Retrieval method:** direct HTTPS GET, raw bytes preserved (no normalization).
- **Server observed:** `openresty` (origin); not behind a Cloudflare proxy at
  capture time.
- **Homepage `last-modified`:** Mon, 23 Mar 2026 22:25:28 GMT.

## Contents
- `public/` — raw served files (HTML both hosts, styles.css, thank-you.html,
  robots.txt, sitemap.xml, og-image.png, favicons, founder photo, logo).
- `headers/` — raw HTTP response headers for both hosts.
- `SHA256SUMS.txt` — checksum manifest for every file in `public/`.
- `redirect-matrix.txt` — www / non-www host + redirect results.
- `external-urls.txt` — Tally, usrfiles.com PDFs, LinkedIn, fonts, tel, host refs.

## KEY FINDING — production == persona branch (exactly)

Every file served by production is **byte-for-byte identical** to the committed
blobs on `origin/claude/add-contact-persona-9dlZa` @ `3759141`, verified by
SHA-256 (12/12 assets MATCH):

    index.html · styles.css · thank-you.html · robots.txt · sitemap.xml
    og-image.png · favicon.ico · favicon-32.png · favicon-16.png
    apple-touch-icon.png · DavidBoruta_edited_FINAL_K4A9852_copy.jpg
    vector-logo-DBS-review1.png

**Implications:**
- Production was deployed from the persona branch (or an identical tree).
- There is **no live-only authored content** ahead of the persona branch.
  `ensentia`, `RSBC`, `Schönfeld`, and the `2004` range all exist in the
  persona branch already (and in `main`) — they are NOT live-only.
- The **authored-source rollback candidate is the persona branch itself**
  (`3759141`), which carries full git history — stronger than this scraped
  snapshot. This snapshot is corroborating forensic evidence.

## Provenance status
- **Authored/origin deployment package:** NOT recovered. No FTP/CI/hosting
  artifact was available locally (no netlify.toml, wrangler.toml, .github/,
  deploy config). Stated explicitly, not assumed.
- **This snapshot** is forensic public evidence, tagged
  `backup/live-public-2026-07-21`. It is emergency recovery material, not an
  automatically-valid authored-source rollback point — but see above: the
  persona branch is the verified authored equivalent.
- **`backup/pre-release-1-2026-07-20`** points to the OLD `main` (`25537dd`,
  2026-03-23 "Add files via upload"), which is a 2,299-line inline-CSS version
  that is NOT current production. It is a **historical stale-main checkpoint**,
  preserved, not moved/deleted.
