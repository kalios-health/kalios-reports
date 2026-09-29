# 12c — Goal pages, stamp explainers, route as two facts, remainder

**Date:** 2026-09-29  ·  **Deploy:** `dpl_BYrVbCWpXWbCHwWmVv1cbm7gTJe9` (Ready, production)  ·  **Live:** https://www.kalios.health

Five parts in one session, one commit each, all pushed. Deployed per the checklist; IndexNow accepted 172 URLs.

---

## Look at these first

1. **Your epithalon example doesn't match its own page.** The brief's example reads "studied IM/SubQ, sold SubQ and intranasal". Checked against the page and its cited abstracts:
   - **Studied.** IM/SubQ in people comes from the dosing table's "Khavinson elderly (injectable)" protocol row. The human cohort behind that name was given **Epithalamin**, the pineal extract, not synthetic epithalon, and the page says so itself. The only cited study of epithalon with a stated route is **SubQ in mice** (Anisimov 2003, PMID 14501183). The cited human studies (retinitis pigmentosa, the Russian series) give no route. The page now reads **"SubQ (mice) · human route not stated"**.
   - **Sold.** The page describes lyophilized vials for SubQ. It calls intranasal a "less common" community use that needs reformulating, not a product anyone sells. The page now reads **"SubQ vials (research chemical)"**.
2. **"Controlled trials" is not the meter's word.** The goal pages group cards under the meter's own labels: Approved drug · **Human trials** · Human pilots · Animal only · No data. The 12b report's table called level 3 "Controlled trials", which was my error, and the brief repeats it. The artboard (v11-Card) and the site both say "Human trials". Level 3 is defined as published controlled trials, so "Controlled trials" is arguably the better word. Renaming it is one string in `EVIDENCE_LABELS` plus the About and guide definitions. Say if you want it.
3. **Three of your five stamp lines aren't true of every page that carries the stamp.** Each stamp keeps your exact line everywhere it's true, and About uses all five verbatim. The 36 exceptions carry a line of their own, keeping your first clause wherever the facts allow (full table below). The main cases:
   - **NOT LEGAL TO COMPOUND** ("FDA said no: on the do-not-compound list") is carried only by the six stacks, and it's true of none of them. A stack gets that stamp when any member isn't legal to compound; Wolverine's two members are PCAC-recommended, the opposite of "FDA said no". Stacks read "At least one compound in it isn't legal to compound." **Kisspeptin and MK-677 fit your definition exactly** (FDA's 503A Category 2, significant safety risks), but their stamp is RESEARCH ONLY, so they now read "FDA said no: on its 503A Category 2 list (significant safety risks). Sold anyway as a research chemical." Should they get the NOT LEGAL TO COMPOUND stamp? That would be a status change, so I didn't make it.
   - **RESEARCH ONLY** ("No FDA ruling: not on any list. Sold as a research chemical…") is false for 28 of the 92 research-only pages:
     - 5 on FDA's Category 3 list;
     - the 2 in Category 2 above;
     - 4 trial-only drugs that aren't sold;
     - 15 cosmetic-first peptides sold in creams;
     - noopept, sold as a supplement;
     - PDA, sold by clinics.
   - **COMPOUNDING PHARMACY** ("On FDA's 503A list") is false for gonadorelin and sermorelin. They are compoundable as ingredients of discontinued approved drugs, not through a list.
4. **The compound hero has no legal stamp.** Spec rule 3 gives the hero one stamp, the evidence verdict. So on compound pages the line names the legal stamp first, in its color ("NOT LEGAL YET · FDA's committee recommended…"). On stack pages, whose hero stamp *is* the legal stamp, the line sits under it. No second stamp was added.
5. **"Sold for" became "used for" in the goal sentences.** Five fat-loss compounds are trial-only (MariTide, amycretin, glumitide, VK2735, pemvidutide), so "Of 32 compounds sold for fat loss" would be false. The site already defines the use tag as "what people take it for".
6. **The citation gate has a gap.** An inline citation passes when journal and year match, even if the sentence names a different first author. At least eight look like wrong PMIDs:
   - SLU-PP-332 "Billon et al., 2023" (PMID 37673341 is a small-RNA paper by Chen and Zhou);
   - humanin "Hashimoto et al., 2001" (11756493 is Harris);
   - HGH "Mauras et al., 2000" (10969269 is Zacharin);
   - HGH fragment "Wu et al., 1993" (8440175 is Yamaguchi);
   - PEG-MGF "Mills et al., 2011" (21604383 is Fink);
   - P21 "Kazim et al., 2017" (28823930 is van der Stijl);
   - pramlintide "Whitehouse et al., 2002" (12242462 is Jackerott);
   - triptorelin "Humaidan et al." (16311288 is Marchetti).

   Not fixed: each needs its right paper found. Tightening the gate would fail these pages until then.

---

## Part 1 — nine goal pages

`/goals/<group>.html`, written whole from the data by `scripts/goals.py` on every prerender run.

**Top to bottom:**
- "By goal" and the H1, "{Group} — what actually works, by evidence". The title follows your pattern.
- One true sentence computed from the evidence levels.
- The six-element cards under the meter's labels, highest first. An empty level says "None."
- Cards that sit in the group only through a secondary tag, under "Also used for …".
- A data table with "Legal today" and "What people actually run".
- The alert form. No references.

**Wiring and SEO:**
- The homepage chips, the guide's goal chips and the `/compounds/` group headers link to the goal pages. The Stacks chip still goes to `/compounds/#stacks`.
- Added to the sitemap; each page has its own OG image.
- JSON-LD: BreadcrumbList and CollectionPage.
- `/goals/<slug>` and `/goals/<slug>/` redirect to the page. `/goals/` goes to `/compounds/`.

