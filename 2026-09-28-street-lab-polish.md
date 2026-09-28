# Street Lab polish — 2026-09-28

All ten items of the STREET LAB — POLISH PASS brief are done, and the result is live on www.kalios.health.

- **Production:** `dpl_5QpBVtvoDyqMurCB5Qn9ZMKygfUv`, state READY, target production, built from commit `ba5d568` through the git-less copy (CLAUDE.md rule 4). Every URL the brief lists returns 200 with the new content (table under Checks).
- **IndexNow:** 450 URLs submitted, HTTP 200.
- **Commits (all pushed):** `7325c1c` 1 · `5f327ed` 2 · `0d95f60` 3 · `2fe3581` 4 · `6cd77d6` 5 · `307806a` 6 · `75cb563` 7 · `ec59898` 8 · `ba5d568` screenshots. Item 9 changed no file (see below). This report is the 10th commit.
- **Scale:** 139 files changed (+786 / −152): 7 added, 1 deleted (`art/kalios-wordmark-480.webp`), 131 modified.

**Session start (rule 2)**
- Claude Code 2.1.283.
- Vercel CLI 60.1.3, the same as the latest on npm.
- `brew outdated`: 38 formulae (e.g. `git`, `node`, `openssl@3`) and 1 cask (`visual-studio-code`). None were updated.
- The site's only third-party code is the Umami script, so no pinned library needed updating. Test tooling (puppeteer-core 25.12, Lighthouse 13.5) lived in the session scratchpad, not the repo.

---

## Look at these first

1. **5-amino-1MQ is not one of the 25, so it still says "Reconstituting this? Do the math."**
   - The brief's curl list calls it "(oral)". Its Quick Facts route is "Oral / SubQ (preclinical)", though, and its Reconstitution section covers a lyophilized powder for SubQ use.
   - I applied the new action to exactly the 25 (item 8), and the curl check confirms 5-amino-1MQ kept the calculator link.
   - If you want it moved, it's one slug in `NO_RECONSTITUTION`. It would then read "See every Fat loss compound" (Open question 1).
2. **Where the spray mark now appears:**
   - the homepage hero
   - `/og-default.png`, where it replaces the drip
   - It is not on the 455 per-page OG images. They keep their drip: they carry no spray mark, so the drip doesn't sit beside it. I read "share cards" as the spec's COA share card (component 9), which isn't built yet. CLAUDE.md now says that card takes the spray mark.
   - If "share cards" meant the per-page OG images, it's one parameter and a prerender run (Open question 2).
