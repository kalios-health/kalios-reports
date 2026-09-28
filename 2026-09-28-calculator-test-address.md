# Calculator: finish + test address — 2026-09-28

All eight parts of the CALCULATOR — FINISH + TEST ADDRESS brief are done. The new calculator is in production at its test address; the old one still serves `/calculator.html`, unchanged.

- **Production:** `dpl_DtMqNhYfq1TVnSBiBYwesNZGaksW`, READY, target production.
  - Built from commit `77ba4b4b` through the git-less copy (CLAUDE.md rule 4).
  - It replaced `dpl_6YmK46sNvzNvG31yUc932crrYogx` (the FLGR-242 deploy).
  - No IndexNow, as the brief says.
- **Test address:** https://www.kalios.health/calc-next.html
  - `noindex`, not in the sitemap, linked from no page.
  - Also new in production: `/embed/calculator.html` and `/calculator-sw.js`.
- **`/calculator.html` is unchanged:** byte-identical before and after the deploy (curl, both times). It is still the old calculator.
- **Commits (all pushed):**
  - `d6a2c800` Part 1 · `59680cfd` Part 2 · `ebe7de03` Part 3 · `a5b34935` Part 4
  - `f7bd5491` Part 5 · `3c56af73` Part 6 · `77ba4b4b` Part 7
  - This report is the last.
- **Scale:** 142 files changed (+1,775 / −449): 5 added, 137 modified. 113 of them are compound and stack pages with a link target changed, and 10 are QA screenshots.

**Session start (rule 2)**
- Claude Code 2.1.284.
- Vercel CLI 60.1.3, the same as the latest on npm.
- `brew outdated`: 39 formulae. None were updated.
- Test tooling (puppeteer-core 25.12.0) stayed in the session scratchpad.

---

## Look at these first

1. **In the repo, `calculator.html` is the old calculator again. The new one is `calc-next.html`.**
   - The old page was restored from `81d2c7ca`, the commit production was built from, and it is byte-identical to what production served.
   - So any later deploy from the repo keeps the old calculator public until you say "promote". The steps are in CLAUDE.md (Calculator, "Promotion").
   - The old page needed two things the rebuild had changed, and both are back:
     - its print-label rules in `street-lab.css` (the 11c `.sl-vial-label` block);
     - its share image, `og/calculator.png`.
   - Without them, deploying would have broken the old page's printed label and changed its share card. `street-lab.css`, `og/calculator.png` and the sitemap are byte-identical to what production served.
2. **The 126 re-pointed links open the old calculator without its preset.**
   - The old calculator read `?c=<slug>` to preselect the compound. Your brief pointed the links at `/calculator.html`, and they are live now.
   - So until promotion, a visitor from a compound page lands on the old calculator with nothing picked.
   - The primary action ("Reconstituting this? Do the math.") already worked this way.
3. **Installing from the test address makes a separate app.**
   - The test page has its own manifest (`calc-next.webmanifest`: id, start and scope `/calc-next.html`) and its own service-worker scope (`/calc-next.html`).
   - "Add to Home Screen" there opens the new calculator, offline too, and never touches `/calculator.html`.
   - Testers who install it keep a second "Calculator" icon after promotion. A 308 from `/calc-next.html` to `/calculator.html` at promotion would carry them over (Open question 1).
4. **`/calculator` now redirects (308) to `/calculator.html`.** It returned 404 before.
   - The rebuild committed this redirect because the slip prints that address. This deploy made it live.
   - Until promotion it lands on the old calculator. The slips the test page makes print `KALIOS.HEALTH/CALCULATOR` too.
5. **116 compound pages still say "→ Check compound compatibility in the Stack Builder"** in section 13, linking to `/calculator.html`.
   - That's true of the old calculator, which is live, and false of the new one, which has no stack mode.
   - I left the text alone. Rewording it is in the promotion steps (Open question 2).
6. **79 of the 126 re-pointed links promise things the calculator doesn't do.** 47 read "→ Peptide Calculator — vial-to-syringe math".
   - The other 79 are in 59 wordings, among them "weekly SubQ scheduling", "enclomiphene cycle planning", "topical formulations" and "oral liquid doses".
   - Only the `href` changed (Open question 3).
