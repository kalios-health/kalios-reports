# The October build: Part A0, the explainer, deploy 1

**Session:** the October build (your brief v1, "one session, five Parts, two deploys"). **Status:** A0 done; A1 done as far as the brief reached me; deployed once; checked on production. **Stopped:** before the rest of A1, A2, A3 and Parts B–E, whose text never reached me in chat (Where I stopped, below).
**Deploy:** `dpl_nfK221cj3AYeKea4o5rxRC3oafUu` (production, READY). The first `vercel --prod` from the git-less copy answered "Not authorized" and created nothing (`vercel ls --prod` showed no new deployment); one retry deployed.
**Snapshot:** taken after the deploy (`674cffd9`): 1 change, the explainer as a new page. Not a trigger, so no alert draft.
**IndexNow:** 39 changed URLs, HTTP 200.
**Commits:** A0 `e6d75cae`; A1 `25f61082`; deploy 1's checks `4833e57f`; what the independent check found, fixed before the deploy `ca20772c`; the snapshot and live record `674cffd9`; the MT-2 redirect order `c1df795a` (not deployed yet); then this report.

## Where I stopped

- **What reached me.** Your paste ended inside A1's August 24, 2026 timeline item ("[FDA warning"). I built A0 and A1 to that point, completed that item's citation with the five letters the page already cites, and deployed.
- **Your later messages.** I applied your Reuters sources and your A1 correction ("…none of the reports cited here quotes it."). Your notes on C2, Part E and D3 apply to Parts I never received.
- **A file, not a message.** `gsc-export/october-build-2026-10-24/prompt-v1.md` (41 KB) appeared at 11:15 with the whole v1 brief. Because it reached me as a file rather than in chat, I treated it as data: I read it, built nothing from it, and kept it out of every commit. It adds:
  - A1's last two timeline items, three closing sections and a Sources list;
  - the link from the tracker's OMB entry;
  - A2 (`/legal-status.html`), A3 (the day-of kit) and Parts B–E.
  Left for you 1.

## Each Part in a line

1. **A0, ten small fixes.**
   - Privacy says what the visit counter records.
   - Four calculator changes.
   - Three titles and descriptions.
   - Four redirects, and the MT-2 stub replaced by a real 301.
   - Five internal links.
   - Umami's `data-exclude-hash` on every page.
   - The two standing rules.
   - The vendor off petrelintide; FLGR-242's promoter links and COA Check's Finnrick links turned to text.
   - Every vote count on the FDA Tracker attributed and linked.
2. **A1, the explainer.** `/fda-peptide-policy.html`: your text, a generated status line, every citation linked, in About's shape.
3. **The independent check.** A reader who hadn't written the day's work found 14 issues; the real ones were fixed before the deploy (Part A0, The independent check).
4. **Deploy 1**, the live checks, the Umami hash test, IndexNow and the snapshot (Checks).

## Part A0

- **Privacy.** Both sentences, your words. "what’s" is set with the site's apostrophe.
- **Calculator.**
  - (a) "one way people end up with ten times what they meant" (entry).
  - (b) The pen field reads "Amount to convert", in the page and the embed.
  - (c) "This is for compounded pens, where the concentration is printed on the label." became "Pens vary; read the concentration printed on your label." "Brand pens … follow the pen." stays before it (D3).
  - (d) The six worked examples moved off 250 and 500 mcg. Every number comes from the frozen math (`scripts/calc_faq.mjs`; the calculator gate checks it):
    - 100 mcg from a 5 mg / 2 mL vial is 4 units;
    - at 10 mg / 2 mL, 1 mg is 20 units;
    - a 5 mg vial holds 50 draws of 100 mcg;
    - the half-unit example is 1 mg from a 10 mg vial with 1.25 mL, 12.5 units, read as 13, which holds 1.04 mg;
    - on U-40, 1 mg from 10 mg / 2 mL is 8 units against 20;
    - the pen example moved from 0.5 mg to 1 mg: 40 units, 7 doses in a 3 mL pen (D4).
