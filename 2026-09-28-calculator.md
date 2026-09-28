# Calculator rebuild — 2026-09-28

All five parts of the CALCULATOR REBUILD brief are done. The rebuild is on a **preview deployment only**; production is untouched until the five-person test.

- **Preview:** `dpl_5GNJvnKdDqsh82T29irrwC8RriDM`, state READY, target preview.
  - Built from commit `beb12c98` through the git-less copy (CLAUDE.md rule 4).
  - No `--prod`, no IndexNow.
  - The preview's address isn't written here: `*.vercel.app` addresses carry the team handle (rule 1). It is in the session's closing message. It also shows under Deployments in the Vercel dashboard, and `vercel inspect dpl_5GNJvnKdDqsh82T29irrwC8RriDM` prints it.
- **Production is unchanged.**
  - www.kalios.health still serves the old calculator.
  - `/embed/calculator.html` and `/calculator-sw.js` return 404 there.
  - The newest production deployment is the one from before this session.
- **Commits (all pushed):**
  - `e280e63d` design inputs
  - `9d7e1600` Part 1 · `9ffcdc7b` Part 2 · `65aafb92` Part 3 · `beb12c98` Part 4
  - This report is the last.
- **Scale:** 57 files changed (+4,536 / −1,114): 44 added, 13 modified, none deleted. 14 of them are QA screenshots.

**Session start (rule 2)**
- Claude Code 2.1.284.
- Vercel CLI 60.1.3, the same as the latest on npm.
- `brew outdated`: 38 formulae (among them git, node, openssl@3, python@3.12). None were updated.
- The calculator uses no third-party library. Test tooling (puppeteer-core 25.12.0, Lighthouse 13.5.0) stayed in the session scratchpad.

---

## Look at these first

1. **Vial and water start filled in: 5 mg and 2 mL**, as in the reference and the artboard ("vial and water carry package-size examples"). The amount starts empty.
   - The risk: someone who doesn't change them gets the right answer for a 5 mg vial, which may not be theirs.
   - "You said" repeats all three numbers under the answer, and so do the slip and the label.
   - Placeholders instead would force every visitor to type all three (Open question 1).
2. **"1,000" and "5.000" are refused, not guessed.** A comma or point followed by exactly three digits, after a non-zero number, could be one thousand or one.
   - The page asks, with two buttons that fix the field: [1000] [1], or [5000] [5].
   - Every other comma is a decimal: 2,5, 0,125, 12,5.
   - That rule is the one place the math says "I can't tell" instead of computing (Open question 2).
3. **The input fields are dark, not paper.** The artboard and the reference draw them on paper, but spec §4 reserves paper for the slip, the search field and the primary button. CLAUDE.md says the spec wins (Open question 3).
4. **"Big dial" starts above 60 units.** The reference used 80; many pens stop at 60 (Open question 4).
5. **The brief's "20 wrong-input scenarios" weren't in the repo**, so I wrote the 20. They and their outcomes are below.
6. **Old links still work.** 113 compound and stack pages link to `calculator.html?c=<slug>`. Those land on the new calculator, which ignores `?c=` (no presets). The locked compound template's section 13 still says "Include calculator stack builder link", and stack mode is gone (Open question 5).

---

## What was done

**Part 1 — the math, frozen** (`9d7e1600`)
- **`assets/calc-math.js`:** pure functions with `CALC_MATH_VERSION = "1.0"`.
  - Integer arithmetic only: BigInt amounts in nanograms (IU in milli-IU), volumes in nanolitres. Nothing is a float until it is text.
  - **Vial mode** takes: vial amount and unit (mg, mcg, IU), water in mL, the amount you're drawing and its unit, the syringe scale (U-100 default, U-40) and its capacity (30/50/100, or 40 for U-40).
  - **Pen mode** takes: concentration in mg/mL, the amount (mg or mcg), units per click, pen volume.
  - **It returns:** units ×100, the volume, the concentration, draws per vial (doses per pen), the nearest readable mark (or click) and what it holds, the unit-family mismatch flag, the caution flags and round-number water tips.
