# Street Lab fixes + promote — 2026-09-28

All three parts of the STREET LAB — FIXES + PROMOTE brief are done. **The Street Lab redesign is live on www.kalios.health.**

- **Production:** `dpl_93Tyjsk76Tmf4MnTUS2vrYGC3uzK`, state READY, target production, built from commit `8fddc8f` through the git-less copy (CLAUDE.md rule 4). All ten requested URLs return 200 with the new text and no Tailwind; `/stack.html` returns 404.
- **IndexNow:** 450 URLs submitted, HTTP 200.
- **Commits (all pushed):** `7d273d2` 1.1 · `3801192` 1.2 · `c8121c9` 1.3 · `08bbfb6` 1.4 · `069bfe7` 1.5 · `efe3d82` 1.6 · `8fddc8f` Part 2. This report is the 8th.
- **Scale:** 471 files changed (+2,614 / −1,750), 2 of them deleted (`stack.html`, `og/stack.png`).

**Session start (rule 2)**
- Claude Code 2.1.283.
- Vercel CLI 60.1.3, the same as the latest on npm.
- `brew outdated`: 38 formulae (e.g. `git`, `node`, `python@3.12`, `openssl@3`) and 1 cask (`visual-studio-code`). None were updated.
- Third-party code on the site: only the Umami analytics script. No pinned libraries to update. The build-time Tailwind CLI (3.4.17) was not run.

---

## Look at these first

1. **Two definitions on the About page don't match the data.** The copy is live verbatim (Open question 1).
   - "RESEARCH ONLY: not on any FDA list." FDA's own 503A list (updated May 14, 2026) puts MK-677 (as ibutamoren mesylate) and kisspeptin-10 in Category 2, and ARA-290, GHRP-2, GHRP-6, MGF and thymulin in Category 3. All seven are stamped RESEARCH ONLY.
   - "NOT LEGAL YET: the FDA's advisory committee has voted on it or scheduled it." DSIP was voted on (against) and is stamped RESEARCH ONLY.
2. **The research-only line on those seven pages doesn't use your wording.** "Not on any FDA 503A list" would be false there, so it names the list instead (DECISIONS 2). The other 73 research-only pages use your line verbatim.
3. **16 evidence levels changed** (table below).
   - Five FDA-approved drugs are approved for a different use than their tag, so they drop below "Approved drug". Methylene blue and SS-31 fall to "Human pilots".
   - ARA-290 drops to "No data". Its trials are all nerve repair, and its tag says Tendon & gut repair; the tag is the real problem (Open question 3).
4. **"Reconstituting this? Do the math." now sits on 25 oral or topical compounds too** (list in DECISIONS 9).
5. **The band is on 12 pages, not 13.** mt-2 is a redirect stub to melanotan-ii, and melanotan-ii carries the band.
6. **A content inconsistency turned up while re-reading every Human Data section.** Not fixed; see Open question 7.

---

## What was done

**Part 1.1 — caution band, research-only line, alert block** (`7d273d2`)
- The band shows on the 11 PCAC pages plus DSIP, and nowhere else.
- The 80 research-only pages lost the band. Each now has:
  - one mono line under the data table: "Research only · not on any FDA 503A list · Tell me if this changes →"
  - a small "Status alerts" block holding the alert form, at the end of the page. The link jumps to it.
- Mechanism: two new generated blocks, `sl:status` and `sl:alerts` (`street_lab_migrate.py status-blocks`), filled by prerender from the data file.
- The band's own HTML is byte-identical to before on all 12 pages.

**Part 1.2 — `/stack.html` removed** (`3801192`)
- The primary action on all 122 compound and stack pages is now "Reconstituting this? Do the math." → `/calculator.html`.
- Homepage door 02 is now "Math — Vial-to-syringe math." → `/calculator.html`.
- Deleted: `stack.html`, `og/stack.png`, and their entries in `data/page-dates.json` and `data/og-manifest.json`. The Part 5 migration step no longer recreates the placeholder.
- A scan of every page, script and data file finds no link to `/stack.html` left; two code comments record the removal.