| Goal | The sentence | Also used for | Legal today (every card on the page) |
|---|---|---|---|
| [Fat loss](https://www.kalios.health/goals/fat-loss.html) | Of 32 compounds used for fat loss, 5 are approved for that use and 16 more have controlled human trials. The other 11 rest on pilot studies or animal data. | 2 | 8 FDA-approved · 0 compoundable · 1 not legal yet · 25 research-only |
| [Muscle & growth hormone](https://www.kalios.health/goals/muscle-growth-hormone.html) | Of 15 compounds used for muscle and growth hormone, 2 are approved for that use and 3 more have controlled human trials. The other 10 rest on pilot studies, animal data or nothing of their own. | 2 | 3 FDA-approved · 1 compoundable · 1 not legal yet · 12 research-only |
| [Tendon & gut repair](https://www.kalios.health/goals/tendon-gut-repair.html) | Of 5 compounds used for tendon and gut repair, none is approved for that use and none has a controlled human trial for it. They rest on pilot studies, animal data or nothing of their own. | 1 | 0 FDA-approved · 0 compoundable · 3 not legal yet · 3 research-only |
| [Skin & hair](https://www.kalios.health/goals/skin-hair.html) | Of 19 compounds used for skin and hair, 1 is approved for that use and 1 more has controlled human trials. The other 17 rest on pilot studies, animal data or nothing of their own. | 3 | 1 FDA-approved · 1 compoundable · 3 not legal yet · 17 research-only |
| [Mind & sleep](https://www.kalios.health/goals/mind-sleep.html) | Of 14 compounds used for mind and sleep, none is approved for that use; 4 have controlled human trials. The other 10 rest on pilot studies, animal data or nothing of their own. | 2 | 2 FDA-approved · 0 compoundable · 2 not legal yet · 12 research-only |
| [Brain & nerves](https://www.kalios.health/goals/brain-nerves.html) | Of 3 compounds used for the brain and nerves, none is approved for that use; 2 have controlled human trials. The other one rests on animal data alone. | 3 | 0 FDA-approved · 0 compoundable · 1 not legal yet · 5 research-only |
| [Hormones & libido](https://www.kalios.health/goals/hormones-libido.html) | Of 13 compounds used for hormones and libido, 7 are approved for that use and 3 more have controlled human trials. The other 3 rest on pilot studies or animal data. | 0 | 7 FDA-approved · 2 compoundable · 1 not legal yet · 3 research-only |
| [Longevity](https://www.kalios.health/goals/longevity.html) | Of 15 compounds used for longevity, none is approved for that use and none has a controlled human trial for it. They rest on pilot studies or animal data. | 5 | 2 FDA-approved · 2 compoundable · 2 not legal yet · 14 research-only |
| [Immune](https://www.kalios.health/goals/immune.html) | Of 10 compounds used for immune support, none is approved for that use; 1 has controlled human trials. The other 9 rest on pilot studies or animal data. | 0 | 0 FDA-approved · 1 compoundable · 2 not legal yet · 7 research-only |

The sentence counts only the compounds whose **use tag** is the group. A secondary-tag card's meter rates its main use, the tag printed on it. HGH, for example, reads "Approved drug" for GH deficiency. Counting it would have said that one longevity compound is approved for longevity. Those cards are listed last, under "Also used for …", with that explained in one line.

---

## Part 2 — stamp explainers

Your five lines live in `STAMP_LINES` (`scripts/streetlab.py`), verbatim with curly apostrophes. They appear in three places:

- **Hero.** One mono line under the hero's title row on every compound, alias and stack page (`sl:legal`).
- **Cards.** Every card's stamp carries the line as its `title`, so a mouse shows it on hover. On touch there is no hover, so a tap on the stamp shows the line inside the card instead of opening the page; a second tap, or a tap anywhere else, hides it. Mouse clicks open the card as before. I tested this with an emulated touchscreen: the first tap showed the line, the second hid it, and a tap on the name opened the page. It is the sixth job in `street-lab.js`.
- **About.** The five definitions are generated (`sl:stamp-defs`), so they are the same strings the pages use, plus one sentence: "On a stack, NOT LEGAL TO COMPOUND means at least one compound in it isn't legal to compound. Where a compound's own facts don't fit its stamp's line, the line under its name says what does."

**Counts:** your exact line on 99 pages; a line of its own on 36 (table at the end).

---

## Part 3 — route as two facts

`data/routes.json` holds, for all 126 compounds:
- `studied`: how the compound was given in the human or animal studies its page cites;
- `sold`: how it is commonly sold, from the page's own product descriptions.

The six stacks carry their own pair. Each studied route lists the PMID behind it, or "page" where the page's own description of a cited study is the source.

**How the data was gathered:**
1. Every page was read for both facts, with verbatim quotes.
2. The 1,319 PubMed abstracts the 126 pages cite were fetched and mined for routes and species.
3. A second pass checked each studied route against those abstracts.
4. I reviewed all 126.

The second pass mattered. BPC-157's page never says how its rat studies dosed it; the cited abstracts do: intraperitoneally, by mouth, locally.

**Rules:**
- Only studies the page cites count. Manufacturer dossiers listed in its references count.
- A parent, precursor, analog or other formulation doesn't count (it goes in `parent_note`). Examples: thymosin beta-4 for TB-500, Epithalamin for epithalon.

**Where the two fields render:**
- The data table: "Route (studied)" and "Route (sold)" replace the hand-written Route row on 135 pages (126 compounds, 3 alias pages showing their canonical compound's, and 6 stacks).
- The FAQ answer to "How is X administered?" (104 pages). JSON-LD can't hold `sl:` markers, so prerender rewrites that answer in place from the data.
- Each stack's members block: "In this stack · route, as studied and as sold".
- The 297 lookup-only pair pages' quick facts (all 317 pair pages were regenerated; the 20 kept pages state no routes).
- The Stacks hub's live lookup (`/stacks/data.json`: `route-studied`, `route-sold`).

**Blend pages.** Two hand-written sentences that stated a member's route were removed:
- GLOW, on GHK-Cu: "Topical and injected routes both viable."
- Wolverine, on BPC-157: "Also orally bioavailable (stable in gastric juice) — the only peptide in this stack with real oral activity."

Protocol steps ("0.1 mL SubQ daily from the pre-blend") and DS5's argument that intranasal dihexa is uncharacterized stay: they describe the blend, not a route field.

**Two pages misstated a cited study's route**, which now contradicted the new rows. Both are fixed, with corrections entries:
- GHRP-6: Cabrales 2013 gave one IV bolus, not SubQ.
- GHRP-2: Mericq 1998 injected it under the skin; only Pihoker 1997 used a nasal spray. Both abstracts also report higher growth velocity, so "growth promotion was not clinically meaningful" went too.

The full differences list is at the end.

---

## Part 4 — remainder

- **(a) Four page fixes, each a corrections entry.**
  - **GW-0742:** WADA class S4.4.1, in five places on the page and on its three pair pages. The Sobolevsky 2012 controlled excretion study (a single 15 mg oral dose in people; PMID 22977012) is now in the Human Data section, the dosing opener, the data table and the references. The Human Data section had also said it was never given to humans "in a peer-reviewed clinical pharmacology report"; that went too. The title's "(No Human Trial)" stays, because the study measured urine detection and was not a clinical trial.
  - **AICAR:** the "most dangerous community stack on safety grounds" line is removed.
  - **Glumitide:** the review is by Douros, Mowery and Knerr (PMID 40507574), in two text mentions and the reference.
  - **CagriSema and cagrilintide:** the amycretin paper is cited as Dahl K, et al. Lancet. 2025;406(10499):149-162. PMID 40550231. CagriSema's reference also carried a code, "(NNC0487-0111)", that is not in the paper's title; it is gone.
- **(b)** All **121** `?filter=` links now point to `/compounds/?tag=<group>#<group>`. The brief says 119; two pages added since 12b carry them too. Each filter maps to its group:
  - `weight-loss` → fat-loss
  - `skin-health` → skin-hair
  - `growth-hormone` → muscle-growth-hormone
  - `tissue-repair` and `gut-health` → tendon-gut-repair
  - `hormone-opt` and `sexual-health` → hormones-libido
  - `sleep` → mind-sleep
  - `longevity`, `immune` and `stacks` unchanged
  - `cognitive` split into two groups, so each page gets its own: brain-nerves for cerebrolysin and cortagen, mind-sleep for the other ten.
  - `fda-approved` has no group, so insulin's link goes to its own group, hormones-libido.

  The `#group` keeps the link working without JavaScript, as the primary action's does.
- **(c) Klotho's Pipeline row:**
  - NCT07216781, Minicircle's plasmid gene-therapy pilot, active and not recruiting;
  - NCT07544420, Klothea Bio's alpha-Klotho mRNA Phase 1b, not yet recruiting.

  Both are labeled "(gene therapy / mRNA)" and were checked on the ClinicalTrials.gov API today. The pilot's estimated primary completion, August 2026, has already passed; that is what the registry says.
- **(d) Ecnoglutide's Pipeline row** shows its three recruiting Phase 3 trials, then "+4 more →". The page had no Pipeline section, and the 17-section template is locked, so the link goes to a generated "Pipeline" list (an h3, `id="pipeline"`) at the end of its Human Data section, drawn from the same data. The rule is general: a row longer than five items shows the three latest-phase ones. Only ecnoglutide (7) is over; petrelintide has 5 and is unchanged. All seven registry records were rechecked today.
- **(e) HMG:** the reference that named a research-chemical seller and its catalog address now describes the listing without the name or link. The text's "at least one online catalog" sentences still have a source. Corrections entry added.
- **(f)** The thirteen 12b compounds are in the Stacks hub's lookup. They are `lookup_only` in `/stacks/data.json`: the pickers offer them and the view renders inline, while `stacks/generate.py` gives them no pair page, so no new files. Tier-1 pair count is unchanged at 317; amycretin + semaglutide renders inline. The lookup now covers 125 compounds, so "112" became 125 in the Related Resources line on 114 compound pages and in the hub's descriptions, plus the homepage and hub FAQ answers. The homepage FAQ's stale "322 pre-generated pair pages" became 317.
- **(g)** The homepage line reads "29 Sep — 13 compounds added (amycretin, MariTide, cardarine, PDA …); 20 combination pages rebuilt." It is the newest content entry in the changelog; 12b's two entries stay as history.

---

## DECISIONS

1. **Goal sentence: "used for", not "sold for"** (Look at these first, 5).
2. **The sentence and the evidence headings cover the group's own use-tag compounds.** Secondary-tag cards go last under "Also used for …", so the page never implies a meter rates the goal's use when it rates another.
3. **Headings use the meter's labels** ("Human trials"; Look at these first, 2).
4. **"Legal today" counts every card on the page.** It adds "not legal yet" when there are any, so the four numbers sum to the cards shown. Your three categories alone would leave the PCAC compounds uncounted.
5. **"What people actually run" lists the named stacks whose use tag is the group.** Groups without one say "None of the six named stacks". Fat loss, brain and nerves, hormones and immune have none.
6. **The goal pages add** a "← All compounds" back link, JSON-LD and extensionless redirects. Cards sit in a generated block, so a card's "Updated" date never moves the page's own Checked date.
7. **Stamp lines: your words verbatim wherever true,** a compound's own line only where your line contradicts its page (Look at these first, 3). The own lines keep your first clause where they can.
8. **DSIP keeps your RESEARCH ONLY line.** "No FDA ruling" is true, since the committee's vote is advisory, and its caution band already says the committee voted against it.
9. **The line on compound heroes names the legal stamp** (Look at these first, 4).
10. **Tap-to-reveal only on touch or pen.** A mouse click on a card's stamp still opens the card.
11. **Studied means cited.** A study the page describes but doesn't cite doesn't count; the supplier panels on nonapeptide-1 are an example. Manufacturer dossiers in the references do count (vialox, SNAP-8, Syn-Coll).
12. **Three cell forms state what's missing:** "No study of it", "Cells only · no animal or human study" and "Not stated in its studies". A known human study with no stated route reads "… · human route not stated" (epithalon, selank, thymagen).
13. **Alias pages show their canonical compound's route facts** (Adamax → N-acetyl semax, GHK-Cu fragment → GHK basic, Vesilute → Vesugen).
14. **"All 127 compounds":** the site has 126 compound profiles, plus 3 alias pages that share their canonical's facts. All 126 have both fields.
15. **GW-0742's studied route includes the human excretion study** added in Part 4: "Oral (one human excretion study) · oral (mice)".
16. **Stacks' own route pair.** Wolverine's is "IP (rats; the one study of the pair)" (Biçer 2026, whose abstract says "administered intraperitoneally"). The other five read "No study of the combination" or "No study of the pair". Their sold forms come from each stack page.
17. **`data/routes.json` is in `.vercelignore`.** It is a build input no page fetches, like `data/dosing-openers.json`.
18. **The `?filter=` redirect stays in `vercel.json`** for outside links; no internal link uses it now.
19. **`stacks/extract.py` gained `from __future__ import annotations`** so it runs on the machine's Python 3.9 (its `str | None` hints failed); no behavior change.
20. **One commit per part.** Part 4 landed before Part 3 because the route data waited on the literature pass. GW-0742's route reflects Part 4's fix.

---

## Corrections added (8, on /corrections.html)

| Page | Was | Now |
|---|---|---|
| GW-0742 | WADA S4.5; "never given to people in a study" | S4.4.1; one controlled excretion study, a single 15 mg oral dose (Sobolevsky 2012) |
| AICAR | GW501516 + AICAR called "the most dangerous community stack on safety grounds" | line removed |
| Glumitide | review attributed to Killion, Lu and Véniant | Douros, Mowery and Knerr (PMID 40507574) |
| CagriSema | amycretin Lancet paper with no authors or PMID, and a code in its title | Dahl et al., Lancet 2025;406(10499):149-162 (PMID 40550231) |
| Cagrilintide | same paper, no authors or PMID | the same, with authors and PMID |
| HMG | a reference named a research-chemical seller and its catalog address | the listing described without the seller |
| GHRP-6 | Cabrales 2013 said to give GHRP-6 subcutaneously | a single IV bolus of 100, 200 or 400 µg/kg |
| GHRP-2 | both pediatric trials called intranasal; growth "not clinically meaningful" | nasal (Pihoker 1997) and subcutaneous (Mericq 1998); growth velocity higher on treatment, IGF-1 unchanged |

---

## URLs to eye-check

- **The nine goal pages:** [fat loss](https://www.kalios.health/goals/fat-loss.html) · [muscle & GH](https://www.kalios.health/goals/muscle-growth-hormone.html) · [tendon & gut](https://www.kalios.health/goals/tendon-gut-repair.html) · [skin & hair](https://www.kalios.health/goals/skin-hair.html) · [mind & sleep](https://www.kalios.health/goals/mind-sleep.html) · [brain & nerves](https://www.kalios.health/goals/brain-nerves.html) · [hormones & libido](https://www.kalios.health/goals/hormones-libido.html) · [longevity](https://www.kalios.health/goals/longevity.html) · [immune](https://www.kalios.health/goals/immune.html)
- [BPC-157](https://www.kalios.health/compounds/bpc-157.html) — the explainer under the title, and the two route rows.
- [Epithalon](https://www.kalios.health/compounds/epithalon.html) — the two routes (Look at these first, 1).
- [GW-0742](https://www.kalios.health/compounds/gw-0742.html) — S4.4.1, the excretion study, the route rows.
- [About](https://www.kalios.health/about.html) — the five definitions under "Legal stamp".
- [Mito Stack](https://www.kalios.health/compounds/mito-stack.html) — a stack's line, and the members' routes.
- [/compounds/](https://www.kalios.health/compounds/) on a phone — tap a card's stamp. The group headers link the goal pages.
- [Homepage](https://www.kalios.health/) — the "what changed" line; the chips go to the goal pages.
- [Kisspeptin](https://www.kalios.health/compounds/kisspeptin.html) — a Category 2 compound's own line.
- [Ecnoglutide](https://www.kalios.health/compounds/ecnoglutide.html) — "+4 more →" and the list it opens.
- [Klotho](https://www.kalios.health/compounds/klotho.html) — the Pipeline row.
- [Stacks hub](https://www.kalios.health/stack.html?a=amycretin&b=semaglutide) — a new compound in the lookup.
- [A pair page](https://www.kalios.health/stacks/pairs/ara-290-with-bpc-157/) — the route cells.
- [Corrections](https://www.kalios.health/corrections.html) — the eight entries.

---

## Open questions for G

1. **Re-stamp kisspeptin and MK-677 as NOT LEGAL TO COMPOUND?** Your definition fits them exactly; the stacks that carry the stamp don't fit it.
2. **Rename the meter's level 3 "Controlled trials"?**
3. **The beginner's guide defines the stamps in its own words** ("How to read a Kalios card"). Should it use the five lines too? The brief named only About.
4. **The citation gate's journal-and-year gap, and the eight suspect PMIDs** (Look at these first, 6). Find the right papers next session, and tighten the gate after?
5. **Petrelintide's Pipeline row has five items.** Truncate at three like ecnoglutide, or leave it?
6. **Found while reading, not fixed (outside this brief):**
   - SS-31 calls Daubert 2017 "PROGRESS-HF … in HFpEF", but that paper is a single-infusion trial in HFrEF.
   - Pinealon's evidence level (Human pilots) rests on a 72-patient Russian series that the page describes but doesn't cite.
   - Epithalon's Cost & Access section names four research-chemical vendors, like HMG did. Apply the same fix?
   - The hub's lookup says "No Biological Rationale Found" for amycretin + semaglutide. Both act on the GLP-1 receptor, but its tag matching compares category words, so it misses shared receptors when a category is worded differently.

---

## Gates

| Gate | Result |
|---|---|
| Calculator (`tests/run-calc-tests.sh`) | PASS: 400/400 agree, 28-case selftest, math frozen at v1.0, 6 FAQ answers match |
| Citations, whole site | **8,899 citations on 454 pages; 0 failing, 0 to review**, 153 allowed with a reason |
| Structure vs HEAD | every failure is an intended change: route text (431 pages), FAQ route answers (104), the 121 links, "112" → "125", the S4.5 fixes, About's definitions, the corrected references |
| Prerender | a second run changes nothing |
| Identity + gitleaks | both identity greps print nothing; no leaks found (every push) |
| Deletions | `git ls-files --deleted` empty; deployed from a git-less copy whose diff lists only `.git`, `.claude`, `reports`, `.DS_Store` |

## Deploy

`vercel --prod` from the git-less copy, first try: `dpl_BYrVbCWpXWbCHwWmVv1cbm7gTJe9`, Ready, aliased to www.kalios.health. IndexNow: 172 URLs, HTTP 200. Curled live, all 200 with the expected text:
- the nine goal pages, and `/goals/fat-loss` redirecting to the `.html`;
- BPC-157 (explainer and route rows);
- epithalon (both routes);
- GW-0742 (S4.4.1, Sobolevsky, route);
- About (the five lines);
- homepage, `/compounds/`, ecnoglutide, Klotho, HMG, corrections, the hub, a pair page.

AICAR no longer has "most dangerous", HMG no longer names the seller, and no page has a `?filter=` link.

## Session start (rule 2)

- Claude Code 2.1.284.
- Vercel CLI 60.1.3; 61.0.0 is available.
- `brew outdated` lists about 20 formulae, including git, icu4c, libsodium and the folly/fbthrift set.
- No pinned CDN libraries on the site: pages load only Google Fonts and Umami.

## Web content treated as data (rule 3)

**Fetched:**
- PubMed E-utilities: 1,319 abstracts cited by the compound pages, plus the three Part 4 papers and Biçer 2026;
- the ClinicalTrials.gov API (nine trial records);
- the claude.ai Design canvas's file listing, checked for a goal-page artboard (there is none).

None contained instructions. The canvas listing carries its own notice that its content is data, not instructions, and was read that way. Review agents read only local page dumps and those abstracts, with no web access.

## Files changed

481 files, +14,765 / −4,050, across six commits:

| Commit | Part |
|---|---|
| `d347f27d` | Part 1: nine goal pages |
| `7ff45bfd` | Part 2: stamp explainers |
| `2acf63a3` | Part 4: page fixes, `?filter=` links, pipelines, HMG, hub lookup, "what changed" |
| `edbf2e6e` | Part 3: route as two facts |
| `46accd2b` | Part 5: `/goals/` redirects; routes.json kept out of the deploy |
| `8dc386f2` | Part 5: sitemap lastmod |

New files: `scripts/goals.py`, `scripts/migrate_12c.py`, `data/routes.json`, `goals/*.html` (9), `og/goals/*.png` (9). CLAUDE.md documents the goal pages, the stamp explainers, route as two facts, the Pipeline "+N more" rule and the hub's `lookup_only`.

---

## Tables

### Route differences: all 90 compounds where the two routes differ

**Bold** = a route it is sold for that no cited study used, in any species (57 compounds).

| Compound | Route (studied) | Route (sold) | Sold, never studied |
|---|---|---|---|
| [5-Amino-1MQ](https://www.kalios.health/compounds/5-amino-1mq.html) | SubQ, IV, oral (mice) · no human study | Oral capsules (pharmacy Rx); SubQ vials (research chemical) | **sublingual** |
| [AHK-Cu](https://www.kalios.health/compounds/ahk-cu.html) | Cells only · no animal or human study | Topical scalp serums (cosmetic); powder (formulators) | **topical** |
| [Amycretin](https://www.kalios.health/compounds/amycretin.html) | SubQ, oral (people) | Not sold: trials only | — |
| [AOD-9604](https://www.kalios.health/compounds/aod-9604.html) | Oral (people and rats) · IP, implant (mice) | SubQ vials (research chemical); topical skincare (Australia) | **SubQ, topical** |
| [ARA-290](https://www.kalios.health/compounds/ara-290.html) | SubQ, IV (people) · IP (rats and mice) | SubQ vials (research chemical) | — |
| [Argireline](https://www.kalios.health/compounds/argireline.html) | Topical, transdermal (people) · topical (mice) | Topical creams and serums (cosmetic) | — |
| [Bimagrumab](https://www.kalios.health/compounds/bimagrumab.html) | SubQ, IV (people) | Trials only; lab reagent (research use only) | — |
| [BPC-157](https://www.kalios.health/compounds/bpc-157.html) | IV, intra-articular, intravesical (people) · IM, IP, oral, local injection (rats) | SubQ vials (research chemical) | **SubQ** |
| [BPC-157 Fragment](https://www.kalios.health/compounds/bpc-157-fragment.html) | No study of it | SubQ vials (research chemical) | **SubQ** |
| [Bromantane](https://www.kalios.health/compounds/bromantane.html) | Oral (people and rats) | Oral tablets (Rx in Russia); powder (research chemical) | **sublingual** |
| [Bronchogen](https://www.kalios.health/compounds/bronchogen.html) | Cells only · no animal or human study | Oral capsules (supplement); SubQ vials (research chemical) | **SubQ, IM, oral** |
| [Cardiogen](https://www.kalios.health/compounds/cardiogen.html) | Not stated in its studies | Oral capsules (supplement); SubQ/IM vials (research) | **SubQ, IM, oral** |
| [Cartalax](https://www.kalios.health/compounds/cartalax.html) | Cells only · no animal or human study | Oral capsules (supplement); SubQ/IM vials (research) | **SubQ, IM, oral** |
| [Chonluten](https://www.kalios.health/compounds/chonluten.html) | Not stated in its studies | Oral capsules (supplement); SubQ/IM vials (research) | **SubQ, IM, oral** |
| [Cortagen](https://www.kalios.health/compounds/cortagen.html) | IM (rats) · no human study | Oral capsules (supplement); SubQ/IM vials (research) | **SubQ, oral** |
| [Dihexa](https://www.kalios.health/compounds/dihexa.html) | IM, oral, local injection (rats) · no human study | Oral capsules and powder; transdermal, sublingual (research) | **sublingual, transdermal** |
| [DS5](https://www.kalios.health/compounds/ds5.html) | No study of it | Nasal spray; powder blend kits (research chemical) | **intranasal** |
| [DSIP](https://www.kalios.health/compounds/dsip.html) | IV (people) · into the brain (animals) | SubQ vials; nasal spray (research chemical) | **SubQ, intranasal** |
| [Ecnoglutide](https://www.kalios.health/compounds/ecnoglutide.html) | SubQ, oral (people) | SubQ injection (Rx in China); lab reagent (research) | — |
| [Eloralintide](https://www.kalios.health/compounds/eloralintide.html) | SubQ (people) | Not sold: trials only | — |
| [FGL](https://www.kalios.health/compounds/fgl.html) | Intranasal (people) · SubQ, intranasal, into the brain (animals) | SubQ vials (research chemical) | — |
| [FLGR-242](https://www.kalios.health/compounds/flgr-242.html) | No study of it | SubQ vials (research chemical) | **SubQ** |
| [Follistatin-344](https://www.kalios.health/compounds/follistatin-344.html) | No study of it | SubQ or IM vials (research chemical) | **SubQ, IM** |
| [FOXO4-DRI](https://www.kalios.health/compounds/foxo4-dri.html) | IP (mice) · no human study | SubQ vials (research chemical) | **SubQ** |
| [GHK Basic](https://www.kalios.health/compounds/ghk-basic.html) | Not stated in its studies | Topical serums and creams (cosmetic); SubQ vials (research) | **SubQ, topical** |
| [GHK-Cu](https://www.kalios.health/compounds/ghk-cu.html) | Topical (people) · local injection (rats) | Topical serums and creams (cosmetic); SubQ vials (research) | **SubQ** |
| [GHRP-2](https://www.kalios.health/compounds/ghrp-2.html) | SubQ, IV, intranasal (people) | IV vials (GHRP Kaken 100, Japan Rx); SubQ vials (research) | — |
| [GHRP-6](https://www.kalios.health/compounds/ghrp-6.html) | IV (people) · SubQ, IV, IP (rats) | SubQ vials (research chemical) | — |
| [Glumitide](https://www.kalios.health/compounds/glumitide.html) | SubQ (people) | Not sold: trials only (online listings aren't Lilly's) | — |
| [Glutathione](https://www.kalios.health/compounds/glutathione.html) | Oral, IV, intranasal, topical, inhaled, buccal (people) | Oral capsules (supplement); IV/IM/SubQ vials, nebulizer (Rx) | **SubQ, IM** |
| [Gonadorelin](https://www.kalios.health/compounds/gonadorelin.html) | SubQ, IV (people) | SubQ vials (compounded Rx); diagnostic vials (Rx outside US) | — |
| [Hexarelin](https://www.kalios.health/compounds/hexarelin.html) | SubQ, IV, oral, intranasal (people) · SubQ (rats) | SubQ vials (research chemical) | — |
| [HGH Fragment 176-191](https://www.kalios.health/compounds/hgh-fragment-176-191.html) | Not stated in its studies | SubQ vials (research chemical) | **SubQ** |
| [HMG](https://www.kalios.health/compounds/hmg.html) | SubQ, IM (people) | SubQ vials (Menopur, Rx); vials (research chemical) | — |
| [Humanin](https://www.kalios.health/compounds/humanin.html) | Into the brain (rats) · no human study | SubQ vials (research chemical) | **SubQ** |
| [IGF-1 DES](https://www.kalios.health/compounds/igf-1-des.html) | IV (rats) · topical (rodents) · no human study | IM or SubQ vials (research chemical) | **SubQ, IM** |
| [IGF-1 LR3](https://www.kalios.health/compounds/igf-1-lr3.html) | Not stated in its studies | SubQ vials (research chemical); cell-culture reagent | **SubQ** |
| [Insulin](https://www.kalios.health/compounds/insulin.html) | SubQ, IV, inhaled (people) | SubQ vials and pens (Rx; some OTC); inhaled (Afrezza) | — |
| [Ipamorelin](https://www.kalios.health/compounds/ipamorelin.html) | IV (people) · SubQ, IV (rats) | SubQ vials (research chemical; peptide clinics) | — |
| [Kisspeptin](https://www.kalios.health/compounds/kisspeptin.html) | SubQ, IV (people) · IP (mice) · into the brain (sheep) | SubQ vials (research chemical; some clinics) | — |
| [Klotho](https://www.kalios.health/compounds/klotho.html) | SubQ (monkeys) · IP (mice) · no human study | SubQ or IM vials (alphaKlothoLR, research chemical) | **IM** |
| [KPV](https://www.kalios.health/compounds/kpv.html) | Oral (mice) · no human study | SubQ/oral/nasal vials (research); topical creams (cosmetic) | **SubQ, intranasal, topical** |
| [Livagen](https://www.kalios.health/compounds/livagen.html) | Not stated in its studies | Oral capsules (Russian OTC); SubQ vials (research chemical) | **SubQ, oral, sublingual** |
| [LL-37](https://www.kalios.health/compounds/ll-37.html) | Topical (people and animals) | SubQ vials (research chemical) | **SubQ** |
| [MariTide](https://www.kalios.health/compounds/maritide.html) | SubQ (people) | Trials only; lab reagent (not for human use) | — |
| [Melanotan I](https://www.kalios.health/compounds/melanotan-i.html) | SubQ, oral, transdermal, implant (people) | SubQ implant (Scenesse, Rx); SubQ vials (research chemical) | — |
| [Melanotan II](https://www.kalios.health/compounds/melanotan-ii.html) | SubQ (people) · IV, intrathecal, into the brain (rats) | SubQ vials (research chemical) | — |
| [Methylene Blue](https://www.kalios.health/compounds/methylene-blue.html) | IV, oral (people) · IV, IP (rats) | IV solution (ProvayBlue); oral, sublingual (compounded) | **sublingual** |
| [MGF](https://www.kalios.health/compounds/mgf.html) | IM (mice) · no human study | IM vials, SubQ PEG-MGF vials (research chemical) | **SubQ** |
| [MOTS-c](https://www.kalios.health/compounds/mots-c.html) | IP (mice) · no human study | SubQ vials (research chemical) | **SubQ** |
| [N-Acetyl Selank](https://www.kalios.health/compounds/n-acetyl-selank.html) | No study of it | Powder vials for nasal spray (research chemical) | **intranasal** |
| [N-Acetyl Semax](https://www.kalios.health/compounds/n-acetyl-semax.html) | No study of it | Powder vials for nasal spray (research chemical) | **intranasal** |
| [N-Acetyl-Epithalon](https://www.kalios.health/compounds/n-acetyl-epithalon.html) | No study of it | SubQ vials, some nasal sprays (research chemical) | **SubQ, intranasal** |
| [NAD+](https://www.kalios.health/compounds/nad-plus.html) | IV (one human pilot study) | IV infusion (clinics); SubQ, IM, nasal (compounded) | **SubQ, IM, intranasal** |
| [Nesfatin-1](https://www.kalios.health/compounds/nesfatin-1.html) | Into the brain (rats) · IV (mice) · no human study | Powder vials (lab reagent, research only) | — |
| [Neurokinin A](https://www.kalios.health/compounds/neurokinin-a.html) | IV, inhaled (people) | Powder vials (lab reagent, research only) | — |
| [Nonapeptide-1](https://www.kalios.health/compounds/nonapeptide-1.html) | Cells only · no animal or human study | Topical creams and serums (cosmetic) | **topical** |
| [Noopept](https://www.kalios.health/compounds/noopept.html) | Oral, intranasal (people) · IV, IP, oral (animals) | Oral tablets (OTC in Russia); capsules, powder (supplement) | — |
| [Ovagen](https://www.kalios.health/compounds/ovagen.html) | No study of it | Oral capsules (supplement); SubQ vials (research chemical) | **SubQ, oral** |
| [Oxytocin](https://www.kalios.health/compounds/oxytocin.html) | IM, IV, intranasal (people) · SubQ, into the brain (animals) | IV/IM ampoules (Pitocin); nasal spray (compounded) | — |
| [P21](https://www.kalios.health/compounds/p21.html) | Oral (mice and rats) · no human study | Powder vials for SubQ, nasal or oral use (research chemical) | **SubQ, intranasal** |
| [Pal-AHK](https://www.kalios.health/compounds/pal-ahk.html) | No study of it | Topical scalp serums and creams (cosmetic) | **topical** |
| [Palmitoyl Dipeptide-6](https://www.kalios.health/compounds/palmitoyl-dipeptide-6.html) | No study of it | Topical serums and firming creams (cosmetic) | **topical** |
| [Pancragen](https://www.kalios.health/compounds/pancragen.html) | Cells only · no animal or human study | Oral/sublingual (supplement); SubQ vials (research chemical) | **SubQ, oral, sublingual** |
| [PEG-MGF](https://www.kalios.health/compounds/peg-mgf.html) | Cells only · no animal or human study | SubQ vials, some used IM (research chemical) | **SubQ, IM** |
| [Pemvidutide](https://www.kalios.health/compounds/pemvidutide.html) | SubQ (people) | Not sold: trials only | — |
| [Pentadeca Arginate](https://www.kalios.health/compounds/pentadeca-arginate.html) | No study of it | SubQ vials, capsules, nasal spray, cream (clinics) | **SubQ, oral, intranasal, topical** |
| [Petrelintide](https://www.kalios.health/compounds/petrelintide.html) | SubQ, IV (people; IV in one Phase 1 cohort) | Trials only; one research-chemical listing | — |
| [Pinealon](https://www.kalios.health/compounds/pinealon.html) | Not stated in its studies | Powder vials for SubQ, nasal or oral use (research chemical) | **SubQ, oral, intranasal** |
| [Selank](https://www.kalios.health/compounds/selank.html) | IP, intranasal (rats) · human route not stated | Nasal spray (Rx in Russia); powder vials (research chemical) | **SubQ** |
| [Semax](https://www.kalios.health/compounds/semax.html) | Intranasal (people) · IV, intranasal (rats) | Nasal spray (Rx in Russia); powder vials (research chemical) | — |
| [Sermorelin](https://www.kalios.health/compounds/sermorelin.html) | SubQ, IV (people) | SubQ vials (compounded Rx); Geref brand discontinued | — |
| [SLU-PP-332](https://www.kalios.health/compounds/slu-pp-332.html) | IP (mice) · no human study | Oral capsules, powder, rare injectables (research chemical) | **oral, injection** |
| [SR9009](https://www.kalios.health/compounds/sr9009.html) | IP (mice) · no human study | Oral capsules (supplement, research chemical) | **oral** |
| [SS-31](https://www.kalios.health/compounds/ss-31.html) | SubQ, IV (people) · SubQ (dogs) | SubQ vials (Forzinity); powder vials (research chemical) | — |
| [TB-500](https://www.kalios.health/compounds/tb-500.html) | SubQ (horses) · no human study | SubQ or IM powder vials (research chemical) | **IM** |
| [Tesofensine](https://www.kalios.health/compounds/tesofensine.html) | Oral (people) · SubQ (rats) | Oral capsules (research chemical, non-US compounded); powder | — |
| [Testagen](https://www.kalios.health/compounds/testagen.html) | Not stated in its studies | Oral capsules (supplement in Russia); SubQ vials (research) | **SubQ, oral** |
| [Testosterone](https://www.kalios.health/compounds/testosterone.html) | SubQ, IM, transdermal (people) | IM/SubQ vials, gels, pellets, oral (Rx, some compounded) | **oral, implant** |
| [Thymagen](https://www.kalios.health/compounds/thymagen.html) | SubQ (rats) · human route not stated | Oral capsules, nasal spray (Rx in Russia); vials (research) | **IM, oral, intranasal** |
| [Thymalin](https://www.kalios.health/compounds/thymalin.html) | IM (people) | IM/SubQ vials (Rx in Russia); powder vials (research) | **SubQ** |
| [Thymulin](https://www.kalios.health/compounds/thymulin.html) | Not stated in its studies | SubQ/IM powder vials (research chemical) | **SubQ, IM** |
| [Tripeptide-29](https://www.kalios.health/compounds/tripeptide-29.html) | No study of it | Topical serums and creams (cosmetic); raw powder (research) | **topical** |
| [Triptorelin](https://www.kalios.health/compounds/triptorelin.html) | IM (people) | IM depot (Trelstar, Decapeptyl); SubQ ampoules (Rx) | **SubQ** |
| [Vesugen](https://www.kalios.health/compounds/vesugen.html) | Not stated in its studies | Oral/sublingual capsules (supplement); vials (research) | **SubQ, oral, sublingual** |
| [Vilon](https://www.kalios.health/compounds/vilon.html) | Not stated in its studies | Oral/sublingual capsules (supplement); vials (research) | **SubQ, oral, sublingual** |
| [VIP](https://www.kalios.health/compounds/vip.html) | IV, intranasal, inhaled (people) | Nasal spray (compounded Rx); powder vials (research) | — |
| [VK2735](https://www.kalios.health/compounds/vk2735.html) | SubQ, oral (people) | Trials only; SubQ and oral products (grey market) | — |
| [Waglerin-1](https://www.kalios.health/compounds/waglerin-1.html) | IP (mice) · no human study | Powder vials (lab reagent, research only) | — |
| [Zuclomiphene](https://www.kalios.health/compounds/zuclomiphene.html) | Oral (people and mice) | Trials only (oral capsules); powder (reference standard) | — |

The other 36 compounds were sold by the same route(s) their cited studies used: Adipotide, AICAR, Cagrilintide, CagriSema, Cardarine, Cerebrolysin, CJC-1295, Clomiphene, Decapeptide-12, Dulaglutide, Enclomiphene Citrate, Epithalon, GW-0742, HCG, HGH, Liraglutide, Matrixyl, Mazdutide, MK-677, Orforglipron, Pal-GHK, Pentapeptide-18, Pramlintide, PT-141, Retatrutide, Rigin, Semaglutide, Setmelanotide, SNAP-8, Survodutide, Syn-Ake, Syn-Coll, Tesamorelin, Thymosin Alpha-1, Tirzepatide, Vialox.


### The six stacks

| Stack | Route (studied) | Route (sold) |
|---|---|---|
| [Wolverine Stack](https://www.kalios.health/compounds/wolverine-stack.html) | IP (rats; the one study of the pair) | SubQ: a 1:1 pre-blend vial or two vials (research chemical) |
| [GLOW Stack](https://www.kalios.health/compounds/glow-stack.html) | No study of the combination | SubQ: a 70 mg pre-blend vial (research chemical) |
| [KLOW Stack](https://www.kalios.health/compounds/klow-stack.html) | No study of the combination | SubQ: an 80 mg pre-blend vial (research chemical) |
| [GH Stack](https://www.kalios.health/compounds/gh-stack.html) | No study of the pair | SubQ: two vials, or a 1:1 pre-blend (research chemical) |
| [Mito Stack](https://www.kalios.health/compounds/mito-stack.html) | No study of the combination | Three separate SubQ products; NAD+ also IV at clinics |
| [Flow State](https://www.kalios.health/compounds/flow-state-stack.html) | No study of the combination | Separate vials: nasal; Epitalon SubQ or nasal |

### Stamp lines: where G's line isn't true of a compound

| Compound | Stamp | Its own line |
|---|---|---|
| AHK-Cu | RESEARCH ONLY | No FDA ruling: not on any list. Sold as a cosmetic ingredient in creams and serums. |
| Amycretin | RESEARCH ONLY | No FDA ruling: not on any list. Not sold: trial drug only. |
| ARA-290 | RESEARCH ONLY | No FDA ruling: on its 503A Category 3 list, nominated without enough data to review. Sold as a research chemical, labeled not for human use. |
| Argireline | RESEARCH ONLY | No FDA ruling: not on any list. Sold as a cosmetic ingredient in creams and serums. |
| Decapeptide-12 | RESEARCH ONLY | No FDA ruling: not on any list. Sold as a cosmetic ingredient in creams and serums. |
| Eloralintide | RESEARCH ONLY | No FDA ruling: not on any list. Not sold: trial drug only. |
| Flow State | NOT LEGAL TO COMPOUND | At least one compound in it isn’t legal to compound. |
| GH Stack | NOT LEGAL TO COMPOUND | At least one compound in it isn’t legal to compound. |
| GHRP-2 | RESEARCH ONLY | No FDA ruling: on its 503A Category 3 list, nominated without enough data to review. Sold as a research chemical, labeled not for human use. |
| GHRP-6 | RESEARCH ONLY | No FDA ruling: on its 503A Category 3 list, nominated without enough data to review. Sold as a research chemical, labeled not for human use. |
| GLOW Stack | NOT LEGAL TO COMPOUND | At least one compound in it isn’t legal to compound. |
| Glumitide | RESEARCH ONLY | No FDA ruling: not on any list. Not sold: trial drug only. |
| Gonadorelin | COMPOUNDING PHARMACY | The active ingredient of a discontinued FDA-approved drug — a licensed pharmacy can compound it for you. |
| Kisspeptin | RESEARCH ONLY | FDA said no: on its 503A Category 2 list (significant safety risks). Sold anyway as a research chemical. |
| KLOW Stack | NOT LEGAL TO COMPOUND | At least one compound in it isn’t legal to compound. |
| Matrixyl | RESEARCH ONLY | No FDA ruling: not on any list. Sold as a cosmetic ingredient in creams and serums. |
| MGF | RESEARCH ONLY | No FDA ruling: on its 503A Category 3 list, nominated without enough data to review. Sold as a research chemical, labeled not for human use. |
| Mito Stack | NOT LEGAL TO COMPOUND | At least one compound in it isn’t legal to compound. |
| MK-677 | RESEARCH ONLY | FDA said no: on its 503A Category 2 list (significant safety risks). Sold anyway as a research chemical. |
| Nonapeptide-1 | RESEARCH ONLY | No FDA ruling: not on any list. Sold as a cosmetic ingredient in creams and serums. |
| Noopept | RESEARCH ONLY | Not on any FDA list. Sold in the US as a supplement; FDA’s warning letters call it an unapproved drug. |
| Pal-AHK | RESEARCH ONLY | No FDA ruling: not on any list. Sold as a cosmetic ingredient in creams and serums. |
| Pal-GHK | RESEARCH ONLY | No FDA ruling: not on any list. Sold as a cosmetic ingredient in creams and serums. |
| Palmitoyl Dipeptide-6 | RESEARCH ONLY | No FDA ruling: not on any list. Sold as a cosmetic ingredient in creams and serums. |
| Pemvidutide | RESEARCH ONLY | No FDA ruling: not on any list. Not sold: trial drug only. |
| Pentadeca Arginate | RESEARCH ONLY | No FDA ruling: not on any list. Sold by clinics, which say compounding pharmacies make it. |
| Pentapeptide-18 | RESEARCH ONLY | No FDA ruling: not on any list. Sold as a cosmetic ingredient in creams and serums. |
| Rigin | RESEARCH ONLY | No FDA ruling: not on any list. Sold as a cosmetic ingredient in creams and serums. |
| Sermorelin | COMPOUNDING PHARMACY | The active ingredient of a discontinued FDA-approved drug — a licensed pharmacy can compound it for you. |
| SNAP-8 | RESEARCH ONLY | No FDA ruling: not on any list. Sold as a cosmetic ingredient in creams and serums. |
| Syn-Ake | RESEARCH ONLY | No FDA ruling: not on any list. Sold as a cosmetic ingredient in creams and serums. |
| Syn-Coll | RESEARCH ONLY | No FDA ruling: not on any list. Sold as a cosmetic ingredient in creams and serums. |
| Thymulin | RESEARCH ONLY | No FDA ruling: on its 503A Category 3 list, nominated without enough data to review. Sold as a research chemical, labeled not for human use. |
| Tripeptide-29 | RESEARCH ONLY | No FDA ruling: not on any list. Sold as a cosmetic ingredient in creams and serums. |
| Vialox | RESEARCH ONLY | No FDA ruling: not on any list. Sold as a cosmetic ingredient in creams and serums. |
| Wolverine Stack | NOT LEGAL TO COMPOUND | At least one compound in it isn’t legal to compound. |
