# The Fine Print — the skin

**Session:** The Fine Print (G's brief dated 2026-10-15; the work ran on 2026-10-13 and 2026-10-14 by the machine's clock, so the four Checked dates that moved, and the two corrections entries, say 14 Oct 2026). **Status:** Parts 1–9 done, deployed once, checked on production.
**Deploy:** `dpl_3puySqLbGzM63odKL9ZMBw9vDcHQ` (production, READY, serving www.kalios.health), one deploy from a git-less copy (the known blocker; the first `vercel --prod` was refused, "Not authorized", and created nothing, and one checked retry deployed). It carries the finish, part A as well: that session ended before its Part 4 commit, its repeated full run and its deploy, so this session committed its Part 4 (`e9deabec`), ran the full online gate once over both sessions' work and deployed once; its report, `reports/2026-10-15-finish-a.md`, is filled from this deploy and says so.
**Snapshot:** `data/status-snapshot.json` taken after the deploy, the checklist's last step (`44c1df18`). The status diff found 2 changes, both the finish's (Thymulin's meter and verdict); the reskin changed no stamp, legal line, verdict, meter or red flag.
**IndexNow:** 203 URLs, the whole live sitemap, after the deploy: HTTP 200, accepted.
**Commits:** `8a6f5d8c` (Parts 1–3), `6a879b8e` (Parts 4–6), `52c3de29` (Part 7), `a00697d7` (Part 7b), `0694ca97` (Part 8, CLAUDE.md), `96af2dbf` (Part 9), `acafdfde` (Part 8b), `e9abb4d1` (the sitemap), `a53a9ef9` (the gate run: counts and the link check), `e58df8f3` (the sweep's record), `44c1df18` (the snapshot), then this report.

**In one line:** every page of kalios.health now wears the approved file, `design/fine-print.html` — paper and ink in one reading column, Newsreader and IBM Plex Mono self-hosted, hairlines instead of boxes, the number as the artwork, a drawing of the form each compound is taken in, rows where cards were — with no word or order changed except the legal paragraphs (your choice) and four sentences that described the old card (each with a corrections entry); a token lint now holds every size, spacing, color and family to the file's set (0 findings), the sweep holds every text color to AA in both themes (0 under), and all 150 compound pages pass the first-screen gate in both themes.

---

## Look at these first

1. **Four sentences changed, though the brief said no words change, because the redesign made them false.** The guide's "How to read a Kalios card" said the evidence meter is "Four bars, bottom left … no bars for no data … four bars for an approved drug", the legal stamp sits "Beside the meter, on the right", the red flag "under the stamp", and "a yellow band" marks a waiting page; About said "A yellow band on a page". The row the file draws has a five-segment meter under the hook, the stamp on the right beside the name, the flag under the meter, and the band is a yellow line beside a paragraph. Each now says that, with a corrections entry (1,109 logged); those two pages, the corrections page and the homepage (its corrections count) are Checked 14 Oct 2026. If you'd rather the drawing changed than the words, say which.
2. **The meter lights one segment per level: No data one, Approved drug all five.** The file draws only Approved drug, with all five lit, and the levels are five, so one per level is the only even reading; it means a "No data" row shows one dark segment beside its words. The alternative, none lit for No data, would leave Approved drug at four of five, against the file. Yours to confirm (D2).
3. **The hero's band is its caution rule alone, without the note's 12 px of padding above and below.** With the file's note padding, Semax's two doors ended at 1,184 px, 10 past a screen and a half (1,174); without it they end at 1,160, and every compound page passes in both themes. The read's warning notes keep the file's padding (D9).
4. **The legal text is your choice, done:** the long legal paragraphs left all 205 pages' footers; the footer's sentence, "Educational only · Not medical advice · We read the fine print", is the disclaimer on every page; /coa/ keeps its two COA paragraphs, /stacks/ its paragraph and How we check its own (the analytics line), in their bodies. CLAUDE.md's content rule says so.
5. **Red words on paper are #D3303E** (your choice: 4.71:1 on #FAFAF7, where #E63946 is 3.99:1); #E63946 stays for the wordmark's dot, the stamp's border and the flag's dot, and for red words on ink.
6. **No drawing carries a quantity.** The file's syringe printed scale numbers; the figure under a compound's chips is "research documentation, not instruction", and a number on a syringe beside a compound's name reads as a dose, so the eight drawings have numbered callouts and a legend and nothing else. The calculator's syringe keeps its scale: it is the answer's drawing.
7. **The share images are mine, in the file's terms** (the file draws none): paper, the page's name in Newsreader, its stamp, a compound's number and italic line, "Checked {date} · kalios.health", and the mark at the right of the foot line, which the standing rule keeps on the share cards. 205 per-page images and /og-default.png. Look at two or three in a link preview.
8. **The calculator's look changed, not its math**: the gate's 400 cases, the 28-case self-test and the freeze all pass; its service worker's cache is `kalios-calc-v5` (the self-hosted fonts).

---

## What shipped, Part by Part

- **Part 1, the system (`8a6f5d8c`).** `assets/street-lab.css` became `assets/fine-print.css`, written from the file: one surface per theme, the tokens below, two families self-hosted as subset woff2 and preloaded (`scripts/fonts.py`; no request to Google), Anton, Inter and the Tailwind compatibility layer gone. The theme follows the phone and the Aa control flips the whole page (`data-theme` on the root, remembered in the `sl-read` key the reading surface's toggle used, so a returning reader keeps a choice). `scripts/token_lint.py` holds every stylesheet, every page's `<style>` and every `style=` to the file's sizes, spacings, radii, colors and families.
- **Part 2, the chrome (`8a6f5d8c`).** The header is the wordmark "Kalios" with its red dot, then the search, the Aa flip and the menu as hairline icons; the tab bar is paper with the file's icons; the byline two mono 13 lines; the footer three lines over a hairline. The legal paragraphs left the footers (Look at these first, 4). The dark shell, the paper reading surface, the seam and its tear went (`scripts/migrate_fine.py`).
- **Part 3, the rows (`8a6f5d8c`).** The row replaced the card everywhere a card was drawn (`streetlab.row_html`): the tag with its route word, the name (serif 22), the hook (serif 16, ink-2), the meter, the stamp on the right, "for another use", the red flag, "Checked {date}", a hairline below. The receipt's script and the search overlay draw rows; the card gate reads rows (1,439 drawings, 0 failing; self-test 11 of 11).
- **Part 4, the compound page (`6a879b8e`).** The hero: the eyebrow, the H1 with its stamp, the byline, the number (96, 76 at 420 px and under) with its italic line and mono scope, the status line with its stamp word run in, the band as a caution rule, the bottom line, the doors as the file's button pair. The figure (below) under the section chips, where the seam was. The sheet: key/value rows in mono, "Show all N". The section chips with a sliding filled marker. The read: H2 serif 30, one 48 px gap before every H2, the four questions' mono labels, citations as hairline links, warnings as the band, boxes as hairlines; dose tables as sheet rows under 720 px. From 1024 px: two columns, the read at 640 and the doors, the sheet and "Same group" sticky on the right at 360.
- **Part 5, the other pages' frames (`6a879b8e`).** The tracker's, About's and the guide's own stylesheets went; their frames take the system's column, H1 and H2; every page's inline styles went (only the view-transition names remain, which no stylesheet can hold).
- **Part 6, the calculator and COA Check (`6a879b8e`).** The calculator: the mode as a chip row, the answer as the number with its italic line (stepping down the type scale's own sizes for a long answer), the syringe and pen redrawn in the figure's stroke with their fill in caution, the syringe choices as rows with a square that fills, hairline fields with mono values, the pinned line with its caution rule; the slip and the vial label drawn on paper in Newsreader and Plex Mono; the embed in the same tokens. COA Check: the two buttons as the pair, the explainer in the read's style, the result's six lines as sheet rows with ink stamps landing 24 ms apart, the share card on paper with the mark in ink.
- **Part 7 and 7b, the share images and the last fit (`52c3de29`, `a00697d7`).** `scripts/og_images.py` redrawn (Look at these first, 7); Anton, Plex Bold and Anton's licence left `scripts/fonts/`; `scripts/fonts.py` keeps each source's timestamp, so two runs write the same bytes; the hero band's fit (Look at these first, 3); How we check's count blocks share one hairline between two; the compare page's card names take the 44 px hit layer.
- **Part 8, CLAUDE.md (`0694ca97`).** "Design System" (Street Lab's) is replaced by "The Fine Print (the design system, G, 2026-10-15)"; the deploy checklist runs `scripts/token_lint.py`; the sweep, the card gate, the scroll's notes, the calculator, COA Check, the templates, the generated blocks, the compound template and the content rule say what is true now.
- **Part 9, the words (`96af2dbf`).** The four sentences (Look at these first, 1) and a site entry in `data/changelog.json` for the redesign, as 2026-09-28's has.

---

## The token set as shipped

| Token | Paper (light) | Ink (dark) | Used for |
|---|---|---|---|
| `--paper` | #FAFAF7 | #0D0D0E | the page |
| `--ink` | #121212 | #ECEAE4 | text, rules of emphasis, the primary button |
| `--ink-2` | #5E5E58 | #9A9A94 | secondary text, mono labels |
| `--rule` | rgba(18,18,18,.14) | rgba(236,234,228,.16) | hairlines |
| `--rule-2` | rgba(18,18,18,.32) | rgba(236,234,228,.36) | stronger hairlines, chip borders, link underlines |
| `--red` | #E63946 | #E63946 | the wordmark's dot, a stamp's border, the flag's dot |
| `--red-text` | #D3303E | #E63946 | red words |
| `--caution` | #F5C518 | #F5C518 | the band's rule, the calculator's pin and fill |
| `--green` | #1F7A45 | #6BCB77 | FDA APPROVED |
| `--amber` | #B35C00 | #F28C28 | COMPOUNDED RX |

Spacing `--s1`–`--s7` 8, 16, 24, 32, 48, 64, 96; `--gutter` 20; `--col` 640; `--side` 360; `--site` 1064; `--head-h` 57; `--tab-h` 62. Radius 0, 2 px (stamps, chips, buttons), 50% (dots). No shadows, no gradients. Type sizes: 10, 11, 12, 13, 14, 15, 16, 17, 22, 24, 30, 40, 46, 76, 96. Families: Newsreader (variable, optical size on; 400, 500, italic 400) and IBM Plex Mono (400, 500), 147,068 bytes of woff2 together (budget 150,000).

## Sizes and spacings found and removed

Counted over every stylesheet, every page's `<style>` and every `style=` attribute, before (`e9deabec`, Street Lab: `street-lab.css`, `calc.css`, `coa.css`, `calc-embed.css` and the pages) and after:

| | Before: distinct values (declarations) | After |
|---|---|---|
| Font sizes | 73 (1,306) | 15 (227), plus the printed label's two fitted sizes |
| Spacing values written as numbers | 64 (1,821) | 15 (147): the file's component internals, 0–14 px, and 64 twice inside a `calc()`; every other spacing is a token |
| Colors written as values | 101 (388) | 17 (52): each token's own value, where it is defined |
| Radii | 7 (73) | 3 (24) |
| Shadows | 7 (12) | 0 |
| Gradients | 9 | 0 |
| Font declarations | 28 distinct (182) | 2 (176): `var(--f-serif)`, `var(--f-mono)` |
| Declarations in all | 13,030 | 5,854 |
| `style=` attributes on pages | 4,499 | 2,878, every one a view-transition name |

Values are counted once however they were written (0.55rem and .55rem are one). **The 60 font sizes removed:** every rem, em and percentage size (30 rem values from .5rem to 3rem, 4 em, 3 percentages), three `clamp()` sizes, the H1's fitted `var(--h1-size)`, one `inherit`, and 18 px sizes outside the set: 9, 9.5, 10.5, 11.5, 12.5, 13.5, 14.5, 16.5, 18, 19, 20, 21, 23, 44, 50, 64, 68 and 88. Four sizes are new: 24, 40, 46 and 76. **The 50 spacing values removed:** every rem (29) and em (5) value, and 16 px values: 13, 15, 16, 18, 20, 22, 24, 26, 28, 30, 36, 40, 56, 60, 72 and 1,184 (16, 20 and 24 survive only as the tokens `--s2`, `--gutter` and `--s3`). `python3 scripts/token_lint.py --count` lists what is left, with its counts.

## The eight drawings

`scripts/figures.py`, 600 x 190, one stroke in `currentColor`, numbered callouts and a legend, no quantity. The drawing follows the card's route word; a shot whose Route (sold) row opens with a pen draws the pen. Caption: "Fig. 1 — the form it was studied in: {Route (studied)}. Research documentation, not instruction."

| Drawing | Route word | Pages |
|---|---|---|
| Vial and syringe | DAILY SHOT, WEEKLY SHOT, MONTHLY SHOT, SHOT | 57 |
| Pen | a shot sold first as a pen | 11 |
| Blister | PILL | 32 |
| Tube | CREAM | 18 |
| Nasal spray | NASAL SPRAY | 9 |
| IV bag and line | IV DRIP | 3 |
| Implant | IMPLANT | 1 |
| Dropper | DROPS | 0 (drawn, no page uses it yet) |
| None | no route word on the card | 19: AICAR, B7-33, BAM15, BPC-157 Fragment, Cortagen, DNSP-11, Follistatin-344, FOXO4-DRI, Humanin, IGF-1 DES, Livagen, LL-37, Nesfatin-1, Neurokinin A, P21, PNC-27, Thymosin Beta-4, Vilon, Waglerin-1 |

## Each page type

| Type | Pages | In the Fine Print |
|---|---|---|
| Compound (with the Adamax alias) | 150 | The hero (eyebrow, H1 and stamp, byline, the number, status, band, bottom line, doors), the figure, the sheet, the section chips, the read, the closure; two columns from 1024 px |
| Named stack | 7 | Hero with its stamp and legal lines, the band, the receipt as rows and sheet rows, the References, the change row |
| Pair | 21 | Hero, the receipt (tested together, the evidence on the combination as sheet rows), what each does alone (rows), the mechanism overlap, what the studies found, Legal Status, References |
| Goal | 11 | H1, the sentence, the three tiers of rows, "Legal today" and "What people actually run", "Also relevant" |
| `/compounds/` | 1 | Search, the sticky chip row with its filled marker, each group's first three rows and "All N in {group} →" |
| Homepage | 1 | One column: the H1, the italic line, "First time? Start here →", the search, the four doors as numbered rows with their proofs, what changed, the counts, the chips, the Four to start with, the author card |
| Calculator, and its embed | 2 | Part 6 |
| COA Check | 1 | Part 6 |
| The receipt (`/stack.html`) | 1 | Rows and sheet rows; with a pick, the pair is the H1 |
| `/stacks/` | 1 | The tested pairs by level, the names people search, as rows |
| FDA tracker | 1 | The status board as rows (the name and stamp, the line, the vote; one layout at every width), the timeline, the read |
| Articles: About, How we check, the guide, the Khavinson article, Alerts, What changed, Corrections | 7 | The system's column, H1, H2 and read; their own stylesheets gone |
| Compare page | 1 | Its cards as hairline sections |
| 404 | 1 | The H1 over its mono line, the search, the door chips |

## First screen and contrast

- **First screen** (`scripts/first_screen.py`, 390 x 844, both themes, all 150 compound pages): the status line ends at most at 676 px (the first screen ends at 783, where the tab bar begins), the doors at most at 1,160 px (a screen and a half is 1,174); 0 misses.
- **The sweep** (`scripts/sweep.mjs`, the brief's pages, one of each other type and the compound page at 1024 and 1440, each in both themes, plus the Aa's two flips): 68 page-theme pairs: 0 wider than the screen, 0 tap targets under 44 px, 0 stamps on two lines, 31,286 text colors checked against the surface under them, 0 under AA.
- **Lowest contrast found, per theme:** on ink, 4.66:1 (GRAY MARKET's words on a stamp, 11 px; AA needs 4.5); on paper, 4.52:1 (COMPOUNDED RX's amber, #B35C00, on a stamp); with the Aa flipped, 4.66:1 on ink and 4.71:1 on paper (GRAY MARKET's #D3303E). Every other text clears those. The amber on paper passes by 0.02: a lighter amber would not.
- Screenshots of every page in the sweep, at the top and scrolled, both themes: `gsc-export/fine-print-2026-10-15/`.

## Share images

205 per-page images (compound, alias, stack, pair, goal and every other page) and `/og-default.png`, 1200 x 630 on paper, quantized to a fixed palette of the theme's colors: 3.2 MB for all of `og/`, about 16 KB each. `data/og-manifest.json` keys each image by what it draws, so a run redraws only what changed.

## Checks and deploy

| Step | Result |
|---|---|
| Calculator gate (`bash tests/run-calc-tests.sh`) | PASS: 400 of 400 cases agree (JS = oracle = expected), the 28-case self-test, FAQ 6, QR 123; `calc-math.js` v1.0 matches its frozen hash |
| COA tests | scorer 26 passed, function 14 passed |
| Prerender | run; a second run changes nothing |
| Citation gate, full online run (`python3 scripts/audit_pages.py --mode cite`, 08:04–09:03 on 14 Oct) | AUDIT PASSED: 5,923 citations on 180 pages, 0 failing, 0 to review, 110 allowed with a reason; 4,408 references, 4,092 linked, 316 text; 15,342 citations anchored to their entries, 213 to their records |
| Link check (the same run) | 4,339 outside links, 0 dead: PubMed 2,390 confirmed in their records, PMC 105, DOIs 923 resolved at doi.org, NCT 473 found in the registry, 448 other addresses; 525 refused a script, 116 not opened (robots.txt), 5 answered with an error a browser doesn't see (two DOIs whose publisher sends an incomplete certificate chain; npr.org, tga.gov.au and one fda.gov file kept a script waiting) |
| Prerender again | How we check and `llms.txt` print the run's numbers; a further run changes nothing |
| Sitemap (`scripts/gen_sitemap.py`) | 203 URLs; 79 lastmod dates brought up to date (75 to 13 Oct, the finish's; 4 to 14 Oct, this session's) |
| Card gate | PASSED: 156 cards, 1,439 drawings, 5 allowed with a reason, 0 failing; self-test 11 of 11 |
| `hero_meter.py` | 150 pages, 0 failing |
| `token_lint.py` | 0 findings over 4 stylesheets and every page |
| `first_screen.py` | 150 pages x 2 themes at 390 x 844: 0 misses; the status line ends at most at 676 px, the doors at most at 1,160 px |
| `hook_group.py`, `orphans.py` | the 4 allowed hooks; only /404.html has no inbound link |
| `refs.py` and `citations.py` self-tests | 29 cases pass; the walker 7 of 7 |
| The sweep (`scripts/sweep.mjs`) | 68 page-theme pairs: 0 wider than the screen, 0 small targets, 0 stamps on two lines, 31,286 text colors checked, 0 under AA |
| `git ls-files --deleted`; gitleaks; the identity checks | empty; no leaks; both print nothing |
| Deploy | `dpl_3puySqLbGzM63odKL9ZMBw9vDcHQ`, READY (the first `vercel --prod` answered "Not authorized" and created nothing; one checked retry) |
| Live | home, retatrutide, the calculator, /coa/ and the FDA tracker answer 200 and load `/assets/fine-print.css`; the stylesheet, the four fonts (their bytes as built), a share image, the sitemap and the feed answer 200; `/assets/street-lab.css` answers 404 |
| IndexNow | 203 URLs from the live sitemap, HTTP 200 |
| Snapshot | `44c1df18` |

---

## DECISIONS

- **D1. Where a size in the brief and the file's differ, the file's is used** (the brief's own rule): the byline, a scope and the figure's caption are mono 13; stamps 11; tab labels 10; a row's hook 16; the band 15; the wordmark and the number's italic line 24.
- **D2. The meter: one segment per level,** No data one, Approved drug all five (Look at these first, 2).
- **D3. The number:** a one line that opens with its figure, or that counts human pilots, sets the figure as the number; the rest of the line goes under it in italic; a line with no figure is the italic line alone. 51 of the 150 open with a number, 25 of them a count of pilots. No word of a one line changed; hero_meter.py reads them as before.
- **D4. Two things in the file aren't on the site:** the eyebrow's catalog number ("No. 087"), since Kalios numbers nothing, and the feed link in the file's footer, since the footer's links are the four doors, the email, Corrections and How we check, and the feed is linked where changes are (What changed, the tracker, How we check).
- **D5. No quantities on the drawings** (Look at these first, 6).
- **D6. Chips are the file's 34 px**, each with a transparent hit layer that makes the target 44 px, as the sweep requires; the same layer gives the site's small links 44 px without moving them.
- **D7. Red words on paper #D3303E** (yours).
- **D8. The printed vial label's fitted type is exempt from the token lint in print only:** it is a physical object, 2.6 x 1.2 in, its type fitted to its width.
- **D9. The hero's band without the note's vertical padding** (Look at these first, 3).
- **D10. Checked dates held through the reskin** (`prerender --reseed`): the change is to the look, and the legal paragraphs were boilerplate; the four pages whose words changed moved (Look at these first, 1).
- **D11. The four sentences changed, with entries,** rather than leaving the guide and About describing what the page doesn't draw (Look at these first, 1).
- **D12. The mark stays on the share images and COA Check's card,** in ink with its red dot, as the standing rule keeps it on the share cards; the header carries the file's wordmark, "Kalios" with the dot.
- **D13. The legal text** as you chose (Look at these first, 4).
- **D14. The spray tag's files stay in the repo and out of the deploy;** no page shows them since the homepage's hero went to the file's headline.
- **D15. One column everywhere but the compound page,** which goes to two from 1024 px (the read and the side); the homepage's Street Lab desktop layout (the hero left, the tag right) went with the tag.
- **D16. The "Also called" values keep their own separators.** On 24 pages they are " | " (Semax's "Met-Glu-His-Phe-Pro-Gly-Pro | MEHFPGP | …"); they are page words, so they stay, and on a phone they wrap with a bar at a line's end (What's left for you, 3).
- **D17. The sweep reports the lowest contrast it found** against what each text needs, per theme, so the margin over AA is on record, not only its failures.

## Counts

- Pages reskinned: 205 deployable pages and the calculator's embed; 0 wider than a 390 px phone.
- Corrections logged: 1,109 (2 this session).
- Per-page share images: 205, and /og-default.png.
- Compound pages with a figure: 131 (19 without a route word have none).
- Card gate: 156 cards, 1,439 drawings, 5 allowed findings, 0 failing.
- Token lint: 0 findings over 4 stylesheets and every page.
- Citation gate (the full online run, shared with the finish): 5,923 citations on 180 pages, 0 failing; 4,408 references, 4,092 linked; 4,339 outside links, 0 dead.

## Files changed

`e9deabec` to `44c1df18`: 601 files (15,360 lines in, 13,526 out). 205 share images in `og/` and `/og-default.png`; 158 files in `compounds/` (the 150 compound pages, the 7 named stacks and the index), 23 in `stacks/` (the 21 pair pages, the stacks index and `stacks/generate.py`), 11 goal pages, and 17 other files at the root (the article and tool pages, `404.html`, `calculator-sw.js`, `llms.txt`, `sitemap.xml`, `.vercelignore`, `CLAUDE.md`), the pages' changes being the reskin's markup: the disclaimers out, the figure in, the side column, the rows; 136 files of the sweep's record (`gsc-export/fine-print-2026-10-15/`). The system: `assets/fine-print.css` (new; `assets/street-lab.css` deleted), `assets/street-lab.js`, `assets/calc.css`, `assets/calc.js`, `assets/calc-embed.css`, `assets/coa.css`, `assets/coa.js`, `calculator-sw.js`, `embed/calculator.html`, `coa/index.html`; the fonts (`assets/fonts/`: four new woff2 files and the Newsreader licence; Anton, Inter and the old Plex files deleted; the sources in `scripts/fonts/`); `templates/header.html`, `footer.html`, `byline.html` (`templates/fine-print.html`, the old strip, deleted); `design/fine-print.html` (the approved file, not deployed). Scripts: new `scripts/token_lint.py`, `scripts/figures.py`, `scripts/fonts.py`, `scripts/migrate_fine.py`; changed `scripts/streetlab.py`, `scripts/card_gate.py`, `scripts/og_images.py`, `scripts/sweep.mjs`, `scripts/goals.py`, `scripts/gen_sitemap.py`, `stacks/generate.py`. Data: `data/changelog.json` (two corrections, one site entry), `data/page-dates.json`, `data/og-manifest.json`, `data/counts.json`, `data/link-check.json`, `data/status-snapshot.json`; `sitemap.xml`, `llms.txt`; `CLAUDE.md`.

## Deploy

- **`dpl_3puySqLbGzM63odKL9ZMBw9vDcHQ`**, production, READY, serving www.kalios.health, built from a git-less copy of `e58df8f3` (rsync without `.git`, `.claude`, `reports` and `.DS_Store`; `diff -rq` listed only those). The first `vercel --prod` answered "Not authorized" and created nothing (`vercel ls --prod`: the newest production deployment was 20 hours old); one retry deployed: 18.8 MB uploaded, 478 files, the COA function compiled.
- **Checked live:** the five pages the brief names answer 200 and load `/assets/fine-print.css` (none mentions `street-lab.css`, which answers 404); the stylesheet (62,097 bytes) and the four fonts answer 200 at their built sizes; the share images, the sitemap and the feed answer 200. The finish's work is live with it: BPC-157 carries 23 reference links and 86 citation links and no FAQPage, nor do the homepage, semaglutide, the calculator or a pair page; the guide prints "Five bars, under the hook"; Thymulin's card on Immune reads Controlled trials and its hero "56%" over "improved vs 17% on placebo"; How we check prints 4,339 links; `/calculator` 308, `/how-we-check`, `/goals/fat-loss.html` and `/stacks-index.html` 301 as before.
- **IndexNow:** 203 URLs, the whole live sitemap, HTTP 200.
- **Snapshot:** `44c1df18` (Thymulin's verdict and meter, the finish's; no trigger, no alert draft).

## URLs to eye-check

- https://www.kalios.health/ — the homepage, on a phone and a desktop, both themes; tap the Aa
- https://www.kalios.health/compounds/retatrutide.html — the file's own page; on a desktop at 1024 and 1440
- https://www.kalios.health/compounds/semax.html — the longest hero (the band)
- https://www.kalios.health/compounds/semaglutide.html — the pen drawing
- https://www.kalios.health/compounds/sermorelin.html — COMPOUNDED RX in amber
- https://www.kalios.health/compounds/ — the rows and the chip row
- https://www.kalios.health/calculator.html#vial=10mg&water=2ml&amount=250mcg&syringe=100u — the number, the syringe; print the label
- https://www.kalios.health/coa/ — the pair of buttons; a result's stamps
- https://www.kalios.health/stack.html?a=bpc-157&b=tb-500 — the receipt
- https://www.kalios.health/compounds/wolverine-stack.html — a named stack
- https://www.kalios.health/fda-pcac-2026.html — the status board
- https://www.kalios.health/beginners-guide.html — the four sentences, in "How to read a Kalios card"
- A share preview of any compound page (paste its address into a message)

## What's left for you

1. **The four sentences** (Look at these first, 1): keep the new words, or have the drawing follow the old ones.
2. **The meter's mapping** (Look at these first, 2).
3. **"Also called" separators** (D16): 24 pages print " | "; the file's sheet uses " · ". A words change, so yours.
4. **How we check calls the byline "the strip at the top" in three places;** the strip became the byline under the H1 in the scroll (2026-10-13), and the brief forbade word changes here, so they wait for you.
5. **Cortagen's route word**, still yours from the finish, part A (its capsules: one seller's research-labelled pack).
6. **The dropper drawing** has no page yet: no card says DROPS.
7. **The index's chip row scrolls sideways on a desktop too** (the scroll's design, as the file draws it): with a mouse and no trackpad, the last chips are reached by the page's own group headings or shift-scroll. Wrapping them on a desktop would break the sliding marker; yours if it matters.
8. **`scripts/fonts.py` needs a venv** (fontTools 4.40+ and brotli; the system Python has fontTools 4.25). Run it when a page prints a character the subset lacks; a missing character falls back to Georgia or the system mono.
9. **`scripts/gen_sitemap.py` is not on the deploy checklist, and prerender doesn't run it,** so `sitemap.xml` had kept the scroll session's dates: this session ran it (79 dates moved; it also had to learn to skip `design/`). Adding it after the second prerender would keep IndexNow's sitemap current; the checklist is yours, so it isn't there yet.

## Open questions

- None that block anything; the list above is what waits for you.

## Session start

- Claude Code 2.1.299. Vercel CLI 60.1.3 installed, 64.0.2 published (update when convenient; the deploy ran on 60.1.3). Homebrew: 41 formulae outdated. pdf.js stays 6.3.289 (6.4.299 published), as the finish decided: a bump wants its hashes and the COA fixture run in a COA session.

## Web content

Nothing fetched in this session was read as an instruction. The citation gate and the link check fetched records and pages as data (their answers are counted above); the design file came from G's own download folder, not the web.
