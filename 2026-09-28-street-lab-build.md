# Street Lab build — 2026-09-28

Parts 1–9 of the STREET LAB BUILD brief are done. The redesign is on a **preview deployment only**; production (www.kalios.health) is untouched and IndexNow was not run.

- **Preview:** `dpl_4HmUneLxQdzWxSxoqek46AMeQMdn`, state READY, target preview (not production). Built from commit `95f1724` via the git-less copy (CLAUDE.md rule 4). The preview sits behind Vercel login; open it from the Vercel dashboard or `vercel inspect dpl_4HmUneLxQdzWxSxoqek46AMeQMdn`. Its `*.vercel.app` URL is not printed here because it carries the team slug.
- **Commits (8, all pushed):** `0f6e786` Part 1 · `4a4f5fa` Part 2 · `a8c40df` Part 3 sample · `f052015` Part 3 · `5657ff5` Part 4 · `427f72e` Part 5 · `b50468b` Part 6 · `95f1724` Part 7. This report is the 9th.
- **Scale:** 998 files changed. 456 pages carry the new header, fine-print strip and footer; 122 compound and stack pages use the new hero; 323 pages no longer load the Tailwind CDN; 456 OG images.

**Session start (rule 2)**
- Claude Code 2.1.283.
- Vercel CLI 60.1.3, the same as the latest on npm.
- `brew outdated` lists 38 formulae (e.g. `git`, `node`, `python@3.12`, `openssl@3`). None were updated.
- Build-time tools, pinned: Tailwind CLI 3.4.17 (the last v3; the CDN pages were v3, so the compile matches them). Screenshot/Lighthouse tooling (puppeteer-core 25.12, Lighthouse 13.5) lived in the session scratchpad, not the repo.

---

## Look at these first

1. **The homepage and the new `/compounds/` page.** Both are new. The homepage copy departs from the artboard in three places (DECISIONS 22); the grid of all compounds moved from the homepage to `/compounds/`.
2. **The primary action on every compound page ("Bought a vial? Check the paperwork") and the Paperwork door** go to `/stack.html`, as the brief said. That page didn't exist, so it is now a small noindex placeholder ("Not built yet"). Keep it, or hide the button until `/coa` exists?
3. **The caution band is on 92 pages.** 11 PCAC pages read "Not legal to compound — yet"; 81 research-only pages read "Not legal to compound" (no "— yet", DECISIONS 15), DSIP among them with its voted-against notice. GHK-Cu reads "— yet" although non-injectable GHK-Cu is back in Category 1: the open GHK-Cu question, inherited from the data.
4. **The 23 evidence judgment calls** (table below; full list in `gsc-export/card-data.csv`). The ones most worth a second opinion: VIP 3 (its badge says Preclinical), Matrixyl 3, AICAR 3, the three N-acetyl variants 1, and every stack 0.
5. **A pair page.** They now render as designed for the first time: until tonight they were black-on-white with the Venn diagram and tag chips almost invisible (Findings 1).
6. **The alert form** posts to Substack's custom-form endpoint (`https://kaliospeptides.substack.com/api/v1/free?nojs=true`). I did not test-submit it, because a test creates a real subscription. Try it once with your own address.
7. **The red "Updated" dot shows on every card** for now: the 26–27 Sep truth passes touched every page. It thins out from late October.
8. **about.html** (full text at the end). It is still unlinked. It has two draft placeholders and describes the old tag vocabulary.
9. **CLAUDE.md is now out of date:** the nav template, "copy BPC-157's `<style>` byte-for-byte", and the Design System section describe the old site. Update it once the preview is approved. Not done tonight.
10. **Two items the artboard cut but I kept** because CLAUDE.md's locked template lists them: the "← All Compounds" back link and the aka/alias line (DECISIONS 12).

---

## What was built

**Part 1 — the system.**
- `assets/street-lab.css`: the spec's tokens, type scale, all 14 components, typography for the existing content, and root-page overrides.
- `assets/street-lab.js`: the menu, copy buttons and the alert form. Nothing else.
- One template each for the header, the fine-print strip and the footer (`templates/`). `scripts/prerender.py` injects them into every page between `<!-- sl:NAME -->` markers; a second run changes 0 files.
- The fine-print strip reads "Written by G · Checked {date} · {N} sources / No vendors · No affiliates · Nothing to sell".
- Nav: Compounds · Stack tool · Calculator · FDA tracker · Alerts (→ Substack). The About slot is in the template, disabled. No X link anywhere.