**Part 1.3 — evidence levels re-derived** (`c8121c9`)
- Every compound's Human Data section (all 113) was re-read against your rule: published human data on the compound itself, for the use on its tag.
- Result: 16 levels change, including AICAR, VIP and TB-500 → 1.
- `scripts/card_data.py` encodes the rule. Overrides now apply before the FDA default, `APPROVED_FOR` records why each level-4 drug earns it, and a newly approved drug stops the run until someone classifies it.
- Updated: `data/regulatory-status.json` (which also stores the rule text) and `gsc-export/card-data.csv`. On the site, 16 card meters on `/compounds/` changed.

**Part 1.4 — About** (`08bbfb6`)
- `about.html` carries your copy verbatim with your headings. The template is kept; the photo and placeholders are gone. A word-by-word comparison against your text found no differences.
- The About link is live in the header template, so it is on all 455 pages, and `about.html` is in the sitemap.

**Part 1.5 — CLAUDE.md** (`069bfe7`)
- The Design System section is now a pointer to `kalios-design/street-lab-spec.md` as the design system of record, plus where the stylesheet, script, templates and card data live.
- New section "Templates and generated blocks" documents the header template and every `sl:` block: what it holds, its inputs, and the Checked-date rule.
- The "copy BPC-157's `<style>` byte-for-byte" rule is deleted in all three places it appeared.

**Part 1.6 — sitemap lastmod** (`efe3d82`)
- `lastmod` now comes from `data/page-dates.json` (last content change), not git.
- Result: 450 URLs, 200 dated 26 Sep and 250 dated 27 Sep. Under the git rule, every URL had read 27 Sep since the redesign commits.
- All 450 dates were checked against the data file: 0 mismatches.

**Part 2 — promote** (`8fddc8f`)
1. Added a changelog line. The homepage now reads "28 Sep — New site design: use, evidence and legal status on every card".
2. `prerender.py`: idempotent apart from that line. Committed.
3. Git-less copy: `diff -rq` listed only `.git`, `.claude`, `reports` and a `.DS_Store`.
4. `vercel --prod` → READY, target production (946 files, 19.7 MB uploaded).
5. `push-to-indexnow.sh` → 450 URLs, HTTP 200.
6. `curl` checks → all pass (table below).

---

## DECISIONS

**1.1 — band, line, alert block**
1. **Band rule:** status PCAC RECOMMENDED or PCAC 2027, or a PCAC vote on record (`shows_caution` in `scripts/streetlab.py`).
   - That gives exactly the 11 PCAC pages + DSIP.
   - Written as a rule rather than a DSIP special case, so a future voted-against compound gets the band too.
   - DSIP keeps its title "Not legal to compound" (no "— yet").
2. **The research-only line on seven pages names the FDA list instead of saying "not on any".**
   - The check: I downloaded FDA's 503A categories list (updated May 14, 2026) and checked every research-only compound's name and aliases against Categories 1–3.
   - The seven: MK-677 and kisspeptin are Category 2 ("significant safety risks"); ARA-290, GHRP-2, GHRP-6, MGF and thymulin are Category 3 ("nominated without adequate support").
   - Their line reads "Research only · on FDA's 503A Category N list (…) · Tell me if this changes →", with "503A Category N list" linking to FDA's PDF.
   - Data: new `fda_503a_category` field, set in `scripts/gen_regulatory_status.py`, so it survives regeneration.
   - The other 73 lines are your wording exactly.
   - Five of the seven pages already say so in their text; ARA-290's and thymulin's don't (Open question 7).
