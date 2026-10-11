# Prune + receipt — 163 pair pages retired, one stacks tool

**Session:** 2026-10-11. **Status:** all four parts done, deployed and checked on production.
**Deploy:** `dpl_7DsxkhaYB51umdmLPay8sQriu2Lg` (production, Ready). **IndexNow:** 368 URLs (the 200 sitemap URLs and the 168 URLs retired today), HTTP 200.
**Commits:** `28aedc4e` (Part 1, the prune), `b42aa608` (Part 2, the receipt), `0e496b03` (Part 3, measurement), `51c84457` (Part 4, IndexNow for retired URLs), then this report.

**In one line:** pair pages went from 181 to 21, the sitemap from 203 to 200 URLs, and three tools now remain: the calculator, COA Check and the receipt at `/stack.html`. The receipt shows, for any two compounds or a named stack, what has been tested together in people (38 pairs have published human data) or "none published".

---

## Look at these first

1. **The receipt is live at the Stacks hub's old URL.** https://www.kalios.health/stack.html. You pick two compounds or a named stack. It shows, in G's order:
   - **Tested together in people:** the count and its citations, or "none published".
   - **What each does alone:** its card.
   - **Mechanism overlap:** labelled "reasoning, not evidence".
   - **What the studies found:** only where a study gave the two to people.

   The kept pair pages and the seven named stack pages carry the same receipt, drawn by the same code.
2. **"None published" has a stated scope.** Every "none published" receipt says what it means: no study that gave them to people together is among the sources Kalios cites. Reports of people already taking both, registered trials, conference talks, patents and makers' own tests don't count. The receipt can't know every paper in the world; it says so.
3. **38 pairs have published human data, not 21.** The 21 kept pairs have pages. Another 17 pairs are ones the compound pages themselves cite as given to people together, and they show on the receipt with their studies.
   - Some are strong. Insulin + pramlintide is pramlintide's approved use. hCG + hMG is the Menopur label's regimen. Bimagrumab + semaglutide had a 507-person randomized trial.
   - Without them, the receipt would have said "none published" right under cards that describe those trials.
   - Whether any of them should get a page is open question 1.