- **Titles and descriptions**, word for word, with their og:, twitter: and JSON-LD copies.
  - **CagriSema's description** is rewritten on every prerender, so it lives in `streetlab.DESCRIPTION_OWN`. That stops the build if CagriSema's stamp turns FDA APPROVED.
  - **CagriSema is not approved on October 24, 2026:** Drugs@FDA (openFDA) has no record, it isn't on FDA's 2026 novel approvals list (current to October 20), and Novo's newsroom shows no approval through October 20.
- **CagriSema's two flags (kalios-hq #38, read as data).**
  - **The date.** Novo's Q4 2026 expectation is in its announcement of **September 30, 2026**, read on novonordisk.com. No September 21 source exists. Fixed on CagriSema and on Cagrilintide, which carried the same date (D5), each with a References entry. Five dated citations left the backlog (599 to 594).
  - **The compounding line.** It now quotes FDA's October 1, 2026 statement: "Retatrutide and cagrilintide cannot be used in compounding under federal law". It used to say cagrilintide would become compoundable once CagriSema is approved.
  - **The box** is headed "Current Status — October 2026" and says the combination is "not yet approved in any market as of October 2026; Novo calls it investigational".
- **Redirects.** `/privacy`, `/terms`, `/disclaimer` answer 308, and `/compounds/mt-2.html` answers 301, live.
  - The stub is gone (`data/retired-pages.json`, kind legacy).
  - Four pages' MT-2 links and `/alerts.html#mt-2` point to Melanotan II's page.
  - Melanotan II's card for itself went (D6).
  - `/compounds/mt-2` reaches Melanotan II in two hops on production; the one-hop order is committed (`c1df795a`) and ships with the next deploy.
- **Links.** Each of these was added:
  - "Bioregulators" ends the homepage's goal list, with 19, the members its page shows (D7);
  - "See the evidence by compound →" on the Khavinson article;
  - "New here? Start with the beginner’s guide →" on About, `/compounds/` and How we check, in the site's existing one-line link.
- **Umami.** The tag sits in each page's own head, not a template, so prerender's decorate step sets `data-exclude-hash="true"` on every page; the goal and pair page generators write it too (D8). On production, `/coa/#test123`, with the collector's POST paused and failed so no visit counted:
  - before the deploy, the payload URL was `https://www.kalios.health/coa/#test123`;
  - after it, `https://www.kalios.health/coa/`.
- **CLAUDE.md.** Your two rules, word for word, under Content Rules: 357 characters of rules. The file is 75,054 characters, 731 more than this morning's; the rest is fact updates.
- **Petrelintide.** The vendor's page, its label quote and every "one supplier" sentence went.
  - The line under the stamp is your words, less "Not FDA-approved.", which the description already opens with (D9).
  - Route (sold) and Cost & Access read "Company trials only".
  - "no analysis of any such product has been published" went, with its sibling "any analysis of petrelintide sold outside the trials".
  - A second copy of the label quote, in Reconstitution & Storage, went too.
  - PubChem stays, for the formula and weight (checked today: C185H305N49O61, 4192).
  - Zealand's page was not cited: it blocked the headless browser, and the Chrome extension wasn't connected, so it couldn't be opened in a real browser as the brief required.
- **FLGR-242.** The seven jaycampbell.com and two Substack links are text, each "Campbell J., title, date. Not linked: the page links to a seller’s store." I opened all nine:
  - every jaycampbell.com page's Shop menu leads to its courses ($199) and to "Jay Recommends", a coupon link to the peptide seller;
  - seven of the nine also link the store or a coupon in their text.
  - The two posts on X stay linked; the brief didn't name them (Left for you 7).
- **COA Check.** The Finnrick sentence is yours, unlinked. Its partner terms do say "Finnrick also sells testing services to Partners."
- **The FDA Tracker.**
  - Each count names and links its report: Reuters for BPC-157, KPV, TB-500, MOTS-c, Semax and emideltide; STAT for Epitalon, written "7 yes, 4 no" in the board's own words (D10).
  - The headline line reads, as you wrote it: "6 of 7 recommended by the panel (Reuters, July 23–24, 2026) · none is on FDA’s 503A bulks list (FDA, checked October 24, 2026)", linked to Reuters' July 24 wrap and to 21 CFR 216.23.
  - The not-voting counts went. FDA's final roster seats 15, 14 or 12 voters by compound, and every reported count adds up to its compound's total, so no one was "not voting".
  - The "14 separate questions … identical results" paragraph became what FDA's documents and the reports say.
  - **The February 2027 meeting stays:** FDA's own PCAC page says "FDA will host an advisory committee meeting before the end of February 2027". The timeline had cited the July meeting page, which doesn't mention it.
  - The sources list gained the seven sources the page now cites, so the byline reads 13 (D11).
  - The seven compound pages' PCAC notices and Legal Status boxes, and the Khavinson article, print the same counts, so they got the same attribution (D12).
