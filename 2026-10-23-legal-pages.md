# Legal pages and the deploy guard

**Session:** the legal pages (your brief: "One pass, smallest diff, no design changes beyond three new pages in The Fine Print"). **Status:** Parts 1–4 done; deployed once; checked on production.
**Deploy:** `dpl_ATvQwp5r5bHjUGakEasdjWwm8Jwh` (production, READY), one `vercel --prod` from a git-less copy.
**Snapshot:** taken after the deploy (commit `87815242`): 3 changes, the three new pages. None is a trigger, so the build wrote no alert draft.
**IndexNow:** the three new URLs and COA Check, HTTP 200.
**Commits:** Part 1 `05539fd7`; Part 2 `3a44ac9d`; Part 3 `3a1078fd`; Part 4, the checks, `d9aecc21`; the snapshot and the live checks' record `87815242`; then this report.

## Each Part in a line

1. **Three pages.** `/privacy.html`, `/terms.html` and `/disclaimer.html`: your text word for word (one change, "the FDA Tracker", D2), in About's shape, each with its title, description, share image, "Checked 23 Oct 2026" and a place in the sitemap. All five policy links answered 200 in the gate, so all five are linked.
2. **The footer and COA Check.** Every page's footer now ends "Corrections · How we check · Privacy · Terms · Disclaimer". COA Check has your sentence directly above its upload control, with corrections entry 1,339.
3. **The guard.** `.vercelignore` gains `.env` and `.env.*`. `scripts/predeploy_check.py` lists the deploy copy as the Vercel CLI would upload it and refuses the guarded paths. CLAUDE.md names it as the step before `vercel --prod`.
4. **Checks, deploy, this report** (Checks, below).

## Part 1: the three pages

- **Words.** A script held each page's H1, every heading and paragraph against the brief's text: all three word for word. The one change is "the FDA Tracker" (D2). Apostrophes are set as the site sets them (’). Which lead-ins became headings is in Left for you 7.
- **Shape.** About's: "← Home", the H1, the byline ("Checked 23 Oct 2026 · No vendors · No affiliates · No one buys a word of it"), the read, the generated header, tab bar and footer, in paper and ink.
- **Head.** Each page has a canonical address, a share image (`og/privacy.png`, `og/terms.png`, `og/disclaimer.png`, drawn by the build), and a title and description in the site's shape, with their og: and twitter: copies:
  - Privacy — What Kalios Collects: Almost Nothing | Kalios
  - Terms of Use — Reading, Linking and Quoting Kalios | Kalios
  - Disclaimer — Educational Only, Not Medical Advice | Kalios
- **The five links.** Each host's robots.txt was read first; all five allow the page and set no delay. The gate's link check then opened each:

| Word | Address | Answer |
|---|---|---|
| Umami | https://umami.is/privacy | 200 |
| Cloudflare’s | https://www.cloudflare.com/privacypolicy/ | 200 |
| Anthropic | https://www.anthropic.com/legal/commercial-terms | 200 |
| Vercel | https://vercel.com/legal/privacy-policy | 200, at `/legal/privacy-notice` after a redirect |
| Substack | https://substack.com/privacy | 200 |

  All five are linked (D4).
- **Sitemap and dates.** 203 to 206 URLs, each new page dated 2026-10-23. No other page's Checked date moved.
- **What changed.** /changes.html and the feed list the three as new pages. The homepage's what-changed and /alerts.html's last three leave them out (D3); both pages are byte-identical to before.

## Part 2: the footer and COA Check