**Part 2 — card and stamp data.** `card_stamp`, `evidence_level`, `use_tag` and `red_flag` were added to every compound and stack in `data/regulatory-status.json`. The derivation script is `scripts/card_data.py`; the full table is in `gsc-export/card-data.csv`.

**Part 3 — the compound template.**
- 116 compound pages + 6 stack pages follow v11-Main: header, strip, eyebrow, H1 + one stamp, gist, primary action, caution band, the Quick Facts as a data table, section-nav rows, then the existing sections unchanged, "Usually looked up with", footer.
- Sample first (bpc-157, semaglutide, adipotide, mito-stack), audited, committed; then scaled, audited, committed.

**Part 4 — cards and homepage.**
- Card component per v11-Card; a compact variant per v11-Home.
- New homepage per v11-Home / HomeDesktop, baked by prerender.
- New `/compounds/` with every compound and stack as a card, grouped by use.

**Part 5 — everything else.**
- The Tailwind CDN is removed from 323 pages, replaced by a compiled, scoped utility layer; layout geometry is identical.
- Stack tool, calculator, tracker, guide, compare pages, stacks-index and about are templated.
- New 404 page.
- `stacks/generate.py` now emits the new template; regenerating gives byte-identical files.

**Part 6 — OG images.** One 1200×630 image per page (black, Anton title, drip mark, KALIOS.HEALTH), referenced in each page's meta.

**Part 7 — QA.** Structural audit, screenshots, Lighthouse, behaviour checks (below).

**Part 8 — preview deploy.** Git-less copy, `vercel` without `--prod`, verified with `vercel curl`.

---

## DECISIONS

**Part 1 — system**
1. **Generated vs content.** Everything between `<!-- sl:NAME -->` and `<!-- /sl:NAME -->` is generated and rewritten each run; everything else is page content. One template edit plus prerender updates every page.
2. **"Checked {date}" means content changed, not that a template ran.**
   - `data/page-dates.json` was seeded from each page's last git commit before the first redesign commit: 453 pages, 202 dated 26 Sep and 251 dated 27 Sep.
   - Prerender hashes each page's content text (outside the generated blocks, order-independent) and moves the date to the run date only when that hash changes.
   - Structural migrations ran with `--reseed`, which keeps the date. So tonight's template work does not make any page claim it was checked tonight.
3. **N sources** counts:
   - compound, stack and compare pages: the Key References items
   - pair pages: literature-table rows
   - tracker: the Primary Sources list
   - other pages: the clause is left out
4. **Legal text kept verbatim.** Each page's existing disclaimer ("…consult a licensed healthcare provider… not evaluated by the FDA…") now sits in its own block just above the new three-clause footer. CLAUDE.md requires both lines on every page, and the spec's footer has room for neither. The email address and "© 2026 Kalios Peptides. All rights reserved." move into the footer template. A safety check refuses any footer whose text would be lost.
5. **"Written by G"** follows the brief. CLAUDE.md rule 7 says the author is Kalios on every page. The strip line is G's explicit wording; the JSON-LD author stays "Kalios Peptides".
6. **About slot.** It's in the header template as a documented placeholder that renders nothing. A hidden link would still be crawlable.
7. **Menu.** Without JavaScript the nav shows as rows under the header; with it, the nav collapses behind the 44×44 button. Desktop (≥1024px) shows it inline, per HomeDesktop.
8. **Fonts.** One css2 link for Anton, IBM Plex Mono 400/500/700 and Inter 400–700, with `display=swap` and system fallbacks. Inter 800 is dropped (the spec lists 400–700). The stylesheet loads via preload + onload with a `<noscript>` fallback (Part 7).
9. **No `?v=` cache-buster** on the stylesheet or script. Vercel already serves `max-age=0, must-revalidate`, and a version string would rewrite all 456 pages on every CSS edit.

**Part 2 — card data.** See the judgment-call table below. Rules:
10. **`card_stamp` comes strictly from status:**
    - FDA APPROVED → FDA APPROVED
    - COMPOUNDED RX → COMPOUNDING PHARMACY
    - PCAC RECOMMENDED and PCAC 2027 → NOT LEGAL YET
    - RESEARCH ONLY → RESEARCH ONLY
    - stacks → NOT LEGAL TO COMPOUND
11. **`evidence_level`:**
    - FDA APPROVED → 4.
    - Stacks → 0: no study has tested any stack as a combination, and every stack page says so.
    - Otherwise the page's badge sets the level, and the Human Data section can override it.
    - Counted: published data on the compound itself.
    - Not counted: unpublished results, manufacturer dossiers, parent or analog data, and observational biomarker studies. The artboard sets this rule: it rates adipotide "animal only" despite an unpublished Phase 1.
    - Ex-US approvals (Russia, China, Japan; thymosin α1 in ~35 countries) and historical FDA approvals (sermorelin, gonadorelin) score 3, not 4. On the meter, "Approved drug" means FDA-approved today.