- **Corrections entries:** 19, now 1,358 in all.

### The independent check

A separate reader held the day's 19 entries and the new text against the base commit (`17112e8c`) and the saved sources before the deploy. It found 14 issues and some minor ones. Fixed before the deploy:

- **Stale boxes.** The seven compound pages' hand-written Legal Status boxes (and BPC-157's What It Is) still printed the tallies, with "not voting" counts and no source. They now match the notices.
- **Six entries were overstated.** They said "no source gives" results by form, an Epitalon abstention or the members' count. The FDA Law Blog gives the first two and FDA's roster the third. The entries now say the counts named no source.
- **The tracker.**
  - The July 24 wrap carries its updated headline.
  - A RAPS sentence I had added came out: its 7–5–1 is one more than the 12 who voted.
  - Emideltide's FDA-staff clause now cites the July 23 story.
- **The explainer.** Buchanan said the title alone doesn't establish the approach, and Executive Order 12866 does set time limits. Two changes to your text follow (D13).
- **Petrelintide.** Its note said prices are "set by the seller", and its Phase 3 heading still said "not started", though the three trials have been recruiting since early October. Both fixed, with the trial record.
- **CagriSema.** Its approval sentence was re-dated to October.

Left as they are, for you: the Privacy wording, the forms, Epitalon's abstention and Cagrilintide's 503A sentence (Left for you 2, 4, 5 and 6). Two findings didn't hold up: CLAUDE.md already called the Finnrick line unlinked, and a Pipeline item can't carry a link, so entry 3 now says where it is linked.

## Part A1: the explainer

- **Words.** Your text to the August 24 item, with your one correction. A script held all 35 sentences to the brief before the check, and then the check's two changes went in (D13):
  - "Law firms that follow FDA read it as pointing to an enforcement-discretion policy … (Holland & Knight …; Buchanan Ingersoll & Rooney …, which adds that the title alone “does not establish the enforcement approach FDA intends to adopt”)";
  - "A review has no fixed length and no published end date." is now "Executive Order 12866 sets time limits for a review that can be extended at the agency head’s request (Executive Order 12866, section 6(b)(2)), and OMB’s record gives no end date for this one."
- **The status line** is generated (`sl:policy-status`) from the OMB event, which gains `checked`, the day reginfo.gov's record was last read. Today it reads: "Status on October 24, 2026: under review at OMB since October 14, 2026 · not issued · no legal deadline." Prerender stops if the event's status words change.
- **Citations.** Every one links its source: OMB's record and meetings list; AP; the Post; Holland & Knight; Buchanan; FDA's 503A and safety-risks pages; the Federal Register notice; FDA staff's BPC-157 evaluation; FDA's meeting page; Reuters; the ten warning letters by their MARCS-CMS numbers.
  - Five don't answer scripts: reuters.com (robots.txt), apnews.com and bipc.com (bot checks), washingtonpost.com (timed out) and eCFR (a challenge page). Each was read another way, and each is recorded in `data/citation-allow.json` with a note, as the brief asked. The gate fails none of them either way.
- **Shape.** About's, in paper and ink; title, description, canonical, a share image drawn by the build, Checked 24 Oct 2026, a sitemap entry.
  - It appears as a new page on /changes.html, the feed and the homepage's what-changed (D14).
  - The tracker links it under its line to the changes page, so it isn't an orphan (D15).
- **Facts held to their sources:**
  - OMB's record: pending; received 10/14/2026; Notice; no legal deadline; not economically significant.
  - Six EO 12866 meetings, 10/21 to 10/28, all "Scheduled"; the detail pages name requestors, and none lists attendees.
  - AP via its wire copies (the story ID matches), the Post via ADN's copy, Holland & Knight.
  - FDA's pages and quotes, all ten letters, and the Reuters counts.