3. **The homepage is heavier, and longer on phones.**
   - The full transparent mark weighs 75 KB on phones and 117 KB on desktop, against 18.6 KB for the old cropped letters. The alpha channel carries the spray texture, and this export keeps alpha lossless (DECISIONS 3).
   - Measured on the same local server (Lighthouse mobile, 3 runs each):
     - Performance: 99–100 both before and after.
     - Median LCP: 1.65 s → 1.80 s. The LCP element is the H1 text, not the image.
     - Page weight: 182 KB → 242 KB.
   - On phones the homepage runs about 500 px longer: the four "start with" cards in one column (+275 px; the six-element card doesn't fit two across at 390 px), the author card (+145 px) and the full-size mark (+85 px).
   - The headline now starts 86 px lower on phones (the mark is 226 px tall, against 141 px) and 51 px lower on desktop (DECISIONS 19).
4. **The author card on phones** sits after the "what changed" line. Item 5 was desktop-only, and "under the mark" on a phone would push the headline down by a card (Open question 3).

---

## What was done

**1. Wordmark** (`7325c1c`)
- **New script, `scripts/wordmark.py`:** keys `art/kalios-wordmark.png` to a transparent background and exports the full composition (tag, drips, spray halo, its own red dot):
  - `art/kalios-wordmark-400.webp`: 400×452, for 200 px on phones.
  - `art/kalios-wordmark-520.webp`: 520×588, for 260 px on desktop.
- **Homepage:** one `<img>` with `srcset`.
  - Phones: above the headline, left-aligned.
  - Desktop: the right column, vertically centred on the headline block (measured: both centres at the same y).
- **Removed:** the inline drip SVG from the homepage (its CSS and the `DRIP_SVG` constant are gone too) and `art/kalios-wordmark-480.webp`. `art/kalios-wordmark.png` is untouched.
- **`/og-default.png`:** carries the spray mark in the drip's place.
- **CLAUDE.md:** records the usage rule. The spray mark goes on the hero, share cards and OG default only; Anton KALIOS.HEALTH stays the header and footer wordmark.

**2. Caution band** (`5f327ed`)
- **The band:** the title, one sentence, and the alert form.
  - PCAC RECOMMENDED (BPC-157, Epitalon, KPV, MOTS-c, Semax, TB-500): "The FDA’s advisory committee recommended it on July 23–24, 2026. Recommended is not legal — nothing changes until rulemaking is finished."
  - PCAC 2027 (dihexa, GHK-Cu, LL-37, Melanotan II, PEG-MGF): "The FDA plans a committee review before the end of February 2027."
  - DSIP: "The FDA’s advisory committee voted against it on July 23–24, 2026."
- **The full notice:** each page's PCAC notice (`caution_note_html`), verbatim, opens its Regulatory Status section, through a new generated block, `sl:pcac-note`. It is filled on those 12 pages and empty on the other 104 compound pages.

**3. Footer fine print** (`0d95f60`)
- The disclaimer is now fine print: IBM Plex Mono 10.5 px, `--ink-muted`, max 70ch. It sits under the footer's hairline, just above the three clauses. The wording is unchanged.
- Checked on the homepage, a compound, a pair page, the stack tool, the guide and About at 390 and 1280 px. The text lines up with the footer's (x = 20 / 96 px).

**4. One card component** (`2fe3581`)
- The homepage's four cards are the `/compounds/` card: use tag, name, hook, evidence meter, legal stamp, red flag when real, and the Updated dot.
- Each card's HTML is byte-identical to the same card on `/compounds/`.
- The compact variant is deleted from the code and the CSS.

**5. Hero composition** (`6cd77d6`)
- **Desktop:**
  - Headline and sub-line on the left, with the search under them spanning the left column, then the three doors.
  - The mark on the right, with the author card under it.
  - The "what changed" line runs full width under both columns.
- **The author card**, in your words: "Written by G — a dad who lifts and reads the studies. No credentials, no vendors, nothing to sell." It ends with "Why Kalios exists →", linking to `/about.html`.
- The homepage's content changed, so its "Checked" date and sitemap `lastmod` are now 28 Sep.

**6. About** (`307806a`)
- RESEARCH ONLY now reads "not on any FDA list that allows compounding; sold as a research chemical."
- NOT LEGAL YET now reads "the FDA’s advisory committee has recommended it or scheduled it; recommended is not legal, and nothing changes until rulemaking is finished."
- Nothing else on the page changed. Its "Checked" date and `lastmod` are now 28 Sep.

**7. ARA-290** (`75cb563`)
- "Nerve & pain" joins the chip vocabulary. ARA-290 moves to it at evidence 3 (Human trials).
- Its Human Data section lists published Phase 2 RCTs in small-fiber neuropathy: sarcoidosis (Dahan 2013, Culver 2017) and type 2 diabetes (Brines).
- `scripts/card_data.py` was re-run, and `data/regulatory-status.json` and `gsc-export/card-data.csv` were diffed field by field. **ARA-290 is the only row that changed** (use tag, evidence level and their reasons).
- `/compounds/` gains a "Nerve & pain" group and chip (1). Tendon & gut repair drops from 6 to 5.

**8. Primary action on the 25** (`ec59898`)
- The 25 oral and topical compounds now say "See every {use tag} compound", linking to `/compounds/` filtered to that tag (`?tag=<group>#<group>`). The list is under DECISIONS.
- The other 91 compound pages and the 6 stacks keep "Reconstituting this? Do the math." → the calculator.
- **The filter on `/compounds/`:**
  - `?tag=` shows that group only, with "‹Tag› only · Show all" above it.
  - A chip switches the group; typing a search clears it.
  - Without JavaScript, the link still lands on the group.

**9. Brave key: closed**
- It's marked closed in the carried-over list below.
- CLAUDE.md doesn't reference the rotation (its only Brave mentions are the `brave_web_search` citation rule), so it needed no change, and item 9 has no commit.
- Old reports are left as written.

**10. Deploy**
- Prerender (no changes) → git-less copy (`diff -rq` listed only `.git`, `.claude`, `reports`, `art/.DS_Store`) → `vercel --prod` → `push-to-indexnow.sh` → curl checks.
- The first `vercel --prod` came back "Not authorized". No deployment was created. `vercel whoami` answered normally, and the retry succeeded (`dpl_5QpBVtvoDyqMurCB5Qn9ZMKygfUv`).

---

## DECISIONS

**1. Wordmark**
1. **Keying.** alpha = (max(R,G,B) − 10) / 245, and the colour is un-blended from #0a0a0a, the page black the PNG was flattened on.
   - Max channel, not Rec. 601 luma: luma would give the red dot an alpha of 0.29 and darken it.
   - The letters are darker than the page (#020203), so they key out too. On the site's black they look exactly as before; on a light background they would show through, and the site never uses one.
   - Checked: each file, composited over #0a0a0a, matches the source to a mean of 0.8 levels, p99 5. A few pixels on the red dot's edge are off by up to 38 levels (lossy WebP subsamples colour).
2. **Trim.** Only the empty black margin was cut: 32 px left, 35 px top, 1 px right and 24 px bottom of the 797×923 source. The phone mark therefore lines up with the text edge.
3. **Encoding.** WebP quality 90 with lossless alpha. Lower alpha quality saved little: about 10% at alpha quality 75, which already cuts the halo's 245 alpha levels to 56.
4. **`/og-default.png`.**
   - The spray mark sits at the drip's height (288 px), top right.
   - The file is full colour: 119 KB, against 15.7 KB at 16 colours. Sixteen colours band the halo.
5. **The spec is not edited** (v1.1, "final"). Your amendments are recorded in CLAUDE.md's Design System section as "G's amendments since spec v1.1", which win over the spec and the artboards. There is one bullet per item: spray mark, band, fine print, card, hero, primary action.
6. **Alt text** stays "Kalios". The mark keeps `fetchpriority="high"`.

**2. Caution band**
7. **The sentences live in the data file** (`caution_band`, written by `scripts/gen_regulatory_status.py`), because the spec says the band's text comes from `regulatory-status.json`. Regenerating the file added only that key.
8. **Typographic apostrophes** ("FDA’s"), as elsewhere on the site. Your dashes are kept as written.
9. **Titles unchanged:** "Not legal to compound — yet" (the PCAC statuses) and "Not legal to compound" (DSIP).
10. **The "Full tracker →" link moved with the notice.** It is part of the notice text, so the band now has no link.
11. **The notice's box** is bordered like the status box below it: no stripe, no yellow. Attitude stays in the band.
12. **`sl:pcac-note` is on all 116 compound pages** (empty where there is no notice), like `sl:status` and `sl:alerts`. A band whose status has no sentence now stops prerender instead of rendering blank.
    - Sample first: bpc-157, dsip, ghk-cu, adipotide.
    - No page's "Checked" date moved.

**3. Footer**
13. **CSS only.** The disclaimer stays page content. The footer's hairline moved above the disclaimer, so disclaimer and clauses read as one block.
14. **Sentence case, normal tracking.** The brief didn't ask for uppercase, and a paragraph of uppercase mono is hard to read. 70ch is 441 px.

**4. Cards**
15. **The homepage grid** is one column on phones and two across from 640 px, desktop included. Four across would squeeze the meter against the stamp.
16. **The Updated dot is included,** because it's part of the `/compounds/` card. All four show "Updated Sep 27" for now.

**5. Hero**
17. **Your wording, verbatim, as one line,** with "Written by G" in bold.
    - The artboard splits it into a label and a body, which would have changed your punctuation.
    - The artboard's last sentence ("Every claim has a source you can click.") is left out, as in your brief.
18. **The right column is the mark's width (260 px);** the author card matches it.
19. **Mutual centring.** The mark (294 px tall) is taller than the headline block (193 px), so the row takes the mark's height. The headline therefore sits 51 px lower than before.
20. **Phones:** the author card goes after the "what changed" line (Look at these first, 4). The author card is page content in `index.html`.

**7. ARA-290**
21. **"Nerve & pain" goes last in the vocabulary, after "Hormones",** so no existing group on `/compounds/` moves. It is the second extension to the chip set.

**8. Primary action: the 25, as logged**
22. **The list** is the one from the promote report's DECISIONS 9, where the Quick Facts route is oral or topical only, or there's no human route:
    - **Oral (8):** bromantane, dihexa, enclomiphene, GW-0742, MK-677, orforglipron, tesofensine, zuclomiphene.
    - **Topical (15):** AHK-Cu, argireline, decapeptide-12, Matrixyl, nonapeptide-1, Pal-AHK, Pal-GHK, palmitoyl dipeptide-6, pentapeptide-18, Rigin, SNAP-8, Syn-Ake, Syn-Coll, tripeptide-29, Vialox.
    - **No human route (2):** SLU-PP-332, waglerin-1.
    - **By tag:** Skin 14 · Fat loss 4 · Hair 2 · Hormones 2 · Mood & focus 2 · Growth hormone 1.
23. **The tag is used as the cards write it:** "See every Fat loss compound", "See every Mood & focus compound", "See every Hormones compound".
    - The arrow is the button's own arrow icon, as on the calculator action, not a second "→" in the text.
    - Lower-case tags would read more naturally ("See every skin compound"). Say so and it's one line.
24. **Intranasal compounds keep the calculator** (Semax, N-Acetyl Semax, N-Acetyl Selank, Adamax, DS5). They aren't in the 25, and nasal sprays are reconstituted too.
25. **Dihexa is both a band page and one of the 25.** It shows the PCAC 2027 band and "See every Mood & focus compound".
26. **The link is `/compounds/?tag=skin#skin`.** The query filters with JavaScript, and the hash lands on the group without it.
    - The page's inline script and its copy in `scripts/street_lab_migrate.py` are identical.
    - Sample first: argireline, MK-677, waglerin-1, dihexa, plus 5-amino-1MQ as a control.

---

## Checks

**Structural audit (`scripts/street_lab_audit.py`)**
- After each item, every page passed against the previous commit (455 pages). The one exception is item 6: `about.html` failed "text nodes lost" on the edited list item only, and its headings, links and JSON-LD all passed.
- Against the session's starting commit `8fc0dc5`: 455 pages checked, 1 failed. It is `about.html`, for the same intended edit.
- Prerender is idempotent: a second run changes 0 files. `git ls-files --deleted` was empty before deploying.

**Behaviour (headless Chrome, 21 checks, run locally and again on production; all pass)**
- The argireline button reads "See every Skin compound" and lands on `/compounds/?tag=skin#skin`. Only Skin shows (17 cards), with "Skin only · Show all", scrolled to the group.
- The Hair chip switches to Hair. Typing "bpc" searches every group (5 matches). "Show all" returns all 15 groups. An unknown `?tag=` filters nothing.
- The existing search still works: `?q=Ozempic` opens semaglutide, and `?q=bpc` shows 5 cards.
- Without JavaScript, `/compounds/?tag=skin#skin` shows all groups with Skin at the top.
- BPC-157, 5-amino-1MQ and semaglutide keep the calculator.
- No console errors.
- These runs, the screenshots and the Lighthouse runs against production added a few test visits to Umami.

**Screenshots** (committed in `ba5d568`): `gsc-export/street-lab-shots/polish/`
- `home-390.webp` and `home-1280.webp`
- `bpc-157-390.jpg` and `bpc-157-1280.webp`

They are full page at scale 1 (JPEG where a page is taller than WebP's 16,383 px limit). No page scrolls sideways.

**Lighthouse (mobile)**

Two runs against production scored 99 and 85; that is network variance, so the comparison below is local.

The LCP element is the H1 in all six local runs.

| Local static server, 3 runs each | Performance | Median LCP | Median FCP | Page weight |
|---|---|---|---|---|
| Session start (`8fc0dc5`) | 99, 100, 100 | 1.65 s | 1.05 s | 182 KB |
| Now (`ba5d568`) | 100, 100, 99 | 1.80 s | 1.05 s | 242 KB |

**Live (`curl`, www.kalios.health, after the deploy): 42 of 42 checks pass**

| URL | Status | Checked for |
|---|---|---|
| `/` | 200 | spray mark with the 400/520 `srcset`; no drip or cropped WebP; author card in your words, linking to `/about.html`; 4 six-element cards, no compact variant; "Checked 28 Sep 2026" |
| `/compounds/bpc-157.html` | 200 | band = title + the PCAC RECOMMENDED sentence + form, no notice in the band; full notice at the top of Legal status; calculator |
| `/compounds/dsip.html` | 200 | band with the voted-against sentence; full notice in Legal status; calculator |
| `/compounds/ghk-cu.html` | 200 | band with the PCAC 2027 sentence; full notice in Legal status; calculator (topical and SubQ) |
| `/compounds/semaglutide.html` | 200 | no band, no notice; calculator |
| `/compounds/ara-290.html` | 200 | Category 3 research-only line; calculator (SubQ) |
| `/compounds/5-amino-1mq.html` | 200 | calculator: not one of the 25 (Look at these first, 1) |
| `/about.html` | 200 | both new definitions; the old wording is gone |
| `/compounds/` | 200 | ARA-290 card reads "Nerve & pain" and "Evidence · Human trials"; Nerve & pain chip; the `?tag=` script |
| `/compounds/argireline.html` | 200 | "See every Skin compound" → `/compounds/?tag=skin#skin` |

- Every page above loads no Tailwind and carries its disclaimer.
- Also checked live:
  - `/art/kalios-wordmark-400.webp`, `-520.webp` and `/og-default.png` return 200.
  - The live stylesheet has the fine-print, notice and card rules, and no "compact".
  - `/art/kalios-wordmark-480.webp` returns 404, as do `/reports/`, `/CLAUDE.md`, `/scripts/wordmark.py` and `/gsc-export/…`.

---

## Counts

- **Pages changed:** 116 compound pages (all gained `sl:pcac-note`), the homepage, `/compounds/` and About.
  - Caution band changed on 12: 6 recommended, 5 PCAC 2027, 1 voted against.
  - Full PCAC notice now in Legal status on the same 12.
  - Primary action changed on 25. The calculator stays on 97 (91 compounds + 6 stacks).
- **Evidence levels** (123 rows: 113 compounds, 4 aliases, 6 stacks):
  - Now: No data 10 · Animal only 52 · Human pilots 22 · Human trials 26 · Approved drug 13.
  - Before: 11 · 52 · 22 · 25 · 13. The only change is ARA-290.
- **Chip vocabulary:** 14 tags (13 + Nerve & pain). `/compounds/` has 15 groups.
- **Content dates moved:** 2 (`index.html`, `about.html`) → 28 Sep, with the same two sitemap `lastmod`s. The sitemap has 450 URLs.

## Files changed (main ones)

- **Scripts:**
  - new: `scripts/wordmark.py`
  - changed: `scripts/streetlab.py`, `scripts/og_images.py`, `scripts/gen_regulatory_status.py`, `scripts/card_data.py`, `scripts/street_lab_migrate.py` (new step `pcac-note-blocks`)
- **Assets:** `assets/street-lab.css`; `art/kalios-wordmark-400.webp` and `-520.webp` (new); `art/kalios-wordmark-480.webp` (deleted); `og-default.png`.
- **Pages:** `index.html`, `about.html`, `compounds/index.html`, the 116 compound pages.
- **Data:** `data/regulatory-status.json`, `data/page-dates.json`, `gsc-export/card-data.csv`, `sitemap.xml`.
- **Docs:** `CLAUDE.md` (the generated-block list, and your amendments since spec v1.1).
- **QA:** `gsc-export/street-lab-shots/polish/` (4 screenshots).

## URLs to eye-check (www.kalios.health)

- `/` at phone and desktop width: the mark, the author card, the four cards, and the fine print at the bottom.
- `/compounds/bpc-157.html`: the two-line band, then the full notice at the top of Legal status.
- `/compounds/dsip.html` and `/compounds/ghk-cu.html`: the other two band sentences.
- `/compounds/argireline.html` and `/compounds/mk-677.html`: the new button, and where it lands.
- `/compounds/?tag=hair` and `/compounds/#nerve-pain` (ARA-290).
- `/compounds/5-amino-1mq.html`: still the calculator (Look at these first, 1).
- `/about.html`: the Legal stamp definitions.
- `/og-default.png`: the spray mark on the OG card.

## Open questions for G

1. **5-amino-1MQ:** move it to "See every Fat loss compound"? Its page covers both oral capsules and a SubQ powder.
2. **Share cards:** did you mean the per-page OG images too? If yes, all 455 get the spray mark in place of the drip.
3. **Author card on phones:** keep it after the "what changed" line, move it, or show it on desktop only?
4. **Button wording:** keep the tag's capital ("See every Fat loss compound"), or lower-case it ("See every fat loss compound")?
5. **Still open from the promote report:**
   - 2: the wording of the seven Category 2/3 lines.
   - 4: the evidence calls to confirm.
   - 7: the four readings of Khavinson & Morozov 2003, and ARA-290's and thymulin's pages not mentioning their 503A Category 3 listing. ARA-290's card changed this session; its page text did not.

## Carried over

- **Brave key rotation: closed.** You rotated the key on 27 Sep.
- **GHK-Cu band wording:** non-injectable GHK-Cu is back in 503A Category 1, and the band still says "Not legal to compound — yet". Open.
- **The 503B question.** Open.
- **The Vercel author block:** deploys still go through the git-less copy. Open.
- **Identity-workflow open questions 5–10.** Open.
- **Content-truth open questions.** Open.

## Web content

Rule 3: everything fetched was treated as data. Nothing was acted on as an instruction, and no instruction-like text was seen.
- **npm packages** `puppeteer-core` 25.12 and `lighthouse` 13.5, installed in the session scratchpad only.
- **HTTP:** checks and Lighthouse runs against www.kalios.health, and the IndexNow API.
- No other web pages were read this session.