- **The rules** are written once, in `tests/README.md`. Three implementations follow them, sharing nothing else:
  - the page's JS (BigInt)
  - `tests/calc-oracle.py` (Python `Fraction`)
  - `tests/gen-calc-cases.py` (Python `decimal`), which writes the expected values
- **`tests/calc-cases.json`:** 400 cases, vial 297 and pen 103. 149 of them also carry values worked out by hand, among them the reference implementation's own 28 checks. The generator refuses to write the file if a hand value disagrees.
- **`tests/run-calc-tests.sh`, the gate:**
  - `calc-math.js` matches its recorded hash (the freeze). A change needs a version bump, regenerated cases and `--record`, and `--record` refuses without a bump.
  - The case file and the page's selftest are exactly what the generator writes.
  - JS = oracle = expected = hand, on every case.
- **CLAUDE.md:** the gate is now the first step of the deploy checklist: no deploy of the calculator, the embed or `assets/calc*` while it fails.
- **The page runs 28 of the cases on load** (`assets/calc-selftest.js`, written by the generator). It prints "Math v1.0 · verified 400 cases" in the footer's fine print, or "do not ship" in red; after a failure it shows no numbers.

**Part 2 — the page** (`9ffcdc7b`)
- **The rebuild:** `calculator.html` on the Street Lab template, with `assets/calc.css` and `assets/calc.js`.
  - Title "Peptide Calculator — vial-to-syringe math | Kalios".
  - No compound picker, no dose presets, no stack mode, no FAQ dosing answers (the FAQ JSON-LD is gone too).
- **Two modes on one screen:** vial + syringe, and pen.
- **The answer comes first:** DRAW TO / DIAL TO N UNITS, the number in caution yellow, live on every keystroke, with the thump (and an 8 ms vibration on Android).
- **The syringe is the v11-Illustration drawing.**
  - Fluid, stopper and marker move together, and the marker rides the stopper.
  - Gradations redraw for each capacity; U-40 draws its own 0–40 scale.
  - Tick labels and strokes keep their size at any width.
- **Syringe choice:** three pictures ("30u · 0.3 mL · the small orange-cap one", "50u · 0.5 mL · the mid-size one", "100u · 1 mL · the full-size one") and a U-100 / U-40 switch. U-40 shows "40u · 1 mL · U-40, the red-cap one".
- **The caution band appears only when earned:** won't fit, more than the vial, too much water, too small to read, hairline on a 100u, units don't match, doesn't land on a click, big dial, plus more than the pen holds.
- **Data rows:** you said / concentration / draws per vial / nearest mark. In pen mode: you said / volume / doses per pen / nearest click.
- **The round-number water tip**, and the two honest paragraphs. In pen mode, also the artboard's brand-pen line.
- **Memory:** the last inputs are remembered on the device, and the page opens showing the last answer. The first visit focuses the amount field.

**Part 3 — the objects** (`65aafb92`)
- **(a) Share:** one primary button.
  - Where the browser can share files: `navigator.share` with the slip PNG, one line of text and the link.
  - Otherwise the PNG downloads and the link is copied.
  - The slip is drawn ahead of time, so the share happens inside the tap (Safari needs that).
- **(b) The slip:** 1080 × 1350 PNG, drawn on canvas:
  - "DID I DO THIS RIGHT?" in mono, then the Anton answer
  - the drawn syringe filled to the line with its scale (or the pen dialed)
  - the paper slip with the data rows, and a caution band when one is up
  - mark-solid top right, KALIOS.HEALTH/CALCULATOR, and "arithmetic only · never suggests an amount"