3. **Line style.** The line uses the mono 10.5px uppercase of the fine-print strip; the source keeps your casing. It sits 10px under the table rule.
4. **Alert block.** The "Status alerts" block holds the band's form with the same input, label and button text, so there is no new copy.
   - It sits at the end of `<main>`, after "Usually looked up with" and just above the disclaimer and footer.
   - On the dark ground: transparent input and ghost button with `--line-strong` borders. The spec reserves paper for the slip, the search field and the primary button.
   - Like the band's form, the email field shows about 12 characters of placeholder at 390px. I kept one form component rather than diverging.
5. **Stacks** get neither the line nor the block: their status is NOT LEGAL TO COMPOUND, not research-only.

**1.2 — `/stack.html`**
6. **Both links swap to COA Check when it ships:** the compound-page primary action and homepage door 02. The code comment at `CALCULATOR` in `scripts/streetlab.py` says the same.
7. **Stack pages get the same primary action.** They are the compound template's stack variant, and the spec wants one primary action per page.
8. **Wording as given:** "Reconstituting this? Do the math." and "Math — Vial-to-syringe math." The doors' "Three ways in" label is unchanged.
9. **Applied to every compound page as instructed**, including 25 whose Quick Facts route is oral or topical (Open question 5):
   - Oral: bromantane, dihexa, enclomiphene, GW-0742, MK-677, orforglipron, tesofensine, zuclomiphene.
   - Topical: AHK-Cu, argireline, decapeptide-12, Matrixyl, nonapeptide-1, Pal-AHK, Pal-GHK, palmitoyl dipeptide-6, pentapeptide-18, Rigin, SNAP-8, Syn-Ake, Syn-Coll, tripeptide-29, Vialox.
   - No human route: SLU-PP-332, waglerin-1.

**1.3 — evidence**
10. **The rule, as implemented:**
    - 4 = FDA-approved today for the use on the tag.
    - 3 = published controlled trials for that use.
    - 2 = published pilots: small, open-label or early studies, case series.
    - 1 = animal or lab data only, for that use.
    - 0 = no data of its own for that use; stacks.
    - Human trials for other uses, and data on a parent, precursor or analog, don't count.
11. **Pilot vs trial grading was not re-litigated.** Where a compound's human data is on its tag's use and on the compound itself, the previous grading stands (Matrixyl 3, argireline 2, and so on).
12. **Hormone-axis tags.** The Hormones and Growth hormone tags count an FDA approval for a therapy on that axis, so tesamorelin, triptorelin and insulin stay at 4. Specific-use tags (Fat loss, Mood & focus, Longevity, Skin, …) need the approval to be for that use.
13. **Publication taken at the page's word.** Where a page states that trials were published, I kept the level:
    - AOD-9604: "six published trials", cited through company results and reviews.
    - VK2735: a 2024 ObesityWeek abstract.
    - Pemvidutide: "published", no citation.

    All three stay 3; worth a source check (Open question 4).
14. **Uncited Russian series don't count**; named or cited ones do.
    - Counted: pinealon's 72-patient series; thymagen's cohort (PMID 9026934).
    - Not counted: livagen, pancragen, testagen and vesugen, whose pages describe reports without citing them.
15. **ARA-290 goes to 0 on a literal reading of its tag.** Its Phase 2 RCTs are for small-fiber neuropathy (nerve repair), and the page has no tendon or gut data, human or animal.

**1.4 — About**
16. **Typography.** Apostrophes and dashes are typographic entities, as elsewhere on the site.
    - The four signals use the site's existing `key-points` list; the three rules keep the template's numbered list.
    - The email address and Substack are links.
17. **Kept:** the "← Home" back link, the `<title>` and the meta description (not visible text, still accurate), and the page's CSS.
18. **Anchors.** The old `#evidence` and `#contact` became `#read`, `#not` and `#corrections`; nothing linked to the old ones.
19. **Dates.** The About page's "Checked" date moved to 27 Sep because its content changed. No other page's date moved all session: template and data changes live in generated blocks.

