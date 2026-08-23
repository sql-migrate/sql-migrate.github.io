# Website redesign — "The diff is the brand" (v2)

Source brief: `~/Downloads/redesign/DESIGN-SPEC.md` + `postgrescompare-design1-full.html`
(the mockup is ground truth where the two disagree).
Related: `~/.claude/plans/for-v2-we-need-logical-dijkstra.md` (the actual v2 free-tier design).

Branch: `website-redesign-v2`, cut from `website-redesign`.

---

## Decisions taken

| Question | Decision |
|---|---|
| Pricing | **$29.95/user/month** (`price_1U2gmnKhvEuHFknZ7qYklVhI`), **$299.95/year** (`price_1U2gmSKhvEuHFknZrkJItaph`). Annual saves $59.45 — stated as the saving, not "2 months free", because two months would be $59.90. |
| Grandfathering | **Not mentioned.** Drop the mockup's "existing subscribers keep their price" footnote. |
| Version framing | Launch as **v2.0**. |
| Trial | **14 days**, then lands on the free tier — no expiry cliff. |
| Free tier contents | Per the v2 licensing plan, not per the mockup (see below). |

---

## Correction 1 — the mockup's pricing story is wrong

The mockup sells: *"Free to see. Paid to act. Viewing differences is free forever.
Deployment scripts, history, and team workflows are what you pay for."*

The real v2 free tier is far more generous. From the licensing plan:

**Free includes:** unlimited comparisons over unlimited objects, all 40+ object types,
the diff/DDL viewer, **deployment script generation**, **deploying to a database**,
data (row) comparison, snapshots, exports, all `Ignore*` options and object-type toggles.

**Free is limited to:** holding **one comparison at a time** (per project). Comparisons
created during the trial stay openable forever and don't count against the limit.

**Paid (subscription or active trial):** comparison history, pipelines, the `pgc` CLI
(so CI/automation stays paid), parallel connections, include/exclude object rules,
pre/post-deploy scripts.

### Replacement homepage pricing copy

```
@@ pricing @@
Free to compare. Paid to automate.
Comparing, scripting and deploying are free forever.
History, pipelines and the CLI are what you pay for.
```

## Correction 2 — misattributed testimonial

The mockup credits *"I needed a quick schema diff and got the answers I needed in under a
minute"* to Scott Hanselman. That is **Richard Hayes (@justcompile)**.
Hanselman's real quote: *"Clever and complete tool to compare PostgreSQL Schemas and Diff
Databases."* Use real attributions everywhere, including the review JSON-LD.

## Correction 3 — unverifiable claim

Drop **"Faster engine — large schemas compare in a fraction of the time"** from the v2
changelog card unless a benchmark exists. Spec §10 requires verifiable claims.

---

## Phase 1 — Token foundation

Highest leverage step. `_sass/variables.scss` (23 lines) drives ~2,846 lines of SCSS via
`$primary`/`$text`/`$border`/`$success`. Remapping those aliases reskins docs, FAQ, blog,
forms and contact with no per-file work.

- **New `_sass/tokens.scss`** — the 15 spec tokens as CSS custom properties on `:root`,
  plus `--sans` (Inter) and `--mono` (JetBrains Mono).
- **Rewrite `_sass/variables.scss`** as aliases onto the new palette:

  | Old | New |
  |---|---|
  | `$primary` `#0F766E` | `#336791` (blue) |
  | `$primary-dark` `#115E59` | `#27506F` (blue-deep) |
  | `$primary-light` `#14B8A6` | `#336791` |
  | `$text` `#1F2937` | `#1B211D` (ink) |
  | `$text-light` `#6B7280` | `#5A645D` (ink-soft) |
  | `$background` `#FAFAFA` | `#F7F8F6` (paper) |
  | `$surface` `#FFFFFF` | `#FFFFFF` (panel) |
  | `$border` `#E5E7EB` | `#E4E8E3` (line) |
  | `$success` `#10B981` | `#1E7B3C` (add) |
  | `$shadow` | `rgba(27,33,29,.24)` |
  | `$accent` / `$accent-light` (amber) | no equivalent — map to `$primary`, then find and remove uses |

  Keep the breakpoint vars. Add `$mid` `(max-width: 880px)` for the new components.
- **`css/screen.scss`** — import `tokens` first, add a `diff` partial last.
- **`_layouts/default.html`** — replace the Plus Jakarta Sans + Inter font link with
  Inter (400–800) + JetBrains Mono (400/500/700), `display=swap`, add `preconnect` to
  `fonts.googleapis.com` and `fonts.gstatic.com`.
- **Hardcoded hex sweep** — teal/amber literals live in `index.html`, `docs/index.html`,
  `downloads.md`, `download-links.md`, `favicon.svg`, `order-cezannehr.md`.

**Checkpoint:** build and walk every interior page. The site should already read as the new
system before any new component exists.

## Phase 2 — Shell (nav, footer, a11y floor)