**Part 3 — compound template**
12. **Kept two locked-template items the artboard cut:** the "← All Compounds" back link (now → `/compounds/`) and the aka line (mono, muted, under the H1). Each is one line in `scripts/street_lab_migrate.py` if you'd rather match the artboard.
13. **Gist = the existing hook** (the first bold line of The Gist), moved up. The other five questions stay verbatim in The Gist box after the section nav. There is no second 14.5px line: that needs new copy (the hook-rewrite session). With one line, the primary action sits above the fold on every page checked (at most ~610px down at 390×844).
14. **Evidence stamp.**
    - Text: the page's badge, verbatim.
    - Colour from the old badge class: green → approved, gold → the COMPOUNDED RX gold, red → research.
    - The four "FDA Approved" badges that were gold → approved green.
    - Stacks: a legal stamp from the data file; the "Stack Protocol" badge is dropped (one stamp per hero).
    - The hero stamp and the card meter can disagree (BPC-157: "Preclinical" vs "Human pilots"), exactly as the artboards show.
15. **Caution band.**
    - Title: "Not legal to compound — yet" for PCAC RECOMMENDED and PCAC 2027. "Not legal to compound" for RESEARCH ONLY, where "yet" would imply a path that doesn't exist.
    - Body, PCAC pages: the 12 existing PCAC notices, moved verbatim into `regulatory-status.json` (`caution_note_html`). The band's text therefore comes from the data file (the spec) and no text is lost.
    - Body, research-only pages: the status vocabulary's first sentence.
    - Stacks get no band: their status isn't one of the spec's three, and the stamp already says it.
16. **H1 size per page.** Measured from Anton's glyph widths, each H1 gets one of 84/72/64/56/48/40px so its longest word fits a 375px phone (e.g. Semaglutide 64, Follistatin-344 48). Desktop is 96px.
17. **Data table = every Quick Fact**, in order and verbatim; the artboard's six rows were an excerpt.
18. **Section nav.**
    - Rows: What it is · What the research shows · Human data · Dosing · Side effects · Legal status · References · N. They're matched by heading pattern (stacks use other headings), and only rows that exist are shown.
    - The only change inside the content: `id` attributes on section wrappers.
    - The artboard's "Who did the research · 80% one lab" row has no data source, so it's omitted.
19. **"Usually looked up with":** the first compound under "Commonly Stacked With" plus the first stack containing the compound (BPC-157 → TB-500 + Wolverine Stack, as in the artboard). Stacks show other stacks that share members, or else their members.
20. **The locked inline `<style>` is gone** from compound and compare pages; `street-lab.css` carries every content rule. Legacy variable names map to the new tokens, so inline `style=""` attributes keep working.

**Part 4 — cards and homepage**
21. **`/compounds/` is a new page,** needed because "Evidence" and "Browse all" point there.
    - It holds all 113 compounds and 6 stacks as cards, grouped by use tag. Each group has a chip; the chips are anchors, so they work without JavaScript.
    - A search filters the cards in place. An exact name, or a single match, opens that page (`?q=Ozempic` → semaglutide). No match shows Karl.
    - The filter is a small inline script on that page, like the calculator's own script. `street-lab.js` stays menu / copy / alert form only.