- **(c) Link:** every calculation lives in the URL hash, for example `#vial=5mg&water=2ml&amount=250mcg&syringe=100u` or `#pen=2.5mg-ml&amount=0.5mg&click=1u&volume=3ml`.
  - Opening the link restores it, and a changed hash in the same tab is followed.
  - A hand-edited bad link is ignored.
- **(d) Label, spec 7c:**
  - "Print a label" prints two at true size (2.6 × 1.2 in), top left of the sheet.
  - "Save label" downloads a 936 × 432 PNG.
  - The label CSS moved from `street-lab.css` to `calc.css`.
- **(e) Embed:** `/embed/calculator.html`, a compact version (syringe sizes as one row of four), with "Powered by Kalios — we don't sell peptides" linking home.
  - It sends nothing anywhere else: no cookies, no storage, no analytics, no service worker.
  - Its own Content-Security-Policy allows this site only.
  - Its fonts are served from `assets/fonts/`, with their OFL licences.
  - The calculator page has an "Embed this" panel with the one-line iframe snippet and a Copy button.
- **(f) Installable:**
  - `calculator.webmanifest` and `calculator-sw.js`: scope `/calculator`, network first, the page, assets and fonts from the cache when offline.
  - A one-time, dismissible "add to home screen" hint.
  - `scripts/mark.py` adds `favicon-192.png` and `favicon-maskable-512.png`.
- **Redirect:** `/calculator` → `/calculator.html` (`vercel.json`), because the slip prints that address.

**Part 4 — QA** (`beb12c98`): results under Checks.
- Fixes found here:
  - the tip no longer says "already lands on a line" under a hairline caution
  - the embed's size row stays four across
  - the structural audit allows the calculator's own stylesheet
- The scenario harness is saved as `tests/calc-scenarios.mjs`.

**Part 5 — preview deploy**
- In order: gate, prerender (0 of 456 pages changed), `git ls-files --deleted` empty, the git-less copy (`diff -rq` listed only `.git`, `.claude`, `reports`, `art/.DS_Store`), `vercel deploy` (preview), then `vercel curl` checks.

---

## The 20 wrong-input scenarios

Headless Chrome, a fresh profile for each. Every number the page shows was checked against the Python oracle on the page's own state.

All pass at 390 px and at 1280 px, and in the embed at 480 px.

| # | Input | Outcome |
|---|---|---|
| 1 | Amount left blank (first visit) | No number: "Type your amount." |
| 2 | Letters in the amount (`abc`) | No number: "Numbers only, like 2.5 or 2,5." |
| 3 | A minus sign (`-250`) | No number: "Numbers only" |
| 4 | `1,000` mcg | No number: "can be read two ways", buttons [1000] [1]. Tapping 1000 gives a plain 40 units |
| 5 | `5.000` mg on the vial | No number: buttons [5000] [5] |
| 6 | Too many decimals (`0.0001` mcg) | No number: "Up to 3 decimal places in mcg." |
| 7 | Two separators (`1,000.5`) | No number: "Numbers only" |
| 8 | Zero water | No number: "Check the water: it can't be zero." |
| 9 | Zero on the vial | No number: "Can't be zero." |
| 10 | IU vial, amount in mg | Caution "Units don't match", no number |
| 11 | 250 typed with mg instead of mcg | Caution "More than the vial holds", no number |
| 12 | 6 mg from a 5 mg vial | Caution "More than the vial holds", no number |
| 13 | 1000 mcg on a 30-unit syringe | Caution "Won't fit this syringe", 40 units (nearest mark: past the top) |
| 14 | 25 mL of water | Caution "That's a lot of water", 50 units |
| 15 | 5 mcg from 10 mg in 1 mL | Caution "Too small to measure", 0.05 units |
| 16 | 50 mcg on a 100-unit syringe | Caution "Hard to read on this syringe", 2 units |
| 17 | A U-40 syringe | Plain answer: 4 units, "0.1 mL on a U-40 syringe, where 40 units = 1 mL" |
| 18 | Pen: 0.33 mg at 2.5 mg/mL | Caution "Doesn't land on a click", 13.2 units (nearest click 13u = 0.325 mg) |
| 19 | Pen: 2 mg at 2.5 mg/mL | Caution "Big dial", 80 units |
| 20 | Pen: 10 mg from a 3 mL, 2.5 mg/mL pen | Caution "More than the pen holds", no number |