- **Logo** — replace the `.logo-icon` "P" circle in `_layouts/default.html` with the `±`
  mono chip: white on `--blue`, 5px radius, mono 700.
- **`_data/navigation.yml`** — trim 9 items to 5: Features, Pricing, Docs, Changelog,
  Download free. Rename labels only; URLs unchanged per spec §9
  (`Buy Now`→`Pricing` → `/purchase`, `Release Notes`→`Changelog` → `/release-notes`,
  `Download`→`Download free` → `/downloads`). About/FAQ/Contact/Articles are already in
  the footer.
- **`_sass/navigation.scss`** — sticky, `rgba(247,248,246,.92)` + `backdrop-filter: blur(8px)`,
  64px tall, bottom border `--line`, sentence case. Keep the existing hamburger
  (spec says don't *add* one; this predates it).
- **`_sass/footer.scss`** + `default.html` — keep the data-driven 4-column structure,
  restyle to tokens, replace the copyright line with the mono
  `© 2026 postgrescompare · built by one person, used before every deploy`.
- **Accessibility floor** — skip-to-content link, global `:focus-visible` (2px `--blue`,
  3px offset), applied in `_sass/elements.scss`.

## Phase 3 — Homepage

Rebuild `index.html` to spec §5. New `_sass/diff.scss` carries the signature components.

1. **Hero** — `@@ v2.0 · schema & data comparison for postgresql @@`; the two-line diff
   headline (struck red line above solid green line — do not soften); sub-copy at 44ch;
   "Download free" + "See pricing"; mono trust line
   `runs 100% locally · no credit card · macOS · Windows · Linux`.
   **Delete** the typewriter script and its inline `<style>`.
2. **Diff panel** — coded, not a screenshot. Title bar `orders — staging → production`,
   column headers, the orders/currency/legacy_ref diff verbatim from the mockup, the
   `DROP COLUMN legacy_ref` warning chip, footer buttons. Staggered rise-in animation
   inside `@media (prefers-reduced-motion: no-preference)`. `aria-label`.
   **Delete** the carousel markup and its ~50 lines of JS; screenshots relocate to a
   features or docs page per spec §9.
3. **Features 2×2** — six current cards compress to four, tagged `compare`, `deploy`,
   `track`, `+ new in v2` (MCP server, green). Emoji icons removed — outside the palette
   and off-tone. Source chips beneath: live databases / pg_dump files / migration folders /
   any combination of the three.
4. **Local band** — full-bleed white band, copy + the ASCII file-tree diagram with
   `aria-label`.
5. **What changed in v2** — changelog card, title bar `git log v1..v2 --oneline`.
   Green: free tier, MCP server, data comparison. Red: the 30-day cliff. No "faster engine".
6. **Quote** — Hanselman, correctly attributed, in `<blockquote>`/`<cite>`. The other four
   testimonials (Hayes, Langille, Wellspring, Booz) become a compact mono strip near
   pricing rather than being dropped.
7. **Pricing** — three cards per the corrected table below.
8. **Final CTA** — dark `--ink` band, white button, muted mono trust line.
9. **JSON-LD** — `softwareVersion` → `2.0`, offer price `14.95` → `29.95`, keep all five
   reviews with their real authors.

### Corrected pricing cards

| | **Free** — $0 | **Pro** — $29.95 /user/mo | **Team** — volume pricing |
|---|---|---|---|
| note | after your 14-day trial | $299 billed yearly — 2 months free | for teams and consultancies |
| + | Unlimited comparisons, all 40+ object types | Everything in Free | Everything in Pro |
| + | Live databases, pg_dump files, migration folders | Full comparison history & drift timelines | Volume discounts |
| + | Dependency-ordered deployment scripts | Pipelines | Client-transferable licences |
| + | Deploy, with destructive-change safeguards | The `pgc` CLI for CI and automation | Invoice billing |
| + | Data comparison, snapshots, exports | Parallel connections, include/exclude rules | Priority support |
| − | Holds one comparison at a time | | |
| − | No pipelines, no CLI | | |

No grandfathering footnote.

## Phase 4 — Purchase page  ⚠️ BLOCKED

`purchase.md` hardcodes **live** Stripe price IDs against a live publishable key:

- monthly `price_1IOXkqKhvEuHFknZlCwbX1eV` ($14.95)
- annual  `price_1IOXlOKhvEuHFknZBrJxEeZP` ($149.95)

Moving to $29.95/$299 requires **two new Stripe prices created first**. Build the page
with clearly-marked placeholder IDs; do not merge until the real IDs are pasted in.

- Three cards mirroring the homepage. Team → `/contact`.
- Product JSON-LD offers → 29.95 / 299.
- Pricing FAQ rewrites: "Can I try before I buy?" → 14-day trial that becomes the free
  tier, not an expiry. New entries: what the free tier includes, the one-comparison limit,
  client-transferable licences. The 30-day money-back guarantee entries stay unless that
  policy is also changing.

## Phase 5 — Site-wide copy sweep

"Free 30-Day Trial" / "30-day trial" → "14-day trial, then free forever" across
`index.html`, `_config.yml` (site description), `downloads.md`, `download-links.md`,
`faq.md`, `purchase.md`, and the docs. Sentence case on buttons and headings throughout.
Interior page heroes gain hunk-style labels (`@@ frequently asked questions @@`).

## Phase 6 — Assets

`favicon.svg` (currently teal) → blue rounded square with a white mono `±`. Then regenerate
`favicon.ico`, `favicon.png`, `apple-touch-icon.png`, `touch-icon.png`, `siteicon.png`.
Needs ImageMagick locally, or export by hand from the SVG.

## Phase 7 — Verification

`bundle exec jekyll serve` (Jekyll 4.4.1 confirmed working). Check:
880px breakpoint on hero/features/pricing/local band; headline wrap at 360px; diff panel
`overflow-x` on mobile; keyboard focus on every interactive element; reduced-motion path;
docs/FAQ/blog/contact inherited the tokens cleanly; no orphaned teal or amber.

---

## Open items

1. **Stripe price IDs** for $29.95 / $299 — hard blocker on Phase 4 only.
2. **v2.0 timing** — the site will claim v2.0 while `downloads.md` serves 1.2.2 artifacts.
   Hold the launch for the app release, or ship the reskin now and flip version strings on
   release day?
3. **"Faster engine"** — needs a benchmark or it stays out.
4. **small.chat widget** — already loads in production via `default.html`. Spec forbids
   *adding* a chat widget; this one predates the work. Leaving it in place.

## Sequencing

Phases 1–3 land first as one reviewable unit on `website-redesign-v2` — no money paths
touched. Phases 4–6 follow once Stripe is sorted.

---

## Status

| Phase | State |
|---|---|
| 1 — Token foundation | done (`10083b8`) |
| 2 — Shell | done (`0458858`) |
| 3 — Homepage | done (`7558c74`) |
| 4 — Purchase page | done — Stripe IDs supplied 2026-08-22 |
| 5 — Copy sweep | done (`9606d5e`) |
| 6 — Assets | done (`803b0a0`) |
| 7 — Verification | done |

### Found during verification, and fixed

- Three palette values failed the WCAG AA floor the brief itself sets in
  section 8. `--gutter` #8A9490 measured **2.93:1** on paper (it carries the
  hunk labels, trust lines, footer headings and pricing notes) and is now
  #6A7470. The excluded pricing rows measured **1.99:1** and **2.06:1** and are
  now #6D7970 / #9A6B61. The dark band's trust line went #7C877F → #7E8981.
  Every text pair on the site now clears 4.5:1.