**1.5 — CLAUDE.md**
20. **Nav description.** Section 1 of the locked template keeps its name "Nav" (the 17 sections can't be renamed) with a new description. Section 17 now describes the disclaimer plus the generated footer.
21. **One new rule.** "NEVER modify the CSS — use the BPC-157 template exactly" became "NEVER hand-edit inside an `sl:` block".
22. **Not touched:** the stale counts in "What This Is" and Tech Stack ("112 compound profiles, 5 stack profiles", "117 HTML files"). Out of scope.

**Part 2**
23. **The changelog line went in before the deploy prerender**, so the live homepage shows it (the brief lists it last).
    - Dated 2026-09-28, since the file keeps one entry per report date.
    - Text: "New site design: use, evidence and legal status on every card".
24. **Commits: one per fix in Part 1, so each fix can be reverted alone.** I read "commit per part" as the minimum.
    - Samples were validated in the working tree before scaling (rule 6): 9 pages for 1.1, 3 for 1.2, 2 for 1.4.

---

## Evidence levels — the 16 changes

Rule: published human data on the compound itself, for the use on its tag. The full table, with a reason for every judgment call, is in `gsc-export/card-data.csv`.

| Compound | Tag | Was → Now | Why |
|---|---|---|---|
| AICAR | Fat loss | 3 → **1** | Your call. Its trials are acadesine in cardiac surgery, AMPD deficiency and CLL, not fat loss |
| VIP | Immune | 3 → **1** | Your call. Its trials are PAH and COVID respiratory failure; the one immune-use report is an uncontrolled CIRS practice series |
| TB-500 | Tendon & gut repair | 2 → **1** | Your call. Human data are on full-length thymosin β4, the parent; SubQ TB-500 itself is unstudied |
| Dulaglutide | Fat loss | 4 → **3** | Approved for type 2 diabetes; weight data come from its diabetes RCTs |
| Pramlintide | Fat loss | 4 → **3** | Approved for diabetes; published obesity RCTs (Aronne 2007, Smith 2008) |
| Oxytocin | Mood & focus | 4 → **3** | Approved for labor; intranasal RCTs on social cognition and anxiety, incl. SOARS-B (NEJM 2021) |
| Methylene blue | Mood & focus | 4 → **2** | Approved for methemoglobinemia. For cognition, one randomized single-dose fMRI study (n=26); the dementia trials used a derivative |
| SS-31 | Longevity | 4 → **2** | Approved for Barth syndrome. For aging, one randomized single-dose study in older adults; its myopathy and heart trials are other uses |
| ARA-290 | Tendon & gut repair | 3 → **0** | Its RCTs are nerve repair (small-fiber neuropathy); no tendon or gut data |
| LL-37 | Immune | 2 → **1** | Human trials are topical leg-ulcer healing and an analog (OP-145) |
| Semax | Mood & focus | 3 → **2** | Its stroke trials belong to another use; small Russian attention and cognition studies |
| Ipamorelin | Growth hormone | 3 → **2** | Phase 1 GH-release studies; its Phase 2 was for postoperative ileus |
| Glutathione | Longevity | 3 → **2** | One RCT on raising body glutathione (n=54); the skin RCTs are another use and NAC is a precursor |
| Zuclomiphene | Hormones | 2 → **1** | Its PK data come from clomiphene, the parent mixture; its own Phase 2 is an unpublished topline |
| Vilon | Immune | 2 → **1** | The cited cohort arm combined Vilon with Epithalon and measured mortality |
| DS5 | Mood & focus | 1 → **0** | No study of the blend; its components' data don't count |

**Totals** (123 rows: 113 compounds, 4 aliases, 6 stacks)
- Now: No data 11 · Animal only 52 · Human pilots 22 · Human trials 25 · Approved drug 13.
- Before: 9 · 47 · 21 · 28 · 18.

**Unchanged, but worth knowing**
- **Tesamorelin, triptorelin, insulin: 4.** Hormone-axis tag reading (DECISIONS 12).
- **Melanotan I: 4.** EPP is a skin indication.
- **BPC-157: 2.** The knee-pain case series (n=16) is its one pilot on tendon or joint repair; the cystitis pilot is bladder and the IV pilot is safety-only.
- **NAD+: 2.** One IV pilot (n=11); the NR and NMN trials are precursors.
- **Epithalon: 2.** Small human aging reports. See Open question 7 on its cohort citation.

---

## Checks

**Structural audit (`scripts/street_lab_audit.py`)**
- After each part, every page passes against the previous commit (456 pages in 1.1, 455 once `/stack.html` was gone). The only failure was `about.html` in 1.4, from the intended rewrite; its structural checks passed.
- Against the session's starting commit `06ce8cb`: every page except `about.html` still has all its headings, tables, references, text and links.

**Behaviour (headless Chrome, local server)**
- The "Tell me if this changes →" link jumps to the alert block.
- An empty address is stopped by the browser's `required` check and nothing is sent.
- The primary action goes to the calculator. Semaglutide shows no band, line or block.
- The menu opens and lists Compounds · Stack tool · Calculator · FDA tracker · Alerts · About. Door 02 goes to the calculator.
- No console errors.

**Screenshots (390px and 1280px; not committed)**
- The research-only line, the Category 2 and 3 lines, and the alert block on adipotide, MK-677 and thymulin.
- The About page at both widths.
- None scrolls sideways.

**Live (`curl`, www.kalios.health, after the deploy)**

| URL | Status | Checked for |
|---|---|---|
| `/` | 200 | About link; door 02 "Math — Vial-to-syringe math."; new "what changed" line; no `/stack.html`, no "COA check" |
| `/compounds/` | 200 | About link; AICAR card reads "Animal only" |
| `/compounds/bpc-157.html` | 200 | Band "Not legal to compound — yet"; new primary action; no status line; no old primary action |
| `/compounds/adipotide.html` | 200 | Research-only line and `#alerts` block; no band; new primary action |
| `/compounds/semaglutide.html` | 200 | New primary action; no band, line or block |
| `/compounds/mito-stack.html` | 200 | New primary action |
| `/stacks/pairs/bpc-157-with-tb-500/` | 200 | About link |
| `/fda-pcac-2026.html` | 200 | About link |
| `/about.html` | 200 | New copy; no placeholder |
| `/calculator.html` | 200 | About link |
| `/stack.html` | **404** | Served by the 404 page ("Not here") |

- None of the ten pages loads Tailwind.
- Also checked live:
  - `/compounds/dsip.html` has the band.
  - `/compounds/mk-677.html` has the Category 2 line.
  - `/data/page-dates.json`, `/templates/header.html`, `/CLAUDE.md`, `/reports/`, `/gsc-export/card-data.csv` and `/og/stack.png` all return 404.

---

## Counts

- **Pages:** 455 deployable (456 before, minus `stack.html`) + 2 untouched redirect stubs.
- **Legal-status blocks:**
  - Band: 12 pages.
  - Research-only line + alert block: 80 pages (73 with your wording, 2 Category 2, 5 Category 3).
  - Neither: 24 approved or compounding-pharmacy pages and 6 stacks.
- **Primary action changed:** 122 pages. **About link added:** 455 pages.
- **Evidence levels changed:** 16 of 123 rows.
- **Sitemap:** 450 URLs (+ `about.html`).
- **Content dates moved this session:** 1 (`about.html`).

## Files changed (main ones)

- **Scripts:** `scripts/streetlab.py`, `scripts/card_data.py`, `scripts/gen_regulatory_status.py`, `scripts/street_lab_migrate.py`, `scripts/gen_sitemap.py`
- **Templates and assets:** `assets/street-lab.css`, `templates/header.html`
- **Pages:**
  - `about.html` and `index.html`
  - `compounds/*.html` (122 pages) and `compounds/index.html`
  - the header on every page
  - `sitemap.xml`
- **Data:**
  - `data/regulatory-status.json`, `data/page-dates.json`, `data/og-manifest.json`, `data/changelog.json`
  - `gsc-export/card-data.csv`
- **Docs:** `CLAUDE.md`
- **Deleted:** `stack.html`, `og/stack.png`

## URLs to eye-check (www.kalios.health)

- `/` (door 02, "what changed" line) and `/compounds/` (meters).
- **Compound pages:**
  - `/compounds/bpc-157.html`: band.
  - `/compounds/dsip.html`: the voted-against band.
  - `/compounds/adipotide.html`: the line under the data table, and the alert block at the end.
  - `/compounds/mk-677.html` and `/compounds/thymulin.html`: the Category lines.
  - `/compounds/semaglutide.html`: none of the three.
- **Primary action in odd places:**
  - `/compounds/argireline.html`: topical.
  - `/compounds/orforglipron.html`: oral.
  - `/compounds/mito-stack.html`: a stack.
- **Evidence and legal status, side by side:** `/compounds/ara-290.html` ("No data" plus a Category 3 line) and `/compounds/methylene-blue.html` ("Approved" stamp, "Human pilots" meter).
- `/about.html`, `/calculator.html`, `/stack.html` (404).

## Open questions for G

1. **About page wording.** Two definitions contradict the data (Look at these first, 1). Possible fixes, both one-phrase edits:
   - "RESEARCH ONLY: not on any FDA list that allows compounding; sold as a research chemical."
   - "NOT LEGAL YET: the FDA's advisory committee has recommended it or scheduled it; …"

   Say the word and I'll update `about.html`.
2. **The seven Category 2/3 lines.** Keep the wording in DECISIONS 2, or pick another?
3. **ARA-290's tag.** Its evidence is nerve repair, and no chip covers that. Add a chip, move it to another group, or leave "No data" under Tendon & gut repair?
4. **Evidence calls to confirm:**
   - the hormone-axis reading (tesamorelin, triptorelin, insulin at 4)
   - methylene blue and SS-31 at "Human pilots"
   - the publication status of the AOD-9604, VK2735 and pemvidutide trials
5. **Primary action on the 25 oral and topical compounds.** Keep "Reconstituting this? Do the math.", hide it on those pages, or use other text until COA Check ships?
6. **Homepage author card.** The HomeDesktop author card ("Why Kalios exists →") was left out because About wasn't signed off. Build it now?
7. **Content, found while re-reading (not changed; each needs a source check):**
   - Khavinson & Morozov 2003 (PMID 14523363) is described four ways:
     - `vilon.html`: a "Vilon + Epithalon" arm.
     - `thymalin.html`: Thymalin / Epithalamin arms.
     - `epithalon.html`: its 266-patient cohort, attributed to Epithalon.
     - `n-acetyl-epithalon.html`: parent Epithalamin, the extract.
   - `ara-290.html` and `thymulin.html` don't mention that they are on FDA's 503A Category 3 list; the new line does.
8. **Carried over, not addressed:**
   - GHK-Cu band wording (non-injectable GHK-Cu is back in Category 1).
   - The 503B question.
   - The Vercel author block: deploys still go through the git-less copy.
   - Brave key rotation.
   - Identity-workflow open questions 5–10.
   - Content-truth open questions.

## Web content

Rule 3: everything fetched was treated as data; nothing was acted on as an instruction, and no instruction-like text was seen.
- **FDA:** "Bulk Drug Substances Nominated for Use in Compounding Under Section 503A" (https://www.fda.gov/media/94155/download, updated May 14, 2026). Used to check the research-only line.
- **Packages:** npm `puppeteer-core` and PyPI `pypdf`, installed in the session scratchpad only.
- **HTTP checks:** www.kalios.health and the IndexNow API.