- **The footer** (`templates/footer.html`) now ends "Corrections · How we check · Privacy · Terms · Disclaimer" on all 208 pages. The footer is chrome, so no Checked date moved.
- **COA Check.** "Cover or crop any names or order numbers before you upload." sits directly above the two upload buttons (`#coa-drop`), in the page's own privacy-note style. At 390 × 844 it ends at 397 px and the buttons at 533 px, of a 783 px first screen, in both themes.
- **Corrections entry 1,339:**
  - Wrong: "Above its upload control it said nothing about the names or order numbers printed on a report."
  - Now: "Directly above the upload control: “Cover or crop any names or order numbers before you upload.”"
  - The entry dates COA Check's Checked line to 23 Oct 2026, where fix 8 had already put it.
  - The corrections count reads 1,339 on the homepage, How we check and llms.txt.
- **Docs.** CLAUDE.md §17, docs/TEMPLATES.md and docs/DESIGN-SYSTEM.md name the footer's new links.

## Part 3: the deploy guard

- **`.vercelignore`** gains `.env` and `.env.*`.
- **`scripts/predeploy_check.py`** lists the copy as the Vercel CLI uploads it: every file, less the CLI's own defaults (as vercel 60.1.3 has them) and the copy's `.vercelignore`, matched as the CLI matches them (gitignore rules, case-insensitive). It exits 1 if any guarded path would ship:
  - `.claude/` or `.env*` at any depth;
  - `reports/`, `gsc-export/`, `fixtures/`, `scripts/`, `tests/`, `design/` or `templates/` at the root;
  - any data/ file but `data/search.json`.
- **Its self-test** plants each guarded kind in two scratch copies:
  - With no `.vercelignore`, all 14 are caught.
  - With the repo's, only a data file no line names and a `.envrc` are caught. Before the two new lines, `.env` and `.env.production` were caught too.
- **Proved on real uploads.** Fix 8's deployment holds exactly the guard's list less this session's six new files. This deployment holds exactly the 457 files the guard listed.
- **CLAUDE.md.** The checklist names the guard as the step before `vercel --prod`, and the known-blocker steps run it on the copy: 340 characters. With §17's 28, the session added 368 of the 500 allowed (CLAUDE.md is 74,323 characters).

**The guard's output on the deploy copy:**

```
predeploy check: <the git-less copy>
  .vercelignore: read; no .git folder
  would ship: 457 files; kept out: 94 paths (folders count once)
  by top-level entry: og/ 208, compounds/ 160, (root files) 34, stacks/ 22, assets/ 16, goals/ 10, coa/ 3, api/ 1, compare/ 1, data/ 1, embed/ 1
  data/search.json: ships; data/ files that would ship: 1
PASS: none of .claude/, .env*, reports/, gsc-export/, fixtures/, scripts/, tests/, design/, templates/ or a data/ file but search.json would ship
```

## Checks