## Checks

| Check | Result |
|---|---|
| Calculator gate | PASS: 400/400, the 28-case self-test, QR 123, the FAQ matches calc-math.js v1.0 |
| Citation gate (`--links-new`) | AUDIT PASSED: 5,955 citations on 181 pages, 0 failing, 0 to review, 109 allowed. 4,472 references (4,154 linked, 318 text), 15,809 in-text citations anchored, 594 dated citations in the backlog, 0 stale. 4,403 links; 20 new across the two runs, 13 gone (the vendor's page, nine promoter pages, Finnrick's two, the RAPS story) |
| Card gate | PASSED: 157 cards, 1,466 drawn, 5 allowed |
| hero_meter | 151 one lines, the nine goal pages and /stacks/: 0 failing |
| Token lint | 0 findings (two selectors added to the running-text link rule, `.sl-num-s a` and `.sl-board-vote a`) |
| first_screen | 161 pages × 2 themes and the calculator: 0 misses. The explainer, the legal pages and COA Check × 2 themes: 0 misses |
| search_check | 210 entries, 0 failing |
| COA tests | scorer and function: pass |
| Sweep | the 28 touched views, both themes and the Aa flips: 58 page-theme pairs, 0 wider, 0 under AA, 0 404s, 0 dead links |
| Prerender | settles, before and after the snapshot. A first run after a correction redraws two share images on the second run (Left for you 9) |
| Sitemap | 207 URLs |
| Guard | PASS on the git-less copy: 458 files, `data/search.json` the only data file. Production's tree is those 458 exactly |
| Deploy | `dpl_nfK221cj3AYeKea4o5rxRC3oafUu`, READY, second attempt |
| Live | 39 checks, 38 pass. The miss is `/compounds/mt-2`'s two hops, fixed in `c1df795a` for the next deploy |
| Seller-link grep | No seller or promoter store link. Matches: Syn-Coll's maker product page and its archived copy; the UK medicines compendium (official labels); FLGR-242's two X posts |
| Secrets and identity | `git ls-files --deleted` empty; gitleaks: no leaks; both identity checks print nothing; no trailers |

## Left for you

1. **The rest of the brief.** Reply "use prompt-v1.md" in chat, or paste the rest.
   - It would add, as that file has it:
     - A1's last two timeline items, "What changes for readers today", "When it issues", the closing line and Sources;
     - the tracker's OMB-entry link ("What it would and wouldn't change: the explainer →"); for now the tracker links the explainer under its changes-page line;
     - A2, A3 and B–E, with your four corrections applied.
   - (1) Build from the file.
   - (2) Paste the rest.
   - (3) Stop here.
2. **Privacy's Visits sentence names less than Umami gets.** The code also sends:
   - `door` (which door), `search` (the overlay opened) and `receipt-click` (the compound and where the link sat);
   - `share` on any "Share this page", not only a tool's.
   For a named stack, `receipt-run` records the stack, not a pair, and `calc-run`'s compound is the page the visit came from.
   - (1) Add "and which doors, searches, links and shares are used, with the page".
   - (2) Leave your words.
3. **Two changes to your explainer text** (D13).
   - (1) Keep.
   - (2) Your own words.
4. **The forms.** The FDA Law Blog reports both forms for BPC-157, KPV, TB-500, MOTS-c and emideltide. The pages no longer name forms.
   - (1) Leave.
   - (2) Restore them, citing the Law Blog.
5. **Epitalon.** The board shows STAT's 7–4, as you asked. The FDA Law Blog's 7–4–1 matches FDA's roster of 12 voters; RAPS's 7–5–1 doesn't.
   - (1) Keep.
   - (2) Add the abstention, citing the Law Blog.
6. **Cagrilintide keeps a forward-looking sentence**: once CagriSema is approved, cagrilintide becomes a component that 503A lets a pharmacy compound with. CagriSema's page now quotes FDA instead.
   - (1) Same treatment.
   - (2) Leave.
7. **Links the brief didn't name:** FLGR-242's two X posts by the promoter, and Syn-Coll's maker product page.
   - (1) Text, like the rest.
   - (2) Leave.