4. **One kept pair page was wrong, and is corrected.** GHRP-2 with Sermorelin said the two had been studied together only inside a three-peptide regimen with GHRP-6. A 1995 study in its own references gave GHRH(1-29)NH2 (sermorelin's molecule) and GHRP-2 together as single IV doses to eight healthy men (Tiulpakov et al., 1995). The page has a corrections entry.
5. **The same question is open for GHRP-6 with Sermorelin.** Two diagnostic studies gave "GHRH" with GHRP-6 (Cordido et al., 2003; Popovic et al., 2003). Their abstracts don't say which GHRH, and the full text was blocked to every route I tried. If it was GHRH(1-29), that page's "no study has given GHRP-6 and sermorelin together without GHRP-2" is wrong too. Open question 2.
6. **Retired pair URLs land on the receipt with both compounds picked.** For example, https://www.kalios.health/stacks/pairs/bpc-157-with-tb-500/ returns 301 → `/stack.html?a=bpc-157&b=tb-500`.

---

## What shipped

### Part 1 — the prune (`28aedc4e`)

**(a) Pair pages**
- A pair keeps a page only where a published human study gave the two to people together.
- Of the 24 kept pairs, 18 met that. Six did not, and were retired:
  - five whose only study of the pair is in rats or mice: BPC-157 + TB-500, cagrilintide + retatrutide, cagrilintide + tirzepatide, hCG + insulin, methylene blue + NAD+;
  - one kept in 12b for its visits only: eloralintide + retatrutide.
- G's three were added: Crystagen + Pinealon (Viner et al., 2012), BPC-157 + thymosin β4 (Lee & Padgett, 2021) and EGF + GHRP-6 (the Cuban stroke trials, Hernández-Bernal et al., 2024 and 2026). That makes 21 pages.
- The 157 remaining Tier 1 pages were retired with them (163 pages in all). Each retired page:
  - is deleted from the repo;
  - is in no sitemap (the 157 Tier 1 pages had been noindex and out of it since 12b);
  - has had every internal link to it removed (142 card links, 6 links in text, a stale one on Semax, 26 list items on the legacy stacks index);
  - 301s to `/stack.html?a=<x>&b=<y>`, with three rules per page (bare, slash, subpaths). Nine older chained redirects were re-pointed to land in one hop;
  - is recorded with its date in `data/retired-pages.json` and was submitted through IndexNow.

**(b) GHK Basic merged into GHK-Cu Fragment** (one copper-free tripeptide under two names).
- 301 from `/compounds/ghk-basic.html`.
- Nothing carried over: GHK Basic's unique lines were unsourced, or had already been corrected on the Fragment page.
- One corrections entry.

**(c) The BPC-157 / TB-500 / GHK-Cu compare page** 301s to the GLOW Stack's receipt (`/stack.html?stack=glow-stack`). Its three crosslink cards are gone. Semaglutide vs tirzepatide stays.

**(d) The ten `compounds.pre-*` backup folders** are gone from the working tree.
- Nothing deployed pointed at them, and all ten returned 404 on production.
- They were untracked, and all 1,133 of their files exist as blobs in git history.

**(e) Orphan scan:** `scripts/orphans.py`, listed below.

**(f) Three tools.**
- The homepage description and FAQ, the tracker's call to action, `llms.txt` and the 404 page name or link the calculator, COA Check and the stack receipt.
- In Part 2, the "Stack Compatibility Tool" / "Stack Research Tool" item on 114 compound pages became the receipt line, and Glumitide's paragraph about the "Stack Research Tool" went.
- No other page calls itself a tool. A sweep of every page's visible text finds only the disclaimer's "analytics tool", the receipt's own disclaimer and compounds described as research tools.

### Part 2 — the receipt (`b42aa608`)

**The receipt and its pages**
- **`/stack.html`** is the receipt: two compound fields, the seven named stacks as chips, and the receipt. Links carry the pick (`?a=&b=`, `?stack=`), so a retired pair URL opens its receipt.
- **The scope line** (Look first, 2) is on every "none published" receipt and in the page's FAQ.
- **`/stacks/`** is a real index page again; since 12a it had redirected to `/stack.html`. It has a button to the receipt, the seven named stacks as cards, and the 38 tested pairs, each with its evidence level: the 21 with pages first, then the 17 on the receipt. The homepage and guide Stacks chips and the `/compounds/` Stacks header link it.
- **The 21 pair pages** are rebuilt in the receipt's format: hero, receipt, Legal Status, References.
- **The 7 named stack pages** lead with their receipt, right after the caution band.

**Compound pages**
- The "Stack Compatibility Tool · Pair X with any of N compounds" item became one line, "Stacking this? Read the receipt →", with the compound picked (149 pages). FLGR-242 has none: the receipt doesn't offer it (decision D16).
- 212 "Commonly Stacked With" cards carry a receipt line:
  - 35 with a trial count and its citations;
  - 177 "none published", 6 of them with a note.
- 25 "some users / some clinicians / some practitioners" sentences were dropped from the cards, retatrutide's three included.
- Pt-141's Kisspeptin-10 card linked a page that doesn't exist; it now links Kisspeptin.

**Data**
- `stacks/data.json` gains `tested_pairs`: 38 pairs, each with its count line, studies, evidence level and the basis for it.
- It also gains `pair_notes`: 9 pairs with data on people together but no study that gave them together:
  - 4 outside journals: a maker's tests (Argireline + Leuphasyl; Pal-GHK + Rigin), a patent (insulin + Pancragen), a conference talk (eloralintide + tirzepatide);
  - 5 of people already taking both (desmopressin with growth hormone, semaglutide, liraglutide or tirzepatide; cardarine + MK-677). For these the receipt says "none published" and names what exists. The desmopressin page itself calls these "Not a combination anyone tested".
- The cards' text is mirrored into `stacks/data.json`: 23 summaries updated, one stale "Some practitioners" summary synced.

### Part 3 — measurement (`0e496b03`)

All four Umami events carry only compound names. Nothing personal is sent.
- **`calc-run`** fires when an answer has stayed on screen for 2 seconds, once per distinct calculation, at most 5 a visit.
  - Its one property is `compound`: the compound page the visit came from (this site's `/compounds/<slug>.html`, from the referrer), or `none`.
  - It never carries an amount, a unit, the label or the link. The tracker's `data-exclude-hash` strips the URL hash from events as well as page views; I checked this in the tracker's code.
  - It doesn't run in the embed.
- **`receipt-run`** fires each time a receipt is shown, with `a` and `b`, or `stack`.
- **`receipt-click`** is on every "Read the receipt" link on the compound pages, with `compound` and `from` (`line` or `card`).
- **`coa-check`** is unchanged. I saw it fire on a completed check against the local mock.
- All four were exercised in headless Chrome with the tracker stubbed.

### Part 4 — checks and deploy (`51c84457`)

`push-to-indexnow.sh --retired [since]` adds the retired URLs from `data/retired-pages.json` to the submission.

---

## DECISIONS

All taken on the conservative side, no questions asked.

**Pair pages and evidence levels**
- **D1. Which pair pages stay.** G's test: a published human study gave the two to people together. 18 of the 24 kept pairs pass. The six above don't, and were retired. Liraglutide + pramlintide stays, but it is the weakest: a mouse study, plus one case report in which pramlintide was given briefly.
- **D2. The receipt's words.** "Tested together in people", or "none published", the same words everywhere. Not "trials": the human data include record reviews and case reports, and each count names its designs.
- **D3. The pair's evidence level** uses the compound meter's five labels.
  - **Approved drug:** the two are one approved product (enclomiphene + zuclomiphene is clomiphene), or an FDA label's directions use them together. Pramlintide is approved only as an add-on to mealtime insulin; Menopur's label has hCG given after it.
  - **Controlled trials:** a randomized or placebo-controlled trial gave both.
  - **Human pilots:** smaller, uncontrolled or retrospective data, case reports included.
  - A named stack shows its card's meter. A pair with no human data shows no meter.

**Card text and receipt lines**
- **D4. Which card sentences went.** Only those built on an unnamed "some" (users, clinicians, practitioners, clinics, protocols): 25 cards on 15 pages. A seller listing's "many users" and named attributions ("Clinic protocols describe…") stay.
- **D5. Which cards get a receipt line.** Only cards whose partner the receipt can look up. Cards for metformin and other compounds with no page get none.
- **D6. Dates.** Structural, link and card changes kept each page's "Checked" date (reseeded). Today's date is on GHRP-2 + sermorelin (corrected), the three new pair pages, `/stacks/` and `/stack.html`.

**GHK and DSIP**
- **D7. GHK Basic.** Nothing carried to the Fragment page (Part 1 b).
- **D8. DSIP's card.** Regenerating the card data for the GHK merge moved DSIP's card meter from Controlled trials to Human pilots, as its page has rated it since Truth 3c. The Mind & sleep goal page now counts 3 of 15 with controlled trials. Corrections entry.
- **D9. Pt-141's Kisspeptin-10 card** linked a 404; it now links Kisspeptin.

**How the receipt is built and counts**
- **D10. One renderer.** The stack pages, the pair pages and `/stack.html` draw identical receipts. 13 receipts compared in headless Chrome match Python's output, apart from a filter attribute the browser's cards don't need.
- **D11. "Tested together".** It means a study gave both to people. People who were already taking both for other reasons go under "none published" with a note, matching the desmopressin page's own words.
- **D12. 17 page-less tested pairs** (`stacks/tested-pairs.json`). The brief's page list is the kept pairs plus three, so these are receipt data, not pages.
  - Every reference is copied from the page that cites it.
  - Every number was checked against that page or the paper's abstract.
  - Two come from sources the pages cite but don't quote. Insulin + liraglutide is the Victoza label's randomized insulin detemir add-on trial, read on DailyMed today; the liraglutide page cites the label. hMG + triptorelin is the long-protocol arm of Mansour et al., 2003, cited on the clomiphene page; its abstract names Decapeptyl.
- **D13. The 9 notes** (Part 2).
- **D14. The scope line** on every "none published" receipt.

**Corrections and exclusions**
- **D15. The two GHRH pair pages.** GHRP-2 + sermorelin is corrected. GHRP-6 + sermorelin is left as it was (open question 2).
- **D16. FLGR-242 stays out of the receipt.** It was left out of the stack tool when its page launched (2026-09-28). Its card lines show "none published" without a link, and its page has no receipt line.

**Hubs and navigation**
- **D17. `/stacks/` and the nav.** `/stacks/` is the index. The nav keeps G's "Stacks" label, now opening the receipt, so the nav lists the three tools: Stacks (the receipt), Calculator, COA Check.

**Data files and the mechanism section**
- **D18. Card text in the data.** `stacks/data.json` mirrors the cards' text (Part 2). Pt-141's Kisspeptin entry was re-keyed to the real page.
- **D19. "Shared mechanism tag" lists mechanisms only.** It leaves out classes, uses and origins (cosmetic peptide, tissue repair, peptidomimetic, Khavinson bioregulator) and "serm", which repeats "selective estrogen receptor modulator".
- **D20. `stacks/pairs.json` stays.** It is a public file; nothing reads it now.
- **D21. Two new input files are not deployed.** `data/retired-pages.json` and `stacks/tested-pairs.json` are inputs, like `data/routes.json`, and are in `.vercelignore`.

**Changelog and measurement**
- **D22. Changelog.**
  - A site entry for the receipt and the prune.
  - A content entry for the three new pair pages; it is the homepage's "what changed" line.
  - Corrections for GHK Basic, Mind & sleep (DSIP) and GHRP-2 + sermorelin.
- **D23. Measurement** as in Part 3: compound names only, never amounts.

---

## Counts, before → after

| | Before (`8aff5af6`) | After |
|---|---|---|
| HTML pages in the repo (deployable) | 367 | 206 |
| Pair pages (`/stacks/pairs/`) | 181 | 21 |
| Files in `compounds/` | 160 | 159 (GHK Basic merged) |
| Compare pages | 2 | 1 |
| Sitemap URLs | 203 (24 pair pages) | 200 (21 pair pages, plus `/stacks/`) |
| Redirect rules (`vercel.json`) | 466 | 958 |
| Tools | calculator, COA Check, the Stacks hub's "look up any two" view; a "Stack Compatibility / Research Tool" item on 114 compound pages | 3: calculator, COA Check, the receipt |
| Pairs shown with published human data | 24 kept pairs (6 of them with no human study) | 38 (21 with pages, 17 on the receipt), plus 9 notes |
| `stacks/data.json` compound entries | 151 | 150 |

**Sitemap changes**
- Gone: the compare page, GHK Basic and the six retired kept pairs.
- Added: `/stacks/`, the three new pair pages and GHK-Cu Fragment, now the canonical page.

## Retired URLs

**168 URLs were retired on 2026-10-11.**
- 163 pair pages in this session: 157 Tier 1 pages and 6 kept pairs.
- 3 ARA-290 pair pages retired earlier today by Truth 3c.
- GHK Basic.
- The compare page.

`data/retired-pages.json` now lists 310 retired URLs in all: 308 pair pages since 2026-09-28, dated from git for the earlier ones, plus the merge and the compare page. All 168 of today's were submitted to IndexNow.

## Orphan list (no inbound link from any other page)

| Page | Visits (gsc-export) | Note |
|---|---|---|
| `/404.html` | no data | The error page; nothing should link it. |
| `/stacks-index.html` | no data | The legacy stacks index, noindex; `/stacks/` now does its job. Open question 4. |

`gsc-export/` holds no Search Console or page-visit export, so no visits could be listed. After Part 1 the list also had four kept pair pages (Argireline + Syn-Ake, Enclomiphene + Zuclomiphene, GHRP-2 + GHRP-6, hCG + HGH); `/stacks/` links them now. Nothing was deleted from the list.

---

## Gates and checks

| Check | Result |
|---|---|
| `bash tests/run-calc-tests.sh` | PASS: 400/400, the 28-case self-test, math frozen at v1.0 |
| COA tests | `coa-scorer` 26 passed, `coa-function` 14 passed. No COA file changed. |
| `python3 scripts/prerender.py`, twice | second run: 0 changed everywhere |
| `python3 scripts/gen_sitemap.py` | 200 URLs |
| `python3 scripts/audit_pages.py --mode cite`, online | 5,803 citations on 180 pages, 0 failing, 111 allowed with a reason: PASSED |
| `python3 scripts/street_lab_audit.py --all --rev 8aff5af6` | 138 of 203 pages differ, every difference intended (below) |
| `git ls-files --deleted` | empty |
| Identity greps (author/committer; denylist) | both print nothing |
| `gitleaks git --log-opts="origin/master..HEAD" --redact` | no leaks (4 commits) |
| Git-less copy, `diff -rq` | only `.git`, `.claude`, `reports`, `.DS_Store` |

**What the Street Lab audit flagged (all intended)**
- The tool items, the 25 dropped sentences, the GHK Basic and compare cards and the links to retired pages.
- The pair pages' new format.
- `/stack.html` (rewritten).
- The GHK merge in JSON-LD and the Skin & hair goal page.
- The DSIP count.
- The tracker's call to action.
- The legacy index's list.
- GHK-Cu Fragment's "Next steps" link now points to its own group, Skin & hair, instead of Tendon & gut repair.

**Receipt parity:** the browser's receipt matches Python's on 13 receipts: pairs with and without pages, a noted pair, "none published" pairs, two stacks and an old slug (Epitalon).

**Phone width (390 px):** no horizontal overflow on `/stack.html`, `/stacks/`, a pair page, a stack page and a compound page.

## Deploy

`dpl_7DsxkhaYB51umdmLPay8sQriu2Lg`, from a git-less copy (the known blocker), production, Ready.

Production checks:

| URL | Result |
|---|---|
| https://www.kalios.health/stack.html | 200; receipt data: 148 compounds, 7 stacks, 38 tested pairs, 9 notes |
| https://www.kalios.health/stacks/ | 200; 38 pair rows (`/stacks` 200 too) |
| https://www.kalios.health/compounds/wolverine-stack.html | 200; receipt "Tested together in people: none published" |
| https://www.kalios.health/stacks/pairs/egf-with-ghrp-6/ | 200; "2 randomized trials, Phase I/II and Phase III (2024, 2026)" |
| https://www.kalios.health/stacks/pairs/ghrp-2-with-sermorelin/ | 200; "1 single-dose study, 8 men (1995); 1 record review, 14 men (2017)" |
| https://www.kalios.health/stacks/pairs/bpc-157-with-tb-500/ | 301 → `/stack.html?a=bpc-157&b=tb-500` |
| https://www.kalios.health/compounds/ghk-basic.html | 301 → `/compounds/ghk-cu-fragment.html` |
| https://www.kalios.health/compare/bpc-157-vs-tb-500-vs-ghk-cu.html | 301 → `/stack.html?stack=glow-stack` |
| https://www.kalios.health/compounds/bpc-157.html | 200; "Stacking this? Read the receipt →" with its click event |
| `/data/retired-pages.json`, `/stacks/tested-pairs.json` | 404 (not deployed, as intended) |
| https://www.kalios.health/sitemap.xml | 200; 200 URLs |

The live receipt also rendered correctly in headless Chrome for BPC-157 + TB-500, insulin + pramlintide and the Wolverine Stack.

## URLs to eye-check

- https://www.kalios.health/stack.html?a=insulin&b=pramlintide: a tested pair with no page; meter, studies and references.
- https://www.kalios.health/stack.html?a=argireline&b=pentapeptide-18: "none published" with a note.
- https://www.kalios.health/stacks/
- https://www.kalios.health/compounds/klow-stack.html: a stack's receipt after the caution band.
- https://www.kalios.health/compounds/desmopressin.html#commonly-stacked-with: the "already taking both" notes.
- https://www.kalios.health/compounds/hmg.html#commonly-stacked-with: hCG + hMG on the label.
- https://www.kalios.health/stacks/pairs/crystagen-with-pinealon/ and https://www.kalios.health/stacks/pairs/bpc-157-with-thymosin-beta-4/
- https://www.kalios.health/corrections.html: the three new entries.
- https://www.kalios.health/: the "what changed" line.

---

## Seen, not changed

1. **Old OG images.** `og/stacks/pairs/` still holds 324 OG images for 21 pair pages. The 303 for retired pages are deployed but nothing references them; deleting them needs a redeploy (open question 5).
2. **A backup file.** `compounds/bpc-157.html.pre-day23-backup` is a tracked file, not one of the ten folders. It isn't deployed (`*-backup` in `.vercelignore`).
3. **Stale page dates.** `data/page-dates.json` still has entries for retired pages. They are harmless; prerender ignores pages that don't exist.
4. **Multi-compound cards.** A card that names several partners gets the receipt line for its first linked one. Desmopressin's "Tirzepatide, Semaglutide, Liraglutide" card shows the tirzepatide line, and its text covers the other two.
5. **Hand-edited summaries.** Two kept pairs' data summaries (cagrilintide + semaglutide, pramlintide + semaglutide) were hand-edited in 12b/12e and differ from the cards' text. The sync script reports them and, by design, doesn't overwrite them.
6. **Combinations the receipt can't know about.** It reads only what Kalios cites. Examples:
   - an hCG plus GnRH-agonist "dual trigger" in IVF;
   - kisspeptin with hCG in women's IVF (the hCG page's search was "in men");
   - Xultophy, the approved insulin degludec + liraglutide combination. Insulin + liraglutide does show a randomized trial, from the Victoza label, but Xultophy isn't named.
7. **pdf.js.** COA Check's pin is 6.3.289; npm has 6.4.299. COA wasn't rebuilt this session.
8. **Test page views in the stats.** My first headless checks ran against a local copy before I blocked the real tracker, so about 35 page views from hostname `127.0.0.1` may show in Umami's stats. Every later run blocked the tracker.

## Open questions for G

1. **Should any of the 17 page-less tested pairs get a page?** The strongest:
   - insulin + pramlintide (the approved use);
   - hCG + hMG (Menopur's regimen; the men's sperm-production studies);
   - bimagrumab + semaglutide (BELIEVE, 507 adults, randomized);
   - clomiphene + hCG (a 282-man randomized trial);
   - exenatide, dulaglutide, tirzepatide and liraglutide each with insulin (label or randomized trials).
2. **GHRP-6 with Sermorelin.** Do you have access to Cordido et al., Eur J Endocrinol 2003 (PMID 12887288) or Popovic et al., Clin Endocrinol 2003 (PMID 12864804)? If their GHRH was GHRH(1-29), the pair page needs the same correction GHRP-2's got.
3. **FLGR-242 in the receipt?** It would need an entry in `stacks/data.json`. It was left out of the stack tool when its page launched.
4. **`/stacks-index.html`, the legacy stacks index** (noindex, orphaned). Retire it with a 301 to `/stacks/`?
5. **The 303 OG images of retired pair pages.** Delete them, with a redeploy, next session?
6. **The nav label.** "Stacks" now opens the receipt. Keep it, or rename it (say, "Stack receipt")?
7. **The "Approved drug" rule for pairs** (D3). Confirm that a label whose directions use the two together counts (pramlintide with mealtime insulin; Menopur then hCG).

---

## Session start

- Claude Code 2.1.296.
- Vercel CLI 60.1.3, with 63.1.2 published; update when convenient.
- `brew outdated` lists 41 packages.
- HEAD at start: `8aff5af6`, tree clean.

## Web content (rule 3)

Everything fetched was read as data, never as instructions. Sources:
- PubMed abstracts and two PMC full texts (Nakhleh et al., 2024; Chong et al., 2024);
- the Victoza label on DailyMed;
- Umami's tracker script, to check what events send.

The Cordido 2003 full text was unreachable (Cloudflare 525, then blocked in a browser). A scan of every fetched file for instruction-like text found none.

## Files changed (`8aff5af6..51c84457`)

385 files.

**Pages**
- 163 pair pages, GHK Basic and the compare page deleted.
- 18 pair pages rebuilt and 3 added.
- 158 compound and stack pages.
- `stack.html` rewritten; `stacks/index.html` new.
- The homepage, `/compounds/`, the guide, `/corrections.html`, the tracker, 404, the legacy stacks index, `coa/index.html` (the compound names list), two goal pages, `llms.txt`.

**Data**
- `stacks/kept-pairs.json`, new `stacks/tested-pairs.json`, `stacks/data.json`, `stacks/pairs.json`.
- New `data/retired-pages.json`.
- `data/changelog.json`, `data/page-dates.json`, `data/og-manifest.json`, `data/regulatory-status.json`, `data/compound-desc.json`, `data/routes.json`, `taxonomy.json`, `gsc-export/card-data.csv`.

**Code**
- `scripts/streetlab.py` (the receipt), `stacks/generate.py`, `scripts/prerender.py`, new `scripts/orphans.py`, `assets/calc.js`, `assets/street-lab.css`, `push-to-indexnow.sh`.

**Config and docs**
- `vercel.json`, `.vercelignore`, `sitemap.xml`, `CLAUDE.md`.

**OG images:** 4 new, 1 redrawn.