**Seven extra checks, all passing:**
- a comma decimal water (`2,5` → 12.5 units)
- a space inside a number
- a huge amount
- the artboard's two examples (10 units; pen 20 units)
- two answers that sit halfway between lines (9 units on a 1 mL; 10.5 on a 30-unit), where the page's own "halfway, 8–10" numbers were checked too

---

## DECISIONS

Conservative options, taken without asking, per the brief.

**Math**
1. **Lines and readable steps.** 1 mL U-100 syringes are drawn with a line every 2 units; 0.3 and 0.5 mL, and U-40, with a line every unit. The nearest readable mark is a line or the point halfway between two, which is the reference's 1-unit and half-unit rounding, extended to U-40. The data row says which: "on a line", or "halfway, 8–10".
2. **Rounding:** half up, once per shown number, from the exact value (never a rounded number rounded again).
3. **Ambiguous numbers are refused** (Look at these first, 2). A number with more decimals than its unit holds is refused, not rounded: mg 6, mcg 3, IU 3, mL 6, clicks 2.
4. **No thousands separators anywhere:** "2500 mcg/mL", not the artboard's "2,500". To a comma-decimal reader, 2,500 is two and a half.
5. **Thresholds:**
   - too much water: over 10 mL
   - too small: under 1 unit
   - hairline: 1 to under 5 units on the 100-unit syringe (the reference's three)
   - big dial: over 60 (Look at these first, 4)
   - "More than the pen holds" is new: the pen's version of "more than the vial".
6. **No number is shown when it can't be drawn or can't be computed:** more than the vial or pen holds, or units that don't match. The other cautions show the number with the band.
7. **One band at a time**, in this order: units, more than the vial, water, won't fit, too small, hairline. Pen: more than the pen, big dial, click.
8. **Exactly 400 cases**, so the footer reads "verified 400 cases". The count comes from the case file, and the gate checks the two agree.

**Page**
9. **Dark input fields** (Look at these first, 3).
10. **The H1 is the mono line** "Peptide calculator · vial-to-syringe math". The Anton answer is an `<output>` in the H1's place, so Anton appears in three places (answer, band title, wordmark), and search and the OG image get a stable heading.
11. **Vial and water start at 5 mg and 2 mL** (Look at these first, 1).
12. **The syringe pictures keep true proportions:** the three are about the same length and differ in barrel width. The nicknames for 50u and 100u are mine; the 30u one is the brief's.
13. **The caution band sits under the inputs**, so it never moves a field while you type. The answer box has a fixed height, so fonts loading or long numbers don't shift the page; long numbers shrink to fit.
14. **Changing the vial's unit into or out of IU switches the amount's unit to match**, since there is no mixed answer. Changing the amount's unit afterwards still gets "Units don't match".
15. **Umami on the calculator carries `data-exclude-hash="true"`.** The tracker includes the URL hash by default and treats every URL change as a page view. Without it, every calculation would reach analytics as a page URL.
16. **Every amount, volume, unit count, concentration and count shown comes from the math module.** The page adds only the drawing, the scale numbers, the two lines either side of a halfway mark, and dates. QA checks the halfway numbers against the oracle.
17. **The "Math v1.0 · verified 400 cases" line sits in the footer's fine print** (page content), not in the footer template, which is the same on every page.
18. **Desktop:** two columns, with the answer column staying in view as you scroll.

**Objects**
19. **The link carries numbers and units, never the label text.** The label text stays on the device.
20. **The slip shows the caution band when one is up.** A shared answer carries its warning.
21. **The label:** the mixed date is the print date (as in 11c), with "28 days →" as spec 7c asks. The pen label says "Pen" unless the user types their own text (up to 24 characters).
22. **The label's layout and file:** printing with an answer on screen prints the two labels whichever way the print starts. The PNG is 3× the artboard's 120 px/in, so 360 dpi.
23. **The embed ignores the URL hash**, so a host page can't preset an amount ("never suggests an amount"). It also has no autofocus, no vibration and no storage.
24. **The embed's fonts are self-hosted** (Anton, Inter, IBM Plex Mono, Latin subsets, 96 KB), so a visitor to a third-party site never reaches Google through it.
25. **Snippet height 1200 px.** The embed measures 1,000–1,190 px at 480 wide; a caution band on a narrow site scrolls inside the frame.
26. **The service worker is network first**, so a deploy shows at once; the cache answers after 4 s or when offline. It controls `/calculator` only, never the rest of the site.
27. **The install hint:** once, on touch screens, after the first answer, never in the installed app. With storage blocked there's no hint, because "once" couldn't be kept.
28. **`embed/` and `tests/` are skipped** by prerender, the OG images and the sitemap. `tests/` is in `.vercelignore`.

**Process**
29. **G's uncommitted design inputs** (spec §7 and the v11 Mark, Label and Logo artboards) were committed as found, unmodified, in their own commit (`e280e63d`). They are this build's sources.
30. **The preview's address is left out of this report** (rule 1 wins over the brief's "report the preview URL"). The deploy is cited by its ID.
31. **Lighthouse ran on a local static server with gzip**, as in earlier sessions. The preview sits behind Vercel login, and I didn't use the project's automation-bypass secret.