| Check | Result |
|---|---|
| Calculator gate | PASS: 400/400, the 28-case self-test, QR 123 |
| Citation gate (`--links-new`) | AUDIT PASSED: 5,955 citations on 181 pages, 0 failing, 0 to review, 109 allowed. 4,469 references (4,160 linked, 309 text), 15,799 in-text citations anchored, 599 dated citations in the backlog, 0 stale. 4,396 links, 0 failing; the five new ones 200 |
| Card gate | PASSED: 157 cards, 1,466 drawn, 5 allowed; self-test 12 of 12, and 2 of 2 on the goals' parts |
| hero_meter | 151 one lines, the nine goal pages and /stacks/: 0 failing |
| Token lint | 0 findings |
| first_screen | 161 pages × 2 themes and the calculator: 0 misses (status line at most 762 px, doors 1,153). The three pages and COA Check × 2 themes: 0 misses (the legal pages' first two blocks end by 563 px) |
| search_check | 210 entries, 0 failing |
| COA tests | scorer 40, function 14 |
| Self-tests | refs 33 cases; citation walker 7/7; the guard's, PASS |
| Orphans | /404.html only, as before |
| Sweep, every page | 492 page-theme pairs over 239 lines (every deployable page in both themes and the Aa flips, fix 8’s states, 18 views at 1440): 0 wider, 0 under AA (lowest 4.52:1), 0 small targets, 0 stamps on two lines, 0 404s, 0 dead links. The first run died when its terminal closed; it was rerun in the foreground in six parts |
| Prerender | settles, before and after the snapshot |
| Sitemap | 206 URLs, written after the last prerender |
| Guard | PASS on the git-less copy (above); `diff -rq` listed only `.git`, `.claude` and `reports` |
| Deploy | `dpl_ATvQwp5r5bHjUGakEasdjWwm8Jwh`, READY, first attempt |
| Live | 77 checks, all pass: the three pages word for word, with their links, dates, canonicals and share images; every fetched page's footer; COA Check's sentence and entry; the sitemap, feed, changes page, homepage list and counts. `data/` and the internal files answer 404; `search.json` 200 |
| Live footers | 390 and 1440, both themes, on the three pages, the homepage, BPC-157 and COA Check: 24 of 24 |
| Live sweep | those six pages at 390 and 1440, both themes and the Aa flips: 26 pairs, 0 findings |
| IndexNow | the three new URLs and COA Check, HTTP 200 |
| Secrets and identity | `git ls-files --deleted` empty; gitleaks: no leaks; both identity checks print nothing; no trailers |

## Left for you

1. **The receipt sends the pair it shows, and Privacy says it sends nothing.** The page reads: "The calculator and the stack receipt. Everything you type stays in your browser and in the link it makes. Nothing is sent to us."
   - **The calculator:** true. Its math lives in the address's hash, which Umami never sees (`data-exclude-hash`), and `calc-run` carries only the compound page the visit came from.
   - **The receipt:** not true. Each receipt shown sends Umami `receipt-run` with the two compounds (`a`, `b`) or the stack. Its page view also records the address, pick included (`/stack.html?a=…&b=…`): stack.html's tag excludes no query string.
   - (1) Make the receipt match the page: `receipt-run` with no properties, and `data-exclude-search="true"` on its tag. The Monday read still counts it (rule 8 counts uses, not pairs).
   - (2) Make the page match the receipt: say it records which pair or stack was opened, never who opened it.
   - (3) Leave.
2. **Visits names what a page view records, not the events.** The tool events also reach Umami: `door`, `search`, `share`, `calc-run`, `coa-check`, `coa-sample`, `receipt-run`, `receipt-click` and `label-scan`. Each carries at most a door's or a page's name, or the receipt's pick.
   - (1) One sentence saying so.
   - (2) Leave: "We count visits" covers them.
3. **"We cite a source for every claim"** (Terms) and **"with the source beside every claim"** (Disclaimer). Two things still fall short:
   - the unsourced lines fix 8 listed (its Left for you 5, 9, 17, 20, 21 and 23);
   - 599 dated citations that match no entry (the backlog).
   - (1) Keep: it is the rule the pages are held to.
   - (2) Clear those lines and the backlog, so the sentence is true everywhere.
   - (3) Words of yours for what is true today.
4. **"We name no sellers and link to none"** (Terms; the Disclaimer: "No vendors are named or linked"). FLGR-242 still names its promoter 62 times, links his site in its References and links two of his X posts (fix 8's Left for you 3). Klotho names him once. Whether a seller's promoter counts as a seller is your call.
   - (1) Give FLGR-242 Klotho's treatment.
   - (2) Leave.
5. **No AI training, says the Terms; robots.txt invites the training crawlers.** robots.txt names GPTBot, ClaudeBot, anthropic-ai and Google-Extended as allowed. Those are the user agents and the control their makers document for model training. The Terms, like the footer since fix 5, say the text and data may not train AI models without written permission.
   - (1) Disallow the training agents and keep the ones that fetch a page for an assistant's answer.
   - (2) Keep, and let the Terms carry it.
6. **COA Check's own privacy note:** "We only send page images to run the check, and nothing is stored." Two places on the site say otherwise:
   - Privacy: Anthropic keeps the images "only for the limited period its API terms set";
   - COA Check's how-it-works line: "within 30 days".
   "Nothing is stored" reads as no one stores them. The 30 days predates this session, and Anthropic's terms, now linked from Privacy, are where to recheck it.
   - (1) "…and Kalios stores nothing", as the page's meta description says.
   - (2) Leave.
7. **The headings.** The brief's bold didn't survive the paste, so I read the lead-ins as below. Each heading keeps its period, as you wrote it.
   - **Privacy:** eleven headings, one per paragraph, starting "What we collect: almost nothing."
   - **Terms:** six. Its opening paragraph ("Kalios is a free reference. …") has no lead-in, so it stays a paragraph.
   - **Disclaimer:** one, its opening line, over three plain paragraphs.
   - (1) Keep.
   - (2) Name the ones that were bold, for example the Disclaimer's "Kalios describes; it does not advise." or Terms' "Kalios is a free reference.".
   - (3) Drop the headings' periods.
8. **The new pages in the curated lists.** The homepage's what-changed and /alerts.html's last three leave the three pages out (D3); /changes.html and the feed list them.
   - (1) Keep.
   - (2) Let them show, like any new page. They would push out the warning letters, the OMB review and AOD-9604's line.
9. **Vercel's address redirects** to `/legal/privacy-notice`, which answered 200.
   - (1) Keep the address you gave.
   - (2) Link where it lands.
10. **Where else the pages could be linked.** Only the footer links them. Not llms.txt's Key Pages, How we check's analytics note, COA Check's privacy note or the search index.
    - (1) Leave.
    - (2) Add them where you choose.
11. **The newsletter.** Privacy describes it, but no page links it; only the homepage's JSON-LD names it, in `sameAs`.
    - (1) Leave.
    - (2) A link, on About or Privacy.
12. **first_screen's default set** (the checklist step) holds the compound pages, the goal pages, /compounds/ and the calculator. The legal pages and COA Check are held only when named.
    - (1) Add them to the default set.
    - (2) Leave.
13. **The guard's list is the brief's.** Some internal files are held only by `.vercelignore` and the sweep's network proof: `taxonomy.json`, `gloss-library.json`, `stacks/*.json`, `art/`'s sources and `hero-figures.png`. The guard also mirrors the default ignores of vercel 60.1.3, and 65.2.0 is out.
    - (1) Add those files to the guard, and re-read the CLI's list when it is updated.
    - (2) Leave.
14. **The shell's Python.** This tool's shell put python.org's 3.12 first, with no Pillow and no CA bundle; prerender and the link check need both. Every Python step ran on `/opt/anaconda3/bin/python3` (3.9.13), as earlier sessions evidently did through conda's base environment. The link check's first probe failed on certificates for all five links before any HTTP request was sent; it was rerun.
    - (1) Name the interpreter in the checklist.
    - (2) Give 3.12 its certificates and Pillow.
    - (3) Leave.
15. **pdf.js stays at 6.3.289.** Its preview gate still can't run (fix 8's Left for you 15).

Fix 8's 24 items are untouched; they were not this session's work.

## DECISIONS

- **D1. The headings** are the lead-ins as read in Left for you 7, each with its period. The words are otherwise yours: a script held each H1, heading and paragraph against the brief's text, with apostrophes set as the site sets them (’).
- **D2. One change to your words:** "the FDA tracker" reads "the FDA Tracker", the door's one name (rule 3 of the scroll). No page's own text has the lower-case form left.
- **D3. The curated lists keep their news.** Nothing outside the brief was to move, so the homepage's what-changed and /alerts.html's last three leave the legal pages out (`streetlab.LEGAL_PAGES`, `legal_page_added`). /changes.html and the feed keep the full record, as for every page.
- **D4. The links:** each of the five sits at the name's first mention ("Cloudflare’s Turnstile"; "our model provider, Anthropic"). They take `rel="external noopener"` with no new tab, like the References' links. The email is a mailto link, as on About and How we check. No other link was added.
- **D5. The shape is About's:** "← Home", the H1, the byline, the read, the header and footer; og:type article. The titles and descriptions use the pages' own words.
- **D6. COA Check's sentence** goes above the two buttons (`#coa-drop`), in the page's own privacy-note style; no stylesheet changed. Its entry's "wrong" was written from the base commit.
- **D7. The guard** mirrors the CLI's own walk: its defaults plus `.vercelignore`, gitignore rules, case-insensitive. It checks `.claude/` and `.env*` at any depth, the seven folders at the root, and data/ for anything but search.json. `.vercelignore` gains exactly your two lines, so a `.envrc` would be refused by the guard rather than ignored.
- **D8. first_screen** holds the four pages when they are named; its default set is unchanged.

## Counts

- **Pages:** 208 decorated (3 new), 206 in the sitemap.
- **Corrections logged:** 1,339, one this session.
- **Citations:** 5,955 on 181 pages, 0 failing. **Links:** 4,396, 0 failing.
- **Status changes:** 3, the new pages.
- **Deployed files:** 457.
- **CLAUDE.md:** 74,323 characters, 368 added.

## Files changed

From `9410ee3a`: 4 Part commits, the snapshot commit, then this report.
- **New pages:** `privacy.html`, `terms.html`, `disclaimer.html`, with `og/privacy.png`, `og/terms.png` and `og/disclaimer.png`.
- **Every page's footer:** 159 in `compounds/`, 21 pair pages, 10 goal pages and the root pages.
- **COA Check:** `coa/index.html`.
- **System:** `templates/footer.html`; `scripts/streetlab.py` (`LEGAL_PAGES`, `legal_page_added`); `scripts/first_screen.py`; `scripts/predeploy_check.py` (new); `.vercelignore`.
- **Data:** `changelog.json`, `changes.json`, `counts.json`, `link-check.json`, `og-manifest.json`, `page-dates.json`, `status-snapshot.json`.
- **Generated:** `changes.html`, `corrections.html`, `how-we-check.html`, `index.html` (counts), `feed.xml`, `llms.txt`, `sitemap.xml`.
- **Docs:** `CLAUDE.md`, `docs/TEMPLATES.md`, `docs/DESIGN-SYSTEM.md`.
- **Record:** `gsc-export/legal-pages-2026-10-23/`: the gate logs, the five hosts' robots.txt, the sweep's list and logs, the guard's list and output, the live checks and the production footer shots.

## URLs to eye-check

- https://www.kalios.health/privacy.html
- https://www.kalios.health/terms.html
- https://www.kalios.health/disclaimer.html
- **The sentence above the buttons:** https://www.kalios.health/coa/
- **The footer, at the end of any page:** https://www.kalios.health/ and https://www.kalios.health/compounds/bpc-157.html
- **Entry 1,339:** https://www.kalios.health/corrections.html
- **The three new pages listed:** https://www.kalios.health/changes.html

## Session start and web content

- **Versions:**
  - Claude Code 2.1.309.
  - Vercel CLI 60.1.3 installed, 65.2.0 published; the deploy ran on 60.1.3.
  - Homebrew: 40 formulae outdated (git, gh, node, go, python@3.12, openssl@3 and ca-certificates among them).
  - pdf.js: COA Check runs 6.3.289 (Left for you 15).
- **Interruption.** The session's terminal closed during the first sweep. Nothing had been deployed. The local server was stopped and restarted, and the sweep was rerun in the foreground before the deploy.
- **Web content.** Everything fetched was read as data. Nothing in it read as an instruction or was followed as one. What was fetched:
  - the five hosts' robots.txt, kept in the record;
  - the five policy pages, opened by the link check for their status only;
  - kalios.health, for the live checks;
  - Vercel's API, for the deployment's file list;
  - IndexNow.
- **Refusals:** none.