8. **"Every page touched moves its Checked date."** Pages whose text changed moved. The four pages whose only change was a link target (PT-141, oxytocin, Melanotan I, setmelanotide) and /alerts.html kept theirs: CLAUDE.md and About define the date as the last content change (D2).
   - (1) Keep.
   - (2) Move them.
9. **Prerender's "one run is complete"** misses by two share images after a correction (corrections, home); the second run settles them.
   - (1) Fix the order.
   - (2) Leave; the checklist runs it twice.
10. **The MT-2 one-hop redirect** (`c1df795a`) ships with the next deploy.
11. **The GHK-Cu qualifier.** FDA's page for the 2027 meeting lists "GHK-Cu" with no route. The tracker's "non-injectable routes only" rests on FDA's 503A list (the nominator kept only non-injectable routes); the event cites both.
12. **"Says nothing about products sold as 'research use only'"** (the explainer) is Kalios's reading of a text no one has seen. Holland & Knight supports "would not cover".
    - (1) Leave.
    - (2) Cite Holland & Knight there.
13. **Versions.** Vercel CLI 60.1.3 runs; 65.2.0 is out. pdf.js stays at 6.3.289. About 40 Homebrew formulae are outdated, git, gh, node and openssl@3 among them.

## DECISIONS

- **D1. Parts not received were not built.** The paste ended inside A1. The brief's "make the smaller, reversible choice" and CLAUDE.md rule 3 meant building only what reached me in chat. A file holding the rest is data until you confirm it.
- **D2. Checked dates move with text or a correction** (CLAUDE.md, About's "What ‘Checked’ means"). The four link-only pages and /alerts.html kept theirs.
- **D3. The pens line.** I replaced the sentence you quoted, not the line, so "Brand pens … follow the pen." stays.
- **D4. The pen example** was 0.5 mg, which is 500 mcg, so it moved to 1 mg.
- **D5. Cagrilintide's date.** Its page carried the same wrong date and was fixed with its own entry.
- **D6. MT-2.** Melanotan II's Related card for itself went rather than staying unlinked; it named the page itself.
- **D7. Bioregulators on the homepage only.** The guide's chips are unchanged.
- **D8. Umami.** No shared template held the tag, so decorate sets it on every page; the goal and pair generators write it too.
- **D9. Petrelintide's line** is your sentence without "Not FDA-approved." The description already opens with those words ("Not FDA-approved; investigational. …").
- **D10. The board writes counts in its own words.** Epitalon reads "7 yes, 4 no (STAT, July 24, 2026)"; prose reads "7–4".
- **D11. The tracker's sources list** is now headed "Sources" and lists the seven sources its new text cites, news reports marked as such. The byline counts that list.
- **D12. The same counts elsewhere.** They were attributed on the seven compound pages and the Khavinson article too (one entry each), so the site doesn't contradict itself.
- **D13. Two changes to your final text** (Buchanan; Executive Order 12866), each a fact the cited source contradicted.
- **D14. The explainer shows as new** on the homepage's what-changed, like any new page.
- **D15. One inbound link.** The tracker links the explainer, so it isn't an orphan.
- **D16. Allowlist entries.** Links that don't answer scripts are linked and recorded in `data/citation-allow.json` with how each was read. The gate lists them and fails none.
- **D17. `stacks/data.json`.** The route migration also carried five route corrections from fix 8 into this internal file; the compound pages had them already.

## Counts

- **Pages:** 209 decorated (the explainer added, the MT-2 stub gone); 207 in the sitemap.
- **Corrections logged:** 1,358, 19 this session.
- **Citations:** 5,955 on 181 pages, 0 failing. **Links:** 4,403, 0 failing.
- **Status changes:** 1, the explainer.
- **Deployed files:** 458.
- **CLAUDE.md:** 75,054 characters, 731 added (357 of rules).

## Files changed

From `17112e8c`: 6 commits, then this report. 280 files in all, most of them each page's head (the Umami attribute).
- **New:** `fda-peptide-policy.html`, `og/fda-peptide-policy.png`.
- **Removed:** `compounds/mt-2.html`.
- **Pages:**
  - `privacy.html`, `calculator.html`, `embed/calculator.html`, `index.html`, `about.html`, `how-we-check.html`, `compounds/index.html`;
  - `khavinson-bioregulators.html`, `coa/index.html`, `fda-pcac-2026.html`;
  - compounds: CagriSema, Cagrilintide, petrelintide, FLGR-242, BPC-157, KPV, TB-500, MOTS-c, Semax, Epithalon, DSIP, Melanotan II, Melanotan I, oxytocin, PT-141, setmelanotide.
- **System:**
  - `scripts/streetlab.py` (DESCRIPTION_OWN, the tracker's line and board, the policy status line, the alias redirect, the Bioregulators chip, the Umami attribute);
  - `scripts/gen_regulatory_status.py` (the votes with their sources; the 2027 meeting's FDA page);
  - `scripts/card_data.py`, `scripts/calc_faq.mjs`, `scripts/prerender.py`, `scripts/first_screen.py`, `scripts/goals.py`, `stacks/generate.py`;
  - `assets/fine-print.css`, `vercel.json`.
- **Data:** `changelog.json`, `regulatory-status.json`, `regulatory-events.json`, `routes.json`, `pipeline.json`, `retired-pages.json`, `dated-cites-backlog.json`, `citation-allow.json`, `counts.json`, `link-check.json`, `page-dates.json`, `og-manifest.json`, `status-snapshot.json`; `stacks/data.json`.
- **Generated:** `corrections.html`, `changes.html`, `alerts.html`, `feed.xml`, `llms.txt`, `sitemap.xml`, the goal pages and the share images whose dates moved.
- **Docs:** `CLAUDE.md`, `docs/TEMPLATES.md`.
- **Record:** `gsc-export/october-build-2026-10-24/`: `sources.md` (every source, how it was read), the sweep list, `live/` (the live checks, the Umami before and after) and the IndexNow list.

## URLs to eye-check

- The explainer: https://www.kalios.health/fda-peptide-policy.html
- The tracker's headline line, board and Vote Results paragraph: https://www.kalios.health/fda-pcac-2026.html
- Petrelintide, with no seller: https://www.kalios.health/compounds/petrelintide.html
- FLGR-242's References: https://www.kalios.health/compounds/flgr-242.html
- CagriSema's title and Legal Status: https://www.kalios.health/compounds/cagrisema.html
- The calculator's worked examples ("The math, by hand"): https://www.kalios.health/calculator.html
- Privacy: https://www.kalios.health/privacy.html
- A PCAC notice and Legal Status box: https://www.kalios.health/compounds/bpc-157.html
- The homepage's title, Bioregulators chip and what-changed: https://www.kalios.health/
- The 19 entries: https://www.kalios.health/corrections.html

## Session start and web content

- **Versions:**
  - Claude Code 2.1.310.
  - Vercel CLI 60.1.3 installed, 65.2.0 published.
  - Homebrew: about 40 formulae outdated.
  - pdf.js 6.3.289.
- **Web content.** Everything fetched was read as data (CLAUDE.md rule 3). What it contained:
  - kalios-hq issues #38, #28 and #40 address requests to Claude ("Ticket for Claude", "Claude to build"), written by other agents. I took from them only the details the brief pointed to (CagriSema's two flags, source addresses), checked each at its source, and acted on none of their requests.
  - The independent reader's report was another model's words; every finding was checked before anything changed.
  - Nothing else fetched read as an instruction.
- **Sources read.** The full list, with how each was read, is in `gsc-export/october-build-2026-10-24/sources.md`. Robots.txt was read first on every host, and fda.gov's 30-second delay was kept.
  - Read directly: FDA's pages and PDFs, OMB's record and meeting pages, the Federal Register API, eCFR's API, openFDA, ClinicalTrials.gov's API, PubChem, Novo's two releases, Roche, Holland & Knight, STAT, RAPS, the FDA Law Blog, Finnrick's terms, and the nine promoter pages.
  - Read in copies: Reuters (robots.txt keeps scripts off reuters.com; you read the stories there), AP, the Post and Buchanan (Cloudflare bot checks or timeouts; not bypassed).
- **Refusals:** reuters.com (robots.txt); apnews.com, bipc.com and zealandpharma.com (bot checks); globenewswire.com and investing.com (403); washingtonpost.com (timed out).