---

## Checks

**The gate (`bash tests/run-calc-tests.sh`): PASS.**
- frozen: v1.0 matches its hash
- the generator's output is current
- 400 of 400 cases: JS = oracle = expected (149 with hand values)
- selftest: 28 cases, total 400

**The gate was tested by breaking the math on purpose**, in a scratch copy with ten seeded bugs. All ten were caught:
- rounding by truncation
- the ambiguity rule off by one digit
- the hairline threshold
- tips keeping the current water
- `>=` for won't-fit
- big dial at 80
- no zero-trimming
- U-40 as 100 units/mL
- the water threshold
- doses rounded up

Also caught: a wrong expected value, a comment-only edit (the freeze), and `--record` without a bump.

**Coverage in the 400:**

| | Count |
|---|---|
| Vial statuses | ok 262, invalid 22, incomplete 5, zero 4, mismatch 2, error 2 |
| Pen statuses | ok 90, invalid 5, incomplete 3, zero 4, error 1 |
| Flags | over-capacity 70, over-vial 37, too-small 30, big-dial 30, off-click 28, too-much-water 15, hairline 13, over-pen 7, mismatch 2 |

**Lighthouse, mobile** (3 runs, local server with gzip)

| Page | Performance | Accessibility | Best practices | SEO | Metrics |
|---|---|---|---|---|---|
| Calculator | 100, 100, 100 | 100 | 100 | 100 | FCP 0.9–1.4 s · LCP 1.4 s · TBT 0 ms · CLS 0.001 |
| Embed (1 run) | 99 | 100 | 100 | 58 | — |

- The embed's SEO score is 58 on purpose: it is `noindex`. Its own security policy also blocked Lighthouse's robots.txt fetch, a test artefact.