- `--faint` #C4CBC5 is kept as specified. It only ever renders diff line
  numbers inside a `role="img"` panel that carries its own description, so it
  is incidental text.
- Blog posts had **no `<h1>` at all** — `_layouts/post.html` rendered only the
  category/date line, and every body heading started at `<h2>`. Ninety pages
  were affected. Fixed in the layout.
- `pg_catalog/` (76 files) is a DB Doc frameset with no site layout, no skip
  link and ~600 images without alt text. Pre-existing, sitemap-excluded, and
  out of scope for a reskin — but worth knowing it is there.
- `order-cezannehr.md` still carries an amber "amended" badge. It is a client
  order form with self-contained styles and its own semantics, deliberately
  left alone.
- `/downloads/` (trailing slash) fails a strict static link check but resolves
  on both Jekyll and GitHub Pages. Pre-existing, not a regression.

### Left alone deliberately

- `order-cezannehr.md` quotes **$149.95/user/year**. That is a negotiated client
  order form, not site pricing, so the sweep did not touch it. Whether that deal
  gets repriced against the new $299.95 is a commercial decision, not a site one.

---

## Content pass (2026-08-23)

Five problems found by reading the homepage against what the product actually
does, four fixed:

1. **Pipelines and the CLI appeared only in the pricing table.** "Paid to
   automate" was the pricing headline for two features the page never
   introduced. The feature grid is 2x3 now, with an `automate` cell covering
   the `pgc` CLI and pipelines.
2. **"Every comparison is recorded" contradicted the free tier**, which holds
   one at a time. Paid capabilities in the grid now carry a `pro` marker on
   their mono tag, so the grid and the pricing table cannot drift apart.
3. **The MCP card undercut the local-only band.** It claimed "your schema stays
   on your machine while your agent does the legwork" — but a hosted agent
   reading a diff is exactly what a security reviewer would care about. It now
   describes the real controls: read-only mode and environment allow-lists.
4. **The screenshots were orphaned** by the carousel deletion. All three are
   rehomed in the docs where they are instructive rather than decorative:
   two in Quick start, one in the Deployment scripts guide, framed and
   captioned.
5. **v2.0 timing** — still a decision, but no longer a chore. `app_version` and
   `marketing_version` live in `_config.yml`; the homepage and product schema
   use the marketing one, the download pages the app one. Flipping on release
   day is two lines.

Deleting the changelog section had also silently dropped data comparison from
the homepage — it now has its own `data` cell.