22. **Copy I changed for accuracy** (G's artboard wording in brackets):
    - Evidence door: "113 compounds. Every page cites its sources." ["Every claim cited." — not every claim carries an inline citation]
    - Paperwork door: "COA check — being built." ["COA Check. Read any lab report in 20 seconds." — the tool doesn't exist yet]
    - Cards label: "Four to start with" ["Most looked up this month" — no traffic data was used]
23. **Homepage structure.**
    - Order follows the brief: chips come before the cards (the spec lists cards first).
    - The four cards are the artboard's four.
    - The search submits to `/compounds/?q=…`, with a `<datalist>` of every name for native suggestions.
    - The visible submit button is screen-reader-only (the artboard shows none; Enter or Go submits).
24. **Legal door and "what changed" line come from data.**
    - The Legal door is computed from `regulatory-status.json`: "PCAC: 6 of 7 recommended. None legal yet."
    - "What changed" comes from a new `data/changelog.json` of public one-liners, because report titles are internal. It shows the newest entry dated on or before the newest report: "27 Sep — FDA status re-checked on every compound page", checked against git (the 27 Sep Category 2 truth pass).
    - Add a changelog entry when the redesign goes live.
25. **Wordmark.** A cropped 480px WebP (18.6 KB, letters only; the spec's drip mark stands in for its red dot) at 200px on phones and 240px on desktop. Today's homepage shows 312px and 640px. `art/kalios-wordmark.png` itself is untouched.
26. **Not built:** the author card in HomeDesktop's aside ("Why Kalios exists →"). It links to About, which isn't signed off.
27. **Homepage metadata.** The JSON-LD is kept except the X profile in `sameAs`; `twitter:site` is removed.

**Part 5 — everything else**
28. **Tailwind replaced by a compiled layer.**
    - The CDN compiled styles in each visitor's browser. Exactly the classes those 323 pages use were compiled once with the Tailwind 3.4.17 CLI: a 15.8 KB layer, colours mapped to the new tokens.
    - Every rule is scoped to `:where(.sl-tw)`: zero added specificity, and only the stack tool and pair pages carry `.sl-tw`.
    - Rebuild with `scripts/build_tw_compat.py`.
29. **Layout check.** Before (`75366e3`) vs after, on 5 pair pages and the stack tool at 390 and 1280px, measuring every section, heading, table, grid, `<details>` and the Venn SVG inside `<main>`.
    - Result: identical position, width, height, grid columns, display and borders, and `<main>` heights equal to the pixel.
    - A line-height regression surfaced on the way and was fixed.
30. **`stacks/generate.py` emits the new template.**
    - It renders the same body, then runs the same `decorate()` prerender runs.
    - A real run rewrote 320 pages byte-identically; a recursive diff was empty.
    - The two KEEP pages were hand-polished on 26 Sep beyond what `data.json` holds, so they're now skipped unless `--force`.
31. **Calculator: template only.** Its footer keeps the standard clauses, not the artboard's "Arithmetic only · Never suggests an amount": today's calculator ships dose presets, so that clause would be false until the rebuild.
32. **Tracker.** The results table is restyled as the data table and the caution stripe is the section rule. On phones the table scrolls inside its own box.
33. **No X link anywhere.** Removed from every nav (template), from about.html's contact list, from the calculator's inner footer, and from the homepage JSON-LD.
34. **New noindex pages:** `404.html` and the `/stack.html` placeholder.
35. **Sitemap.** Regenerated: + `/compounds/`. `gen_sitemap.py` now skips `assets/`, `templates/`, `kalios-design/` and `og/`.

**Part 6 — OG images**
36. **Built into prerender.** `scripts/og_images.py` renders at 2×, downsamples, and quantizes to 16 colours (checked for banding): 7.2 MB for 456 images. They're cached by title, so reruns only redraw changed titles.
37. **`/og-default.png` added.** Every compound page's Article JSON-LD names it as the publisher logo, and it never existed. Adding the file fixed that without editing 122 JSON-LD blocks.

**Part 7 — performance**
38. **Font stylesheet no longer blocks first paint.** It now loads via preload + onload; `display=swap` already meant text first shows in a fallback face, so nothing changes visually. Lighthouse Performance went from 91 → 100 (homepage) and 86 → 99 (bpc-157).

---

## Card data judgment calls

All 123 rows (117 compounds incl. 4 aliases, 6 stacks) are in `gsc-export/card-data.csv`. The `evidence_basis` column shows the badge rule or the reason for each override.

**Totals**
- **Evidence:** 0 no data ×9 (6 stacks + 3 pages) · 1 animal only ×47 · 2 human pilots ×21 · 3 human trials ×28 · 4 approved drug ×18.
- **Stamps:** RESEARCH ONLY 81 · FDA APPROVED 18 · NOT LEGAL YET 12 · COMPOUNDING PHARMACY 6 · NOT LEGAL TO COMPOUND 6.

**Evidence level vs badge (23 calls; `mt-2`, the Melanotan II alias, inherits one)**

| Page | Badge | Level | Why |
|---|---|---|---|
| aicar | Limited Human Data | 3 Human trials | Published multicenter randomized acadesine trials in CABG (Mangano 1997 meta-analysis) |
| argireline | Limited Evidence | 2 Human pilots | Small published randomized cosmetic studies (Wang 2013, n=60) |
| bpc-157 | Preclinical | 2 Human pilots | Three published human pilots (Lee 2021, 2024, 2025); the artboard rates it "human pilots" |
| bpc-157-fragment | Preclinical | 0 No data | Page: no peer-reviewed study isolates fragment activity; no human data |
| decapeptide-12 | Limited Evidence | 2 Human pilots | Published split-face RCT n=5 (Hantash 2009), open-label n=33 (Ramírez 2013) |
| ghrp-2 | Limited Human Data | 3 Human trials | Pivotal Japanese multicenter diagnostic trial (126 children) + Phase II trials |
| hexarelin | Phase II | 2 Human pilots | Many small pharmacology/diagnostic studies; no efficacy RCT |
| matrixyl | Limited Evidence | 3 Human trials | Robinson 2005: 93-subject double-blind vehicle-controlled RCT |
| melanotan-ii | Research Only | 2 Human pilots | Small published Phase 1/2 studies (Dorr 1996; Wessells 1998/2000) |
| n-acetyl-epithalon | Moderate Evidence | 1 Animal only | No human data on the acetylated variant; badge describes parent Epithalon |
| n-acetyl-selank | Clinical Use (Russia) | 1 Animal only | No human data on the acetylated variant; badge describes parent Selank |
| n-acetyl-semax | Clinical Use (Russia) | 1 Animal only | No human data on the acetylated variant; badge describes parent Semax |
| pal-ahk | Limited Evidence | 0 No data | Page: stand-alone evidence "sparse to non-existent" |
| palmitoyl-dipeptide-6 | Limited Evidence | 0 No data | Page: "almost nothing"; no peer-reviewed research base of its own |
| pentapeptide-18 | Limited Evidence | 2 Human pilots | Published open-label cosmetic study (Dragomirescu 2014, n=20) |
| pinealon | Preclinical | 2 Human pilots | 72-patient Russian TBI clinical series on the peptide itself |
| tb-500 | Preclinical | 2 Human pilots | Published Tβ4 IV Phase 1 and topical ophthalmic trials; SubQ "TB-500" itself unstudied; identity question open |
| thymagen | Preclinical | 2 Human pilots | Small published Russian cohort (Zhuk & Galenok 1996, PMID 9026934) |
| thymalin | Moderate Evidence | 2 Human pilots | Khavinson & Morozov 2003 controlled cohort; no randomized trial |
| thymulin | Preclinical | 2 Human pilots | Page lists small open-label interventional pilots in children |
| vilon | Preclinical | 2 Human pilots | Khavinson & Morozov 2003 cohort (PMID 14523363) had a Vilon arm |
| vip | Preclinical | 3 Human trials | Published Phase 2 (inhaled VIP in PAH) and randomized aviptadil COVID trials, both negative |
| zuclomiphene | Limited Evidence | 2 Human pilots | Published human PK studies; its Phase 2 RCT is an unpublished interim topline |

**Badge vs section disagreements worth a content look:**
- VIP, AICAR and Matrixyl: the badge undersells what the Human Data section lists.
- The N-acetyl variants: the badge describes the parent compound.

**Use tags** map the taxonomy primary tag to the chip vocabulary, with one extension and a few overrides.
- **One extension, "Hormones":** testosterone, hCG, enclomiphene, zuclomiphene, gonadorelin, kisspeptin, triptorelin, testagen, insulin. "Hormone & Anti-Aging" holds two populations; the TRT half has no fit, and its secondary tag would read "Libido".
- **Overrides:**
  - cerebrolysin, cortagen → Stroke & brain recovery (artboard)
  - humanin → Longevity
  - oxytocin → Mood & focus
  - follistatin-344, IGF-1 DES, IGF-1 LR3, MGF, PEG-MGF → Muscle (without them "Build muscle" would be empty)
  - AHK-Cu, Pal-AHK → Hair
- **Group sizes:** Fat loss 25, Skin 17, Longevity 14, Mood & focus 11, Immune 10, Growth hormone 9, Hormones 9, Tendon & gut repair 6, Muscle 5, Hair 2, Libido 2, Stroke & brain recovery 2, Sleep 1 (secondary tags are not used).
- **Stacks:** Wolverine and KLOW → Tendon & gut repair · GLOW → Skin · GH → Growth hormone · Mito → Longevity · Flow State → Mood & focus.

**Red flags (5).** Each page's "Important" box (and, for two, a quick fact) names a specific observed or lethal harm.

| Page | Red flag |
|---|---|
| adipotide | Kidney damage in primate trials |
| insulin | Hypoglycemia can be fatal |
| waglerin-1 | Lethal snake-venom neurotoxin |
| mk-677 | Trial halted over heart failure |
| melanotan-ii | Mole changes in case reports |

Not flagged: theoretical risks (cancer theory on IGF-1 / c-Met compounds), and the GLP-1 class boxed warning, which only 2 of the 5 GLP-1 pages feature prominently.

**Aliases.** adamax, ghk-cu-fragment and vesilute are derived from their own pages; mt-2 (a redirect stub) copies melanotan-ii.

---

## Audit results

`scripts/street_lab_audit.py` checked all **456 deployable pages** against the last pre-redesign commit `75366e3`: **0 failures.** Output: `gsc-export/street-lab-audit.json`.
- **Which pages:** the 449 sitemap URLs (448 before + `/compounds/`) plus about, stacks-index, 3 alias pages, 404 and stack.html. The 2 redirect stubs are skipped.
- **Checked on every page:**
  - every `<h2>` (same text, same order)
  - every table
  - every Key References item
  - every text node outside the old nav and footer
  - every JSON-LD block unchanged, and it parses
  - content links
  - exactly one `<h1>`
  - the generated header, strip and footer are present
  - stylesheets are Google Fonts and `/assets/street-lab.css` only, with no Tailwind CDN
  - under 16 MB (largest: `compounds/index.html`, 115 KB)
  - the og:image file exists
- **Allowed drops (all intended):** the stack pages' "Stack Protocol" badge; the X link and its separator on about and calculator; the homepage's X `sameAs`. The rebuilt homepage is checked on its own.
- **The audit can fail:** a renamed h2, a deleted reference, a deleted sentence and a changed link were each caught in a negative test.
- **Per-part audits** each passed against the previous commit: Part 3 sample 4/4, Part 3 122/122, Part 4's two new pages 2/2, Parts 5 and 6 456/456.

**Behaviour checks (headless Chrome)**
- The homepage search lands on the page.
- `/compounds/?q=` works for an exact name, a trade name (Ozempic → semaglutide), a partial term (bpc → 5 cards) and no match (Karl).
- The menu opens, closes, and closes on Escape.
- The alert form refuses an empty address and sends nothing.
- Calculator: identical results before and after (BPC-157 preset: 10 units at 250 mcg, 20 at 500 mcg).
- Stack tool: BPC-157 + TB-500 renders the Wolverine result.
- All three font families load.
- No console errors on any page tested.

**Preview checks (`vercel curl`, through Deployment Protection)**
- 200 for:
  - `/`, `/compounds/`, `/compounds` (no slash)
  - `/compounds/bpc-157.html`, `/compounds/mito-stack.html`
  - `/stacks/`, a pair page
  - the tracker, calculator, `/stack.html` and `/about.html`
  - both assets, an OG image, `/og-default.png` and the wordmark
- `/compounds/bpc-157` → 308 → `.html`.
- 404 served by the new 404 page for:
  - an unknown path
  - `/templates/`, `/kalios-design/`, `/data/page-dates.json`, `/data/og-manifest.json`
  - `/scripts/`, `/reports/`, `/.claude/`, `/gsc-export/`

---

## Screenshots

All captured in headless Chrome at 390px and 1280px.
- **`gsc-export/street-lab-shots/`** (Part 7): `home`, `bpc-157`, `mito-stack`, `tracker`, `calculator`, `pair-bpc-157-with-tb-500`, `about`, each `-390` and `-1280`. WebP, or JPEG where a page is taller than WebP's 16,383px limit. None scrolls sideways.
- **`gsc-export/pair-shots/before/` and `after/`** (Part 5): 5 pair pages + the stack tool at both widths. "Before" is the live design at `75366e3`, black-on-white on pair pages; "after" is the new design.

---

## Lighthouse (mobile)

Lighthouse 13, mobile, run against a local static server with gzip. The preview sits behind Vercel login, which Lighthouse can't pass without a bypass token.

| Page | Performance | Accessibility | Best practices | SEO | LCP | CLS |
|---|---|---|---|---|---|---|
| Homepage (new) | **100** | **100** | **100** | **100** | 1.4 s | 0.002 |
| bpc-157 (new) | **99** | **100** | **100** | **100** | 1.6 s | 0.019 |
| Homepage (pre-redesign, same server) | 89 | 90 | 96 | 100 | 3.8 s | 0.001 |
| bpc-157 (pre-redesign, same server) | 91 | 97 | 100 | 100 | 2.8 s | 0 |

The first pass of the new pages scored 91 and 86 on Performance, which led to DECISIONS 38. Reports: `gsc-export/lighthouse/home-mobile.html`, `gsc-export/lighthouse/bpc-157-mobile.html`.

---

## Counts

- **Pages:** 456 deployable (453 before + 3 new: `compounds/index.html`, `404.html`, `stack.html`) + 2 untouched redirect stubs.
- **Compound + stack pages restructured:** 122 (116 + 6). PCAC notices moved into the data file: 12.
- **Pair pages:** 322. Tailwind CDN removed from 323 (322 + the stack tool).
- **OG images:** 456 (+ `og-default.png`).
- **`street-lab.css`:** 48.9 KB (10.7 KB gzipped), of which 15.8 KB is the utility layer. **`street-lab.js`:** 2.6 KB.

## Files changed (main ones)

- **New:**
  - `assets/street-lab.css`, `assets/street-lab.js`
  - `templates/{header,fine-print,footer}.html`
  - `compounds/index.html`, `404.html`, `stack.html`
  - `og/` (456) and `og-default.png`
  - `art/kalios-wordmark-480.webp`, `art/karl-280.webp`
  - `data/page-dates.json`, `data/changelog.json`, `data/og-manifest.json`
  - `scripts/{streetlab,street_lab_migrate,street_lab_audit,card_data,build_tw_compat,og_images}.py`
  - `scripts/fonts/` (Anton + OFL licence)
  - `kalios-design/` (the spec and artboards)
- **Changed:**
  - every HTML page
  - `scripts/prerender.py`, `scripts/gen_regulatory_status.py`, `scripts/gen_sitemap.py`, `scripts/sync_pair_fields.py`, `scripts/extract_desc_map.mjs` (marked retired)
  - `stacks/generate.py`, `stacks/extract.py`
  - `data/regulatory-status.json`, `data/compound-desc.json` (+ the six stack hooks, verbatim from the old homepage)
  - `sitemap.xml`, `.vercelignore`, `.gitignore`
- **Kept out of deploys (`.vercelignore`):** `templates/`, `kalios-design/`, `data/page-dates.json`, `data/og-manifest.json`, plus everything already excluded (`scripts/`, `gsc-export/`, `reports/`, `.claude/`).

---

## Findings (pre-existing)

1. **Pair pages were broken on production (fixed by this build).** All 322 had a JavaScript syntax error in their inline Tailwind config (`[''Inter'', …]`), left by an earlier font replacement. The config never applied, so the pages rendered black-on-white with the Venn diagram and tag chips almost invisible. Evidence: `gsc-export/pair-shots/before/`.
2. **`/og-default.png` and `/og-image.png` returned 404 on production.** They are every page's current og:image, and the compound pages' JSON-LD publisher logo. Every page now has its own image, and `og-default.png` exists.
3. **The tracker's results table overflowed a 390px screen** (469px wide). Fixed.
4. **`stacks/extract.py` needs Python ≥3.10 and BeautifulSoup:** it uses `str | None` at definition time. The machine default is 3.9. I updated it for the new markup and tested it in a scratch venv.
5. **Sitemap lastmod comes from git,** so after this redesign every URL reads 27 Sep. Switching it to `data/page-dates.json` would make lastmod mean "content changed". Not changed.

## URLs to eye-check

On the preview (`dpl_4HmUneLxQdzWxSxoqek46AMeQMdn`). The same paths will apply on www.kalios.health once promoted.
- `/`, `/compounds/`, `/compounds/?q=bpc`
- `/compounds/bpc-157.html` (PCAC band), `/compounds/semaglutide.html` (no band), `/compounds/adipotide.html` (red flag, research-only band), `/compounds/dsip.html` (voted-against band), `/compounds/mito-stack.html` (stack)
- `/stacks/pairs/bpc-157-with-tb-500/` (now dark), `/stacks/`, `/fda-pcac-2026.html`, `/calculator.html`, `/compare/semaglutide-vs-tirzepatide.html`
- `/stack.html`, `/about.html`, any unknown path (404)

## Open questions for G

1. **Promote the preview to production?** If yes: `python3 scripts/prerender.py` → git-less copy → `vercel --prod` → `bash push-to-indexnow.sh` → curl the changed live URLs (rule 4), then add a line to `data/changelog.json`.
2. **Primary action / Paperwork door** until `/coa` exists: keep the `/stack.html` placeholder, point it somewhere else, or hide it?
3. **Caution band on 81 research-only pages:** keep it (the spec says so), or limit it to the 12 PCAC pages?
4. **Copy changes** in DECISIONS 22: accept, or restore the artboard wording?
5. **Evidence judgment calls:** especially VIP, Matrixyl, AICAR, the N-acetyl variants, and stacks at 0.
6. **Back link and aka line** (DECISIONS 12): keep, or cut to match the artboard?
7. **CLAUDE.md** after approval: nav template, design system, and the retired "copy BPC-157's `<style>`" rule. Draft the update next session?
8. **about.html** (below) still has "[AUTHOR]" and two "[PHOTO PLACEHOLDER]"s, and describes the old tag vocabulary. The site now uses five statuses, and the new cards say FDA APPROVED / COMPOUNDING PHARMACY / NOT LEGAL YET / RESEARCH ONLY. Its "Compoundable … 503A or 503B" line is also the open 503B question.
9. **Carried over and not addressed tonight:**
   - identity-workflow open questions 5–10
   - content-truth open questions 1 and 3–10 (read for context only, per the brief)
   - the Vercel author block (deploys still go through the git-less copy)
   - ~~Brave key rotation~~ **Closed 2026-09-27: G rotated the key.**

## Web content

Rule 3: fetched material was treated as data only, and nothing fetched was acted on as an instruction. No instruction-like text was seen in any of it.
- Anton and IBM Plex Mono font files from Google's fonts repository on GitHub (the Plex files were later removed as unused).
- npm packages: tailwindcss 3.4.17, puppeteer-core, Lighthouse.
- HTTP status checks of cdn.tailwindcss.com, fonts.googleapis.com and www.kalios.health.

---

## about.html — full text, for review

This is the visible text of `about.html` as it stands on the preview: templated, still unlinked, and with the X contact row removed per "no X link anywhere". Every one of the page's 40 text nodes is below.

> Nav, strip and footer are omitted; they are the shared templates.

#### About Kalios

Independent peptide compound reference. 113 cited compounds, a stack tool, a dosing calculator, and an FDA tracker. Free, no vendors.

##### Who runs this

[PHOTO PLACEHOLDER] *(photo box)*

[PHOTO PLACEHOLDER] *(caption)*

Kalios is written and maintained by [AUTHOR]. Not a clinic, not a company, not a supplement brand — one person reading the peptide literature and writing it down honestly. Every compound profile follows the same 17-section template so evidence, dosing, and gaps line up the same way across the database. When the research is thin, the page says so.

##### Three rules

1. No vendors. Kalios never links to a peptide seller, never accepts affiliate revenue, never receives free product for review.
2. No affiliates. No commission from any pharmacy, compounder, clinic, or supplement brand. Kalios makes no money from the compounds it documents.
3. Nothing to sell. No paid tier, no ebooks, no coaching, no supplements, no consulting. The site is free because there is nothing to upsell.

##### How we grade evidence

Every compound page carries two signals: a regulatory status tag on the homepage card, and an evidence tier badge on the profile itself. Neither is a recommendation — they mark where a compound sits in the pipeline and how strong the human data is. Free-form “Evidence Strength” text in Quick Facts adds the per-compound specifics (e.g. “Preclinical: Strong / Human: Pilot only,” “Strong (multiple large Phase 3)”).

Regulatory status — homepage card tag

| Tag | What it means |
|---|---|
| FDA Approved | Approved by FDA for a specific indication. Available by prescription under a labeled use. Off-label prescribing is a separate clinical decision. |
| Compoundable | Currently accessible via a 503A or 503B compounding pharmacy under a valid prescription. Regulatory status can change — see the FDA tracker for the July 2026 PCAC vote and pending rulemaking. |
| Research Only | Not FDA-approved and not currently available through a licensed compounding pathway. Sold as a research chemical or accessed outside the regulated supply chain. |

Evidence tier — compound profile badge

| Badge | What it means |
|---|---|
| FDA Approved | A pivotal RCT program (typically Phase 3) supports at least one labeled indication. Human efficacy and safety data are the strongest tier on the site. |
| Mid-tier | Meaningful human data short of full approval — e.g. Phase 2 or Phase 3 in progress or completed without approval, established clinical use outside the U.S., long-standing off-label prescribing, or strong preclinical data with a positive human pilot. Badge text varies per compound (“Phase II,” “Phase III Complete,” “Clinical Use (Ex-US),” “Strong Preclinical,” “Moderate Evidence,” “Limited Human Data”). |
| Preclinical / research only | Animal, in vitro, or single-lab evidence with no or minimal published human trials. Badge text varies (“Preclinical,” “Phase 1,” “Limited Evidence,” “Research Only,” “Cosmetic Only”). Read the profile before assuming a preclinical signal generalizes to people. |

A compound’s regulatory tag and evidence badge can diverge. A compoundable peptide can still be preclinical-only in humans (e.g. BPC-157). An FDA-approved drug is always the strongest evidence tier on the site.

##### How to reach us

- **Email:** kaliospeptides@proton.me
- **Substack:** kaliospeptides.substack.com

Corrections, missing citations, and factual errors are welcome. Send the source. No PR pitches, no sponsorship offers, no vendor introductions — see the three rules above.