7. **Analytics noise, small.**
   - My first local test runs loaded the page's Umami tag from `127.0.0.1` and `localhost`, and the tracker has no localhost exclusion.
   - If those hostnames show up in Umami, filter them out.
   - The scenario harness now blocks Umami, and every check against production blocked it, so this session added no production pageviews.

---

## What was done

**Part 1 — placeholders** (`d6a2c800`)
- **Vial mode:** every field starts empty, with "e.g. 5", "e.g. 2" and "your amount" as placeholders.
- **Pen mode:** "e.g. 2.5", "your amount", "e.g. 1", "e.g. 3".
- The embed is the same.
- **A first visit** focuses the first field (the vial) and reads "Fill in what's on the vial, the water and your amount." The page's static text says the same, so nothing swaps on load.
- **Unchanged:** a returning visitor still gets their last inputs back, and a shared link still fills every field.
- **The scenario harness** types the examples in before each scenario. It adds two first-visit checks (vial and pen): every field empty, the right placeholders, focus on the first field.

**Part 2 — input fields on paper** (`59680cfd`)
- **The calculator's fields,** on the page and in the embed, are `--paper` with `--paper-ink` text, as the v11-Calc and v11-Pen artboards draw them:
  - units and the unit menus at paper ink .7 (7.2:1), as the artboards have them;
  - placeholders at .62 (5.4:1);
  - the yellow focus ring on the black;
  - a tight yellow ring around a field to fix.
- **Forced colors:** a transparent border shows the field's edge there (checked with Chrome's forced-colors emulation).
- **`kalios-design/street-lab-spec.md` §4:** "Paper (`--paper`) is reserved for: the slip, input fields, the primary button. Nothing else.", with a dated note. The `--paper` row in §2's token table says the same.

**Part 3 — the ambiguity refusal stays** (`ebe7de03`)
- No code change. "1,000" and "5.000" are still refused, with a button for each reading.
- CLAUDE.md and the math spec in `tests/README.md` record your call, so it isn't relaxed later.

**Part 4 — the embed's auto-size line** (`a5b34935`)
- **The embed** posts its height to the page around it (`{kaliosCalcHeight}`, from a ResizeObserver), only when it is framed. Only the height crosses the frame, never an input or an answer.
- **"Embed this"** now shows an optional second line under the iframe line, with its own Copy button:

  ```
  <script>addEventListener("message",function(e){var h=e.data&&e.data.kaliosCalcHeight;if(e.origin!=="https://www.kalios.health"||!(h>=200&&h<=6000))return;[].forEach.call(document.getElementsByTagName("iframe"),function(f){if(f.contentWindow===e.source)f.style.height=Math.ceil(h)+"px"})})</script>
  ```

  - It loads nothing.
  - It accepts the message only from `https://www.kalios.health`, and only for the iframe that sent it.
  - Without it, the frame stays 1200 px tall.

**Part 5 — the links and section 13** (`f7bd5491`)
- **Links:** every `/calculator.html?c=<slug>` is now `/calculator.html`.
  - 126 links on 113 compound and stack pages: 101 pages with one, 11 with two, and `glow-stack` with three. None are left.
  - Only the `href` changed. Page text, content hashes and "Checked" dates are untouched, and prerender changes nothing.
  - Sampled first on `bpc-157`, `mk-677` and `glow-stack`, then run on the rest (rule 6).
- **The structural audit** (`scripts/street_lab_audit.py`) now accepts a lost `?c=` link only when the page's own text gained a plain calculator link for each one.
  - A planted loss still fails.
  - My first version counted links inside the generated blocks and let the planted loss through; the version in the commit doesn't.
- **CLAUDE.md:**
  - The locked template's section 13 now says "Include the calculator link (`/calculator.html`)", and its quality line says "stacking section with the calculator link".
  - The Calculator section says links to it carry no query.

**Part 6 — the gate** (`3c56af73`)
- `bash tests/run-calc-tests.sh`: PASS.
- The screenshots in `gsc-export/calculator-shots/` now show the paper fields and the empty first visit. Two are new: the "1,000" prompt on paper, and the "Embed this" panel.