**Behaviour (headless Chrome, local)**
- **Link:** the hash is written and normalized (`2,5` → `2.5`). A fresh visitor opening it gets the same answer and syringe, and not the label text. A changed hash in the same tab is followed; a bad one is ignored.
- **Share fallback:** the PNG downloads, the link is copied, and the toast says so.
- **Print:** both labels measure 2.6 × 1.2 in, the only thing on a Letter sheet.
- **Reload:** the last answer comes back (50-unit syringe, 12 units). With storage blocked the page still works.
- **Embed:** zero requests to any other origin, no storage, no cookies, no service worker, no console errors. It ignores a preset hash, and all four sizes stay visible.
- **Offline:** the service worker takes control on the second load. Offline, the page, the last answer and the fonts load from the cache, and so does `/calculator`.
- **Installable:** the manifest has no errors. The only installability error was "in-incognito" (the test profile).
- **Hint:** shows once; not on the next visit.
- **No JavaScript:** "— units" and a by-hand formula; the pen panel and labels stay hidden.
- **Old links:** `?c=bpc-157` computes normally, and the query drops off the address.
- **Keyboard:** a syringe size can be picked with Space.
- **Structural audit:** 456 pages checked against HEAD, 0 failed. Against the session's start, only the calculator differs, and only by the brief's removals: the FAQ and its links to four compound pages, the old header text, stack mode.

**The preview (`vercel curl`, 38 checks, all pass)**
- **Byte-identical to the commit, with the right content types:**
  - the service worker and the manifest (`application/manifest+json`)
  - every `assets/calc*` file and both site assets
  - the fonts (`font/woff2`) and their licence
  - both new icons, `og/calculator.png` and the sitemap
- **The four HTML pages:** identical except the Vercel toolbar script that previews add to every page. Production never gets it.
- **Redirect:** `/calculator` → 308 → `/calculator.html`.
- **404 on the preview:** `/tests/…`, `/reports/`, `/CLAUDE.md`, `/scripts/…`, `/kalios-design/…`, `/gsc-export/…`.
- **Page content:** Umami's `data-exclude-hash`, the manifest link, no FAQ JSON-LD and one H1 on the calculator; `noindex` and the self-only policy on the embed; the old label rules gone from `street-lab.css`.

**Screenshots** (`gsc-export/calculator-shots/`)
- At 390 and 1280: empty, answer, caution, pen.
- The slips (vial, caution, pen), the labels (vial, pen) and the printed sheet.

---

## Manual checks for G (phone)

The preview is behind Vercel's login. Open it in a phone browser that is signed in to Vercel, or make a shareable link in the dashboard (the deployment, then Share).

1. **Share, iPhone (Safari):** type an amount, tap "Share this math".
   - The share sheet should show the slip image, one line of text and the link.
   - Send it to yourself in Messages, then open the link: the same math, the same syringe.
2. **Share, Android (Chrome):** the same; the image should attach.
3. **Add to home screen:**
   - iPhone: Share, then Add to Home Screen.
   - Android: the hint's button, or the menu's Install app.
   - Open it from the icon: black, no browser bar.
   - On an iPhone the installed app has its own cookies, so on the preview Vercel's login may appear once inside it. That's the preview's protection, not the calculator.
4. **Offline:** after one visit, turn on airplane mode and open it (from the icon or the browser). The last answer should be there, and the math should still work.
5. **The thump:** on Android, a short buzz when the answer changes.
6. **Print a label** at 100% scale and measure it: 2.6 × 1.2 in, two to a sheet.
7. **The hint:** it should appear once, after the first answer, and not again.

On the preview only, the embed logs one console error: its policy blocks the Vercel toolbar script that previews add.

---

## Counts

- **Math:** 1 module, 3 implementations, 400 cases (149 hand-worked), 28 run on every page load.
- **Scenarios:** 20 wrong-input, plus 7 extra, at 390 px, 1280 px and in the embed.
- **Preview checks:** 38.
- **Pages:** 2 new or rebuilt (the calculator, the embed); 0 of the other 455 changed.
- **Sizes, gzipped:**
  - `calculator.html` 6.3 KB
  - `calc.js` 15.3 KB · `calc-math.js` 3.2 KB · `calc-selftest.js` 1.7 KB · `calc.css` 3.3 KB
  - the embed's HTML 3.5 KB and CSS 1.6 KB, plus 96 KB of fonts
  - the service worker 1.4 KB