**Part 7 — test address and production deploy** (`77ba4b4b`, then `dpl_DtMqNhYfq1TVnSBiBYwesNZGaksW`)
- **`calc-next.html`** is the new calculator exactly as it will ship, except:
  - a short comment and `<meta name="robots" content="noindex">`;
  - the manifest link (`/calc-next.webmanifest`);
  - its own OG image (`og/calc-next.png`, the same picture as the rebuilt page's).
- **`calculator.html`, `street-lab.css`'s 11c block, `og/calculator.png`:** restored (Look at these first, 1).
- **The service worker** keeps the page its scope names: `/calculator` keeps `/calculator.html`, and `/calc-next.html` keeps itself and its manifest. The page registers it for its own path.
- **Tools:** prerender and the audit know `calc-next.html`. The scenario harness defaults to it and blocks analytics.
- **The checklist, in order:**
  - the gate (PASS);
  - prerender (1 page and 1 OG image, then 0 on a second run);
  - the sitemap (unchanged, `calc-next.html` excluded as `noindex`);
  - `git ls-files --deleted` (empty);
  - gitleaks and both identity checks (clean), then push;
  - the git-less copy (`diff -rq` listed only `.git`, `.claude`, `reports` and `art/.DS_Store`);
  - `vercel --prod`, once;
  - the live checks below.
  - No IndexNow.

---

## DECISIONS

Conservative options, taken without asking, per the brief.

**Parts 1–4**
1. **The embed gets the placeholders too.** It is the same calculator, and it promises never to suggest an amount.
2. **The pen's placeholders are the old prefills as examples:** "e.g. 2.5", "your amount", "e.g. 1", "e.g. 3".
3. **A first visit focuses the first empty field, now the vial.** Before, the amount was the only empty field.
4. **"On paper" was read as colour, per the brief's parenthetical.**
   - The fields keep their sizes: 50 px tall, 18 px values, 14 px units. The artboards draw 48 px and 12 px.
   - The "Label for the vial" field is paper too.
   - The "Embed this" code boxes stay dark: they aren't input fields.
5. **§4 now allows paper for input fields; it doesn't require it.** Only the calculator's fields changed.
   - The alert form on research-only pages (on the dark ground) and the stack tool's pickers are as they were (Open question 4).
6. **The auto-size line is inline, not a script served from kalios.health.**
   - A host page runs it as written and loads nothing; there's nothing on our side for them to trust later.
   - It accepts heights from 200 to 6000 px.
   - Browsers without ResizeObserver keep the fixed 1200 px frame.

**Part 5**
7. **The links' anchor text is unchanged.** It's page content; the brief asked for the target (Look at these first, 6).
8. **The template's quality line changed too.** It restated section 13's "stack builder link".
9. **The 116 section-13 "Stack Builder" links weren't reworded.** Their text is true of the calculator that's live (Look at these first, 5).

**Part 7**
10. **The repo mirrors production.** The new calculator lives at `calc-next.html` and the old one at `calculator.html`.
    - The other way was to swap the files only in the deploy copy. Then the next plain deploy from the repo would have promoted the new calculator without anyone deciding to.
11. **`calc-next.html` keeps its canonical, `og:url` and JSON-LD naming `/calculator.html`,** as the embed already does (`noindex`, canonical to the calculator). The test page is the page that will ship.
12. **Nothing in `robots.txt` and no `X-Robots-Tag`.** A `Disallow` would stop crawlers from seeing the `noindex`, and it would advertise the path. The meta tag also keeps the page out of the sitemap, because `scripts/gen_sitemap.py` skips `noindex` pages.
13. **The test address can be installed and used offline,** because the brief deploys the service worker.
    - It needed its own manifest and its own worker scope.
    - Without them, "Add to Home Screen" would have installed the old calculator, and the worker would have taken control of `/calculator.html` for testers.
    - The worker now works out its page from its scope. That change stays after promotion.
14. **`calculator.webmanifest` is deployed but unlinked** until promotion.
15. **The `/calculator` redirect went live with this deploy,** as committed in the rebuild (Look at these first, 4).
16. **Deploy through the git-less copy.** The Vercel author block (rule 4) wasn't retested.
17. **Tests send no analytics:** the harness blocks `*umami.is*`, and so did every check against production.

---

## Checks

**The gate (`bash tests/run-calc-tests.sh`): PASS**, run after every part and again before the deploy.
- frozen: v1.0 matches its hash
- the generator's output is current
- 400 of 400 cases: JS = oracle = expected (149 with hand values)
- selftest: 28 cases, total 400

**Browser scenarios (`tests/calc-scenarios.mjs`): 29 of 29, every run.** That's the 20 wrong-input scenarios, 7 extra checks and the 2 new first-visit checks.
- Locally at 390 and 1280 px and in the embed at 480 px, after each part.
- On production: `/calc-next.html` at 390 px and `/embed/calculator.html` at 480 px.

**Embed auto-size, end to end.** The embed sat in a host page on another origin, which carried the two lines as the page shows them.
- Tested locally, and against the production embed with the exact snippet.
- Both Copy buttons copy exactly the text shown.
- 390 px: the frame matched the calculator, 1027 px with nothing scrolling inside. It grew to 1205 px with a caution band and shrank to 1071 px when the band went.
- 1280 px: 1010 → 1171 → 1038 px.
- A message from another origin is ignored: the frame stays 1200 px.
- Without the second line, the frame stays 1200 px.

**Service worker, manifest and offline (production)**
- The worker registers with scope `/calc-next.html`. It caches the test page and its manifest, never `/calculator.html`.
- The second load is controlled.
- The share link stays on the test address: `/calc-next.html#vial=5mg&water=2ml&amount=250mcg&syringe=100u`.
- The manifest loads with no errors, start and scope `/calc-next.html`. Chrome listed no installability errors.
- Offline, the page loads with the last answer (10 units) and "Math v1.0 · verified 400 cases", and the math still works.
- `/calculator.html` isn't controlled, and the test page's scope is the only registration.
- No console errors, apart from the blocked analytics.

**Live `curl` checks: 37 of 37**
- **Byte-identical to the repo:**
  - `calc-next.html`, `calc-next.webmanifest`, `embed/calculator.html`, `calculator-sw.js`, `calculator.webmanifest`
  - every `assets/calc*` file, the five fonts and a licence text
  - `favicon-192.png`, `favicon-maskable-512.png`, `og/calc-next.png`
  - three of the re-pointed compound pages
- **Content types:** the service worker is `application/javascript`, the manifests `application/manifest+json`, the fonts `font/woff2`.
- **Byte-identical to before the deploy:** `/calculator.html`, `street-lab.css`, `street-lab.js`, `og/calculator.png`, `sitemap.xml`, `robots.txt`, `/` and `llms.txt`.
- **The three compound pages:** the only live change is the `?c=` links.
- `/calculator` → 308 → `/calculator.html`.
- `calc-next.html` carries `noindex`, isn't in the sitemap, and no other page names it.
  - That last check failed on its first run. Its exclusion pattern expected `./calc-next.html` while grep printed `calc-next.html`, so it flagged the test page itself. Listing the files showed that page is the only one naming it.
- `www.kalios.health` serves `dpl_DtMqNhYfq1TVnSBiBYwesNZGaksW`.

**Structural audit**
- 456 of the 457 pages pass against HEAD.
- The one exception is `calculator.html`, now the old page, and it passes against `81d2c7ca`.
- `calc-next.html` passes against the committed new calculator.

**Also:**
- **Forced-colors emulation:** the fields keep a visible edge and the focus ring.
- **`gitleaks`:** no leaks.
- **Identity checks:** clean.
- **No `Co-Authored-By` or other trailers** (rule 7).

---

## Manual checks for G (phone), on https://www.kalios.health/calc-next.html

Unlike the preview, this address needs no Vercel login.

1. **First visit:** every field empty, with its example in grey on paper. Type 5, 2, 250: DRAW TO 10 UNITS.
2. **"1,000" in the amount:** the two buttons appear under the field.
3. **Share, iPhone and Android:** the slip image, one line of text, and a link that opens the same math at `/calc-next.html`.
4. **Add to Home Screen:** the icon opens `/calc-next.html`, black, with no browser bar. Then airplane mode: the last answer is there, and the math still works.
5. **Print a label at 100%:** 2.6 × 1.2 in, two to a sheet.
6. **"Embed this":** both lines, and both Copy buttons.

---

## Counts

- **Links re-pointed:** 126 on 113 pages. 0 left.
- **Live pages changed:** 113 (link target only). New: `calc-next.html` and the embed. All other pages unchanged, `/calculator.html` included.
- **Files in production:** 21 new, 114 modified (the 113 pages and `vercel.json`).
- **Tests:**
  - gate: 400 cases, plus the 28-case selftest
  - scenarios: 29, at 3 widths locally and 2 on production
  - auto-size: 8 checks, locally and on production
  - service worker: 9, locally and on production
  - live curl: 37
- **Commits:** 7, plus this report.

## Files changed (main ones)

- **New:** `calc-next.html`, `calc-next.webmanifest`, `og/calc-next.png`, `gsc-export/calculator-shots/calculator-390-fix.jpg`, `gsc-export/calculator-shots/embed-panel-390.png`.
- **The new calculator:** `assets/calc.js` (focus, the height post, the worker's scope), `assets/calc.css` (paper fields, the second snippet), `embed/calculator.html`, `calculator-sw.js`.
- **Restored as production had them:** `calculator.html`, `assets/street-lab.css`, `og/calculator.png`, and those pages' entries in `data/page-dates.json` and `data/og-manifest.json`.
- **Links:** 113 files in `compounds/`.
- **Design:** `kalios-design/street-lab-spec.md` (§2 row, §4).
- **Tools and docs:**
  - `scripts/streetlab.py`, `scripts/street_lab_audit.py`
  - `tests/calc-scenarios.mjs`, `tests/README.md`, `tests/run-calc-tests.sh`
  - `CLAUDE.md`: where each calculator lives, promotion, placeholders, paper, the ambiguity rule, the embed line, links, section 13
- **Screenshots:** 8 refreshed in `gsc-export/calculator-shots/`.

## URLs to eye-check

- https://www.kalios.health/calc-next.html, on a phone and on desktop:
  - the empty first visit
  - 5 / 2 / 250
  - 50 (the hairline band)
  - "1,000"
  - pen mode
  - U-40
- https://www.kalios.health/embed/calculator.html
- https://www.kalios.health/calculator.html: the old calculator, as before.
- https://www.kalios.health/compounds/bpc-157.html: the Reconstitution link now opens `/calculator.html` with nothing preset.
- https://www.kalios.health/calculator: redirects to `/calculator.html`.

## Open questions for G

1. **Promotion.** When the five-person test is done, say "promote"; the steps are in CLAUDE.md. Add a 308 from `/calc-next.html` to `/calculator.html` then, for testers' links and installs?
2. **Section 13's "→ Check compound compatibility in the Stack Builder"** (116 pages). At promotion, reword it to the calculator ("→ Peptide calculator — vial-to-syringe math"), or point it at the stack tool (`/stacks/`) with words to match?
3. **The re-pointed links' wording.** One plain text for all 126 at promotion, "→ Peptide Calculator — vial-to-syringe math", as 47 already say? The other 79 promise scheduling, cycle planning, formulations and the like.
4. **Other input fields on paper?** The amended §4 allows it. The alert form on research-only pages and the stack tool's pickers are still dark.
5. **Still open from the rebuild report:**
   - big dial at 60 or 80 units (its Q4)
   - the label's 28-day line (Q6)
   - the syringe nicknames "the mid-size one" and "the full-size one" (Q8)

## Carried over (unchanged)

- The Vercel author block: deploys still go through the git-less copy.
- Street Lab 11c open questions 2–6: GHK-Cu's band title, the OG mark, the "Aa" memory, the held reports, the alert's follow-ups.
- Earlier open items: the promote report's Q2, Q4 and Q7; the 503B question; identity-workflow 5–10; content truth.

## Web content

Rule 3: everything fetched was treated as data; nothing was acted on as an instruction. No instruction-like text was found.
- **The Street Lab design canvas** (your claude.ai Design artifact): its file list, and `project/v11-Calc.dc.html` and `project/v11-Pen.dc.html`. Both are byte-identical to the repo's copies, so "per the artboard" means those two.
- **The Umami tracker** (`cloud.umami.is/script.js`), read as code, to see whether it skips localhost. It doesn't.
- **HTTP:**
  - `curl` of www.kalios.health before and after the deploy;
  - headless Chrome against production, with analytics blocked;
  - headless Chrome against local static servers.
- **npm:** puppeteer-core, installed in the session scratchpad only.