## Files changed (main ones)

- **New:**
  - `assets/calc-math.js`, `assets/calc-selftest.js`, `assets/calc.js`, `assets/calc.css`, `assets/calc-embed.css`, `assets/fonts/` (5 woff2 + 3 OFL texts)
  - `embed/calculator.html`, `calculator.webmanifest`, `calculator-sw.js`, `favicon-192.png`, `favicon-maskable-512.png`
  - `tests/` (README, cases, generator, oracle, JS runner, gate, freeze hash, browser scenarios)
  - `gsc-export/calculator-shots/` (14)
- **Rebuilt:** `calculator.html`.
- **Changed:**
  - `assets/street-lab.css` (the 7c label rules moved out)
  - `scripts/mark.py` (two icons), `scripts/streetlab.py` and `scripts/gen_sitemap.py` (skip `embed/`, `tests/`), `scripts/street_lab_audit.py` (the calculator's stylesheet)
  - `vercel.json` (the redirect), `.vercelignore` (`tests/`)
  - `data/page-dates.json`, `og/calculator.png`, `data/og-manifest.json`
  - `CLAUDE.md` (the gate in the deploy checklist; a Calculator section; the label note)
- **Committed as found:** `kalios-design/street-lab-spec.md` §7 and the v11 Mark, Label and Logo artboards.

## URLs to eye-check (on the preview)

- `/calculator.html` on a phone:
  - first visit (empty, the amount field focused)
  - type 250: 10 units
  - then 50: the hairline band
  - pen mode with 0.33
  - U-40
- `/embed/calculator.html`, and the "Embed this" snippet on a test page.
- `/calculator` (the redirect), "Print a label" in print preview, "Save label".

## Open questions for G

1. **Prefilled 5 mg / 2 mL, or placeholders?** Placeholders are safer (nobody gets an answer for a vial that isn't theirs) and cost one more thing to type.
2. **The ambiguity rule.** Keep refusing "1,000" and "5.000", or treat the point as always decimal? The rule costs a US visitor who types "1.250 mg" one tap.
3. **Input fields: dark (the spec) or paper (the artboard and the reference)?**
4. **Big dial: over 60 units, or over 80 as the reference had?**
5. **Old links and the locked template.**
   - Section 13's "calculator stack builder link" has no target now; the stack tool at `/stacks/` is the closest.
   - Keep the 113 `calculator.html?c=<slug>` links (they work), or point them at `/calculator.html`?
6. **The 28-day line on the label.** Spec 7c asks for it, and it's kept. 11c used each compound's own stability note, but the calculator no longer knows the compound. Keep 28 days as a plain date marker?
7. **Embed height.** A fixed 1200 px, or a second, optional script line that sizes the frame to its content?
8. **The nicknames** "the mid-size one" (50u) and "the full-size one" (100u): keep, or your words?

## Carried over (unchanged)

- The Vercel author block: deploys still go through the git-less copy.
- Street Lab 11c open questions 2–6: GHK-Cu's band title, the OG mark, the "Aa" memory, the held reports, the alert's follow-ups.
- Earlier open items: the promote report's Q2, Q4 and Q7; the 503B question; identity-workflow 5–10; content truth.

## Web content

Rule 3: everything fetched was treated as data; nothing was acted on as an instruction. No instruction-like text was found.
- **The Umami tracker** (`cloud.umami.is/script.js`), read as code: this is how the default hash reporting was found.
- **Google Fonts:** the CSS for Anton, Inter and IBM Plex Mono, and their five Latin woff2 files, now self-hosted for the embed.
- **The Inter and IBM Plex Mono OFL licence texts** from the google/fonts repository.
- **HTTP:** headless-browser checks against a local server, `vercel curl` against the preview, and two `curl`s of www.kalios.health confirming production is unchanged.
