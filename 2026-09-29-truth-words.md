# 12a — Truth, words and the calculator ship — 2026-09-29

All six parts of the 12a brief (0–5) are done and live on www.kalios.health.

- **Production:** `dpl_3693prZChU7DCiqFKpp2Y2m5nHZi`, state READY, target production. It was built from commit `de295e58` through the git-less copy (CLAUDE.md rule 4), and every live check passes on it (see Checks).
  - It's the fourth production deploy tonight. The first, `dpl_41CZJ1mtRdCjie9shyjiAEys7oi7`, carried Parts 0–4.
  - The live checks then found three problems. Each fix went out as its own deploy: `dpl_AAipccg3z8kJeg1cK3RcmYXTAJSA`, `dpl_GVBWwAqyZZ1qSWQ2n1cvr9md7pgu`, then this one.
- **IndexNow:** 447 URLs submitted, HTTP 200, after the final deploy. It also ran after the two deploys before it.
- **Commits:** 35, all pushed; this report is the 36th.
  - Part 0 (13): `298d14a8` 0.1 · `bbf3383f` 0.2 · `e9eba1b2` 0.3 · `2d451217` 0.4 · `b90eebd7` 0.5 · `35fa1878` 0.6 · `254db504` 0.7 · `cd46dbeb` + `6cc9eba5` 0.8 · `a3eb8bb4` 0.9 · `b19fe13e` 0.10 · `81972a0e` 0.11 · `33124c67` 0.12
  - Part 1: `1be3f87b`
  - Part 2: `093e3050`
  - Part 3 (16): `47e2f4ff` a · `ef37da23` b · `8023590c` c · `15374460` d · `95d62d3b` e · `e7a6f7fc` f · `b7d10986` g · `6ea12221` h · `061bb105` i · `39f84b03` j · `ddbc26b9` k · `30af53ff`, `bbedebfe`, `f85e68b9` l · `d6c17a52` m · `05464f8a` Semax (extra)
  - Part 4: `c03b16a3`
  - Part 5 (3 fixes): `031c2d38` · `1019e275` · `de295e58`
- **Scale:** 512 files changed (+61,678 / −14,885).
  - 12 added, 16 deleted, 483 modified, and 1 moved (`stacks/index.html` → `stack.html`).
  - About 44,600 of the added lines are the committed citation cache (35,531) and the audit CSV (9,052).

**Session start (rule 2)**
- Claude Code 2.1.284.
- Vercel CLI 60.1.3, the same as the latest on npm.
- `brew outdated`: 38 formulae and 1 cask (`visual-studio-code`). None were updated.
- No new packages. The test tooling (puppeteer-core) stayed in the session scratchpad.

---

## Look at these first

1. **GHK-Cu's band, in your words, says something FDA's record doesn't.**
   - The title (Part 0.11) reads "Injectable GHK-Cu: not legal to compound — yet". The sentence (11c) reads "Injectable GHK-Cu is scheduled for committee review before the end of February 2027."
   - FDA's 503A categories list (updated May 14, 2026) says:
     - GHK-Cu "except for injectable routes of administration" went back into Category 1, because a nominator "intended to withdraw only its nomination of the injectable route".
     - FDA intends to consult the PCAC on GHK-Cu before the end of February 2027. Our own tracker lists that review as "GHK-Cu (noninjectable routes only)".
   - So no injectable nomination is pending. "— yet" and "scheduled for committee review" point to a review FDA hasn't announced for injectables.
   - Both lines are kept verbatim (Open question 1).
2. **Your five example hooks don't all follow the formula they illustrate.** They were used verbatim, as the brief asked.
   - Three have no number: Adipotide, Ipamorelin and Semaglutide ("The one …" isn't a count).
   - Two state the claim as fact:
     - Adipotide: "Kills fat cells by starving their blood supply."
     - Ipamorelin: "pulses without the cortisol mess". The page's cortisol data come from pigs.
   - Semaglutide, "The one that actually went through the FDA": four other fat-loss compounds on the site are FDA-approved for weight (liraglutide, tirzepatide, setmelanotide, orforglipron).
   - BPC-157, "544 rat papers": the page says "544 papers. Almost all rats."
   - Cerebrolysin's example matches its page (200 trials, 44-country approval).
   - Open question 2.
3. **The old calculator's FAQ didn't survive the ship.**
   - The page it replaced had six questions with FAQPage markup, for example "How do I calculate peptide dosage?" and "How much BAC water for 5mg BPC-157?".
   - The rebuild never had them. Open question 3.
4. **Some counts differ from the brief.** TB-500 Fragment folded into TB-500 (3a) before the calculator ship (Part 4), so:
   - 113 compounds and 124 files in `compounds/` (the brief says 114 and 125)
   - 115 section-13 links (brief: 116)
   - 125 re-pointed calculator links (brief: 126)
   - The hooks were written before the fold: 120 of them, 119 now live.
5. **The compounding timeline doesn't end in February 2025.**
   - The brief says "telehealth timeline to Feb 2025".
   - FDA's grace periods ended March 5 / March 19, 2025 for tirzepatide and April 24 / May 22, 2025 for semaglutide.
   - So the compare page says the channel ran to spring 2025 (3i).
6. **Semax had its two strengths swapped, a tenfold error per drop.** It wasn't in the brief; I found it while checking and fixed it (`05464f8a`).
7. **Eleven trial IDs on compound pages pointed to unrelated studies.**
   - I found them while building the Pipeline row, and all are fixed (3l).
   - BPC-157 has no registered Phase II in ulcerative colitis; the page had cited a dental study.
8. **Three fixes after the first deploy (Part 5).** The live checks found all three.
   - The five removed pair pages' trailing-slash addresses returned 404. They now 301.
   - The seven TB-500 pair pages cut TB-500's FDA status to "…PCAC voted 8-6 to r…", because the box holds 60 characters.
   - My 3l label for the enclomiphene Phase 3 trials said ZA-301 / ZA-302.
     - The registry and Kim 2016 both say ZA-304 / ZA-305; the NCT numbers were right.
     - The 3l corrections entry now reads ZA-304/ZA-305.
9. **40 citations were left as they were and listed** (Part 1, table below). Several look invented: an ID that opens an unrelated paper, or no PubMed record at all.
10. **Part 0.8 is two commits.** `cd46dbeb` holds only the file move, because a staging command failed on a path. `6cc9eba5` has the rest.

---

## What was done

**Part 0 — nits**

- **0.1 Homepage search** (`298d14a8`)
  - A Kalios typeahead replaces the browser datalist: dark, mono, nothing until you type, then the best six matches across name, trade name and alias.
  - Arrow keys and Enter work; Escape closes; there's no ▼.
  - Placeholder: "BPC-157, Retatrutide, GHK-Cu…".
  - Every input gets a 2 px `--ink` focus ring, never `--caution`.
- **0.2 Homepage chips** (`bbf3383f`)
  - Stacks plus the nine goal groups, in the same words and with the same muted counts as /compounds/. One scrolling row on phones.
  - "New here" is now a line under the sub-line: "First time? Start here →".
- **0.3 Nine groups** (`e9eba1b2`)
  - Each compound has a `use_tag` plus up to two `secondary_tags`, and its card appears in every group it carries.
  - Within a group, cards run by evidence level, highest first, then by name. Assignments are under Tags.
- **0.4 "What changed" line** (`2d451217`)
  - It shows content and status events only.
  - Today's reads "28 Sep — FLGR-242 added; follistatin-344 corrected".
- **0.5 Footer** (`b90eebd7`): the disclaimer runs full width above the three clauses.
- **0.6 Compound pages** (`35fa1878`), on the 123 compound and stack pages:
  - The body Gist box is "The other four questions".
  - Section rows and headings now agree.
  - The data table shows 8 rows, then a mono "Show all N". Every row prints, and every row shows without JavaScript.
  - "Aa" sits at the end of the last row: References · N ··· Aa.
- **0.7 Card dot** (`254db504`): the dot shows only for 7 days after a content change. The date text stays.
- **0.8 Stacks** (`cd46dbeb`, `6cc9eba5`)
  - The nav says "Stacks", and /stack.html is now a hub:
    - the six stacks as cards, with their stamps
    - then "Look up any two — we show what's published; usually nothing on the combination itself, and we say so."
    - then two pickers; the result renders once both are chosen.
  - The Generate button and the headline question are gone.
- **0.9 /alerts.html** (`a3eb8bb4`)
  - What triggers an email, the next known event, the form (the same Substack endpoint), and "No newsletter. No opinions. No vendors."
  - The nav's "Alerts" goes there.
- **0.10 Beginner's guide** (`b19fe13e`)
  - A five-line Gist at the top.
  - "How to read a Kalios card": the meter, stamps and red flag, as the cards look now.
  - The 503A sentence is fixed: BPC-157 and ipamorelin can't be compounded today; sermorelin can.
  - It ends with four doors: goal chips, calculator, tracker, alerts.
- **0.11 GHK-Cu band title** (`81972a0e`): your words (see Look at these first, 1).
- **0.12 /corrections.html** (`33124c67`)
  - One line per correction: date, page, what was wrong, what it says now.
  - It's generated from `data/changelog.json` and linked from every footer as "Corrections". It holds 150 entries.

**Part 1 — citations** (`1be3f87b`)
- Every PubMed ID, PMC ID and DOI on every page was resolved. Its title, first author and year were compared with the reference text or the sentence citing it.
- About 2,200 links that opened a different paper now open the one named.
- Follistatin-344's six PMIDs, the swapped Mendell trials and the NEJM wording are fixed.
- `scripts/audit_pages.py --mode cite` now fails any page with a dead or wrong ID. It's in the deploy checklist.
- Numbers and the listed 40 are under Citation audit.

**Part 2 — cards and dosing** (`093e3050`)
- **120 hooks rewritten.** All are ≤ 110 characters and two sentences, with the numbers taken from each page's own text. They're under Read aloud, each next to the old one.
- **"Dosing from the literature" moves up.** On all 123 compound and stack pages, including the six stacks, it now sits directly after the Gist.
  - It opens with one line: "Published for {use}: … Not published: …".
  - The calculator link sits beneath it.

**Part 3 — truth follow-ups**: 16 commits, one per item. Each correction is on /corrections.html; sources are under Truth fixes.

**Part 4 — calculator ship** (`c03b16a3`)
- **Promotion.** The rebuild is now /calculator.html. /calc-next.html and /calc-next return a 308 to it. The embed, the service worker and the sitemap entry stay.
- **Placeholders.** The vial field reads "e.g. 10"; water stays "e.g. 2". The embed's narrow field now fits "your amount".
- **U-40.** It's no longer a top-level toggle. The picker shows U-100 only (30/50/100). A small mono link, "Using a syringe marked 40 units? →", reveals U-40 with your line.
- **Links.** Section 13 on 115 pages, and all 125 re-pointed links, read "→ Peptide Calculator — vial-to-syringe math".
- **Paper inputs.** The research-only alert form and the Stacks pickers are on paper.
- **Gate:** `bash tests/run-calc-tests.sh` PASS.

**Part 5 — deploy and report**
- Deployed per the checklist: prerender twice, the gates, the git-less copy, `--prod`, IndexNow, curl.
- Three fixes followed (Look at these first, 8), each committed and deployed on its own.
- This report.

---

## Citation audit (Part 1)

The full list is in `gsc-export/citation-audit.csv`, one row per occurrence, with the old ID in `fixed_from`.

- **Scope:** reference lists, literature tables and inline text on every page.
  - 9,033 occurrences on 447 pages.
  - 1,300 distinct IDs: 7,651 PMID, 1,122 DOI and 260 PMC occurrences.
- **First pass, before any fix:** 9,184 occurrences.
  - 6,431 matched.
  - 2,461 opened a different paper.
  - 175 needed a closer look.
  - 117 didn't resolve.
- **After the fixes:** 9,033 occurrences. There are fewer because the rebuilt pair tables drop a row that repeats a paper already listed for the other compound.

  | Result | Occurrences |
  |---|---|
  | Matches | 8,771 |
  | Matches, checked by hand (the automatic check can't see the title or author in the citing text) | 118 |
  | Wrong paper, left and listed (no paper matching the reference could be identified) | 133 |
  | Unresolvable, left and listed | 11 |

- **Links replaced:** 2,234 wrong IDs came off 405 pages.
  - 680 were on 109 compound, stack and other pages.
  - 1,554 were in the literature tables of 296 pair pages, rebuilt from the corrected `stacks/data.json`.
  - The CSV's `fixed_from` marks 2,547 rows. That count is higher because it also counts IDs that were shuffled between references on the same page.
- **Beyond the links:**
  - Wrong author lists and journal names on the right paper were corrected.
  - Descriptions on five pages were rewritten to match the paper they cite:
    - thymosin α1: Poo 2008, Chien 1998, Zhang 2009
    - DSIP: the 2006 "unresolved riddle" review
    - NAD+: Orr 2024
    - tesofensine's expression of concern
    - waglerin-1's 1991 isolation
- **Corrections entries:** 115, one per page fixed.
- **The gate today:** 8,957 citations on 442 pages; 0 failing, 0 to review, 262 allowed with a reason.
  - `data/citation-allow.json` holds 231 entries: 97 hand-checked false alarms and the 40 listed IDs on their 134 page entries.
  - The gate's count differs from the CSV's because Part 3 changed pages after the CSV was written: TB-500 Fragment's six pages are gone, and TB-500's references were rewritten.

**The 40 listed citations** (left as they were; each needs a person to find the right paper or remove it)

| ID | Result | Where | What the page cites | What the ID opens |
|---|---|---|---|---|
| doi:10.3389/fphar.2021.716947 | unresolvable | thymalin, 8 pair pages | Khavinson VK, Linkova NS, Kvetnoy IM, Kvetnaia TV, Polyakova VO, Ashapkin VV. The Use of Thymalin for Immunoco… | doi.org HTTP 404 |
| PMID 15953974 | unresolvable | ovagen | Chalisova NI, Khavinson VKh. Neuroendocrine regulation of organotypic culture: tropic effect of short peptides… | cannot get document summary |
| PMID 26260367 | unresolvable | humanin | MDPs are retrograde mitochondrial-to-cell signals that communicate mitochondrial functional status to the rest… | cannot get document summary |
| PMC8996222 | wrong paper | dihexa, 6 pair pages | Stem Cell Research & Therapy. Dihexa as adjunct in peripheral nerve repair. 2022 Apr 11;13(1):159. PMID: 35410… | Pan T 2022: Efficiently generate functional hepatic cells from human pluripotent… |
| PMID 10358599 | wrong paper | adamax | Myasoedov NF, Skvortsova VI, Nasonov EL, Zhuravleva EY, Grivennikov IA, Arsenyeva EL, Sukhanov IV. Studies of … | Koh A 1999: Racism: time to turn words into action. |
| PMID 11396112 | wrong paper | bromantane, 3 pair pages | Morozov IS, Ivanova IA, Sergeeva SA, Petrov VI. Bromantan - a new immunostimulant and psychostimulant of the a… | Thabet F 2001: [Medullary arteriovenous malformations in children]. |
| PMID 11875579 | wrong paper | ovagen, 1 pair page | Khavinson VKh, Yakovleva NG, Popuchiev VV, Kvetnoi IM, Manokhina RP. Reparative effect of epithalon on ovarian… | Collins-Nakai RL 2002: Ask not what you can do for us, but what we can do for yo… |
| PMID 12243262 | wrong paper | adamax | Levitskaya NG, Sebentsova EA, Andreeva LA, Alfeeva LY, Kamenskii AA, Myasoedov NF. Comparative study of the ph… | Plakhova VB 2002: A possible molecular mechanism for the interaction of defensin… |
| PMID 12459869 | wrong paper | bromantane, 3 pair pages | Yakimovskii AF, Varshavskaya VM. Experimental analysis of psychopharmacological activity of bromantane. Bull E… | Kolganova TV 2002: Combined effect of low-frequency laser and polyphenol oxidase… |
| PMID 12820081 | wrong paper | livagen, 2 pair pages | Khavinson VKh, Malinin VV, Myl'nikov SV. Effect of peptides on lifespan of animals and peptide-dependent gene … | Berend KR 2003: Hip arthroplasty after failed free vascularized fibular grafting… |
| PMID 15240655 | wrong paper | triptorelin | Central precocious puberty — Carel et al., J Clin Endocrinol Metab 2004 (PMID 15240655) — long-acting triptore… | Villarino AV 2004: Understanding the pro- and anti-inflammatory properties of IL… |
| PMID 15668064 | wrong paper | humanin | Alzheimer's mouse models (Tajima et al., Neurosci Lett 2005; PMID 15668064) — Intracerebroventricular HNG impr… | Harper CM 2005: Preoperative and intraoperative electrophysiologic assessment of… |
| PMID 16180321 | wrong paper | pramlintide, 2 pair pages | Young A. Amylin: Physiology and Pharmacology. Adv Pharmacol. 2005;52:1-272. PMID: 16180321. (Comprehensive pha… | Rodríguez Tolrá J 2005: [Incomplete urethral duplication]. |
| PMID 16927610 | wrong paper | adamax | Hippocampal BDNF / NGF upregulation — Semax treatment rapidly increases BDNF and NGF mRNA and protein in rat h… | Hollis A 2006: How parallel trade affects drug policies and prices in Canada and… |
| PMID 16962082 | wrong paper | n-acetyl-semax | Cerebral ischemia gene expression — Medvedeva, Dergunova, Stavchansky, and colleagues have published a series … | Geva R 2006: Memory functions of children born with asymmetric intrauterine grow… |
| PMID 18651609 | wrong paper | bromantane, 3 pair pages | Voronina TA, Molodavkin GM, Sergeeva SA, Borliakov LM. Analysis of the neurophysiological mechanisms of anxiol… | Li F 2008: Determination of dehydrodiisoeugenol in rat tissues using HPLC method… |
| PMID 19888452 | wrong paper | humanin | Muzumdar RH, Huffman DM, Atzmon G, Buettner C, Cobb LJ, Fishman S, Budagov T, Cui L, Einstein FH, Poduval A, H… | Roland CL 2009: Cytokine levels correlate with immune cell infiltration after an… |
| PMID 20211192 | wrong paper | waglerin-1, 1 pair page | Verma V, Kala N, Kar P, Singh M, Patel S, et al. Conformational analysis of a toxic peptide from Trimeresurus … | Thyagarajan B 2010: Effects of hydroxamate metalloendoprotease inhibitors on bot… |
| PMID 22585422 | wrong paper | n-acetyl-semax | Cerebral ischemia gene expression — Medvedeva, Dergunova, Stavchansky, and colleagues have published a series … | Davis RW 2012: The marine mammal dive response is exercise modulated to maximize… |
| PMID 22808512 | wrong paper | testagen, 3 pair pages | Khavinson VK, Solovyev AY, Zhilinskiy DV, Shataeva LK, Vanyushin BF. Effect of peptides on the proliferative a… | Alexandrova NV 2012: Markers of transmembrane and energy exchange in cells of te… |
| PMID 23289233 | wrong paper | bronchogen, cardiogen, cartalax +2, 22 pair pages | Khavinson VKh, Solov'ev AIu, Zhilinskii DV. Molecular mechanism of the peptide regulation of gene expression: … | Vlasova MM 2012: [Medical irradiation and health. Communication 1. Health of the… |
| PMID 23998781 | wrong paper | thymosin-alpha-1 | Tuthill CW, Rios I, Lemesre JL. Thymosin alpha 1 — a peptide immune modulator with a broad range of clinical a… | Lee JH 2013: Monitoring by LC-MS/MS of 48 compounds of sildenafil, tadalafil, va… |
| PMID 24127760 | wrong paper | hcg | Kaminetsky et al., BJU Int 2014 (PMID 24127760) — RCT of enclomiphene citrate in obese hypogonadal men demonst… | Bush HM 2013: Failing to fail: clinicians' experience of assessing underperformi… |
| PMID 25028537 | wrong paper | p21, 3 pair pages | Blanchard J, Wanka L, Tung YC, Cárdenas-Aguayo Mdel C, LaFerla FM, Iqbal K, Grundke-Iqbal I. Pharmacokinetics … | Correale J 2014: Assessing the potential impact of non-proprietary drug copies o… |
| PMID 26705446 | wrong paper | compare/bpc-157-vs-tb-500-vs-ghk-cu | Nestor MS, Berman B, Swenson N. Safety and Efficacy of Oral Enzymatically Derived Glycine Peptide in Reducing … | Nguyen TA 2015: Imaging Pediatric Vascular Lesions. |
| PMID 28380907 | wrong paper | humanin | Mitochondrial retrograde signaling — The broader conceptual role of humanin (shared with MOTS-c and SHLPs) is … | Morrison J 2017: Tuning the resonance frequencies and mode shapes in a large ran… |
| PMID 2957300 | wrong paper | thymosin-alpha-1 | Shen SY, Josselson J, Sadler JH, Goldstein AL, Nayak SK, Goldstein G. Effects of thymosin α1 on peripheral T-c… | Crossley B 1987: Does nodular lymphocyte predominant Hodgkin's disease arise fro… |
| PMID 30271090 | wrong paper | methylene-blue, 2 pair pages | Gauthier S, Feldman HH, Schneider LS, Wilcock GK, Wischik CM. Rember® and LMTM in Alzheimer's disease. Lancet … | Ding JW 2018: The Effects of High Mobility Group Box-1 Protein on Peripheral Tre… |
| PMID 30796687 | wrong paper | humanin | Humanin and metabolic disease (Conte et al., Geroscience 2019; PMID 30796687) — Review of human observational … | Yu WM 2019: Address at the 60(th) Anniversary of Comrade MAO Ze-dong's Instructi… |
| PMID 31657509 | wrong paper | oxytocin | (2020; PMID 31657509) — 4-week randomized controlled crossover of 24 IU intranasal oxytocin QID in adults with… | Lawson EA 2020: The role of oxytocin in regulation of appetitive behaviour, body… |
| PMID 31982580 | wrong paper | ara-290, 3 pair pages | Brines M, Dunne A, van Velzen M, Proto PL, Ostenson CG, Kirk RI, Petropoulos IN, Javed S, Malik RA, Cerami A, … | Sloots JJ 2020: Cardiac and respiration-induced brain deformations in humans qua… |
| PMID 32420127 | wrong paper | triptorelin, 2 pair pages | Lundy SD, Sabanegh ES Jr. Non-classical forms of male infertility: triptorelin in recovery of hypogonadotropic… | Zhu P 2020: Proteomic analysis of oxidative stress response in human umbilical v… |
| PMID 33263832 | wrong paper | testagen, 3 pair pages | Khavinson VK, Linkova NS, Ashapkin VV, Ryzhak GA. Peptide Regulation of Aging: 40 Years of Research. Bull Exp … | Herwig-Carl MC 2020: [An ophthalmopathological view on microsurgical techniques]… |
| PMID 3340595 | wrong paper | hgh-fragment-176-191, 3 pair pages | Salem MA. A possible direct lipolytic effect of growth hormone. Proc Soc Exp Biol Med. 1988;187(1):1-6. PMID: … | Price B 1988: What are nurses like? |
| PMID 33705811 | wrong paper | gonadorelin, 5 pair pages | Kohn TP, Louis MR, Pickett SM, Lindgren MC, Kohn JR, Pastuszak AW, Lipshultz LI. Effects of subcutaneous gonad… | Yin H 2021: Plectin regulates Wnt signaling mediated-skeletal muscle development… |
| PMID 35410439 | wrong paper | dihexa, 6 pair pages | Stem Cell Research & Therapy. Dihexa as adjunct in peripheral nerve repair. 2022 Apr 11;13(1):159. PMID: 35410… | Pan T 2022: Efficiently generate functional hepatic cells from human pluripotent… |
| PMID 35684208 | wrong paper | bronchogen | Kononenko NV, Fedoreyeva LI. Molecular Mechanisms Involved in Regulating Shoot and Root Development of Nicotia… | Al Salameen F 2022: Genetic Diversity of Rhanterium eppaposum Oliv. Populations … |
| PMID 7515280 | wrong paper | igf-1-des, 1 pair page | Sommer A, Maack CA, Spratt SK, Mascarenhas D, Tressel TJ, Rhodes ET, Lee R, Roumas M, Tatsuno GP, Flynn JA, et… | Siegall CB 1994: In vivo activities of acidic fibroblast growth factor-Pseudomon… |
| PMID 9288722 | wrong paper | hexarelin | Acromegaly / refractory GH (Arvat et al., 1997; PMID 9288722) — Hexarelin counteracted hydrocortisone-induced … | Gussoni E 1997: The fate of individual myoblasts after transplantation into musc… |
| PMID 9868709 | wrong paper | selank, 8 pair pages | Seredenin SB, Kozlovskaia MM, Blednov IuA, Kozlovskii II. [Anxiolytic properties of Selank in BALB/c mice with… | Gratz S 1998: Arthroscintigraphy in suspected rotator cuff rupture. |

---

## Truth fixes, with sources

Each is its own commit, and each correction has a line on /corrections.html (34 entries from Part 3).

- **3a TB-500** (`47e2f4ff`; 3 entries)
  - **What changed:**
    - The page now profiles what's sold as TB-500: Ac-LKKTETQ, residues 17–23 of thymosin β4, 889.0 Da.
    - Half-life: not published. WADA: S2.3, named since the 2018 List.
    - Human data: none on the fragment. The parent protein's trials stay, labelled as the parent's.
    - Removed: the 43-aa / 4,921 Da / 2–3-day claims, "vials are nearly always full length", SEER-3 "missed its endpoint", and the horse repair studies.
    - TB-500 Fragment folded in, with 301s.
  - **Sources:**
    - FDA PCAC briefing, "Evaluation of TB-500-related Bulk Drug Substances" (fda.gov/media/193349), and its introduction (fda.gov/media/193342), July 23–24, 2026
    - Esposito 2012 (PMID 22962027); Ho 2012 (PMID 23084823); Kwok 2013 (PMID 23318763); Rahaman 2024 (PMID 38382158)
    - the WADA Prohibited Lists
    - WHO proposed INN "fequesetide" (List 127)
- **3b hCG** (`ef37da23`; 1 entry)
  - **What changed:** hCG can't be compounded under 503A or 503B. Since March 23, 2020 the approved products are licensed biologics. The page no longer says 503A compounding is "limited", no longer says state boards restored it, and no longer lists a compounded multi-dose product.
  - **Sources:**
    - FDA's notice to compounders on the March 23, 2020 transition (names hCG)
    - FDA's list of NDAs deemed BLAs (Pregnyl, Novarel, chorionic gonadotropin, Ovidrel)
    - FDA guidance on mixing, diluting or repackaging biological products (January 2018)
    - FDA's June 2023 untitled letter on bulk hCG
    - FDA's biosimilar product list (no hCG)
- **3c CagriSema** (`8023590c`; 2 entries)
  - **What changed:** "Under FDA review" is now dated: NDA submitted December 2025, decision expected Q4 2026. "Anticipated within 1–2 years" is replaced. Cagrilintide's page follows.
  - **Sources:**
    - Novo Nordisk release, December 18, 2025
    - company announcements 2/2026 and 13/2026 (SEC 6-K)
    - H1 2026 report and Q2 2026 presentation
    - Novo Nordisk release, September 21, 2026 ("A decision is expected in Q4 2026")
    - openFDA Drugs@FDA: no approval as of 2026-09-24
- **3d 503B claims** (`15374460`; 4 entries)
  - **What changed:**
    - Enclomiphene and VIP have no 503B pathway.
    - Sermorelin's 503B route is the Category 1 interim policy.
    - Glutathione's basis is named.
    - Sermorelin's "withdrawn for commercial reasons" is replaced with FDA's own record.
  - **Sources:**
    - FDA 503B bulks list (August 21, 2023)
    - 503B categories (March 21, 2025)
    - 503A categories (May 14, 2026)
    - 503B interim policy guidance (January 2025)
    - FDA drug shortage list
    - 74 FR 23407 (2009) and 78 FR 14095 (2013)
- **3e 5-Amino-1MQ and bromantane** (`95d62d3b`; 2 entries)
  - **What changed:** neither has a 503A basis. The "USP-grade bulk powder framework" claim is gone.
  - **Sources:**
    - 21 CFR 216.23
    - 503A (May 14, 2026) and 503B (March 21, 2025) categories: neither substance appears
    - FDA's significant-safety-risks page (April 22, 2026)
    - Orange Book and Drugs@FDA
    - FDA's GSRS substance registry
    - FDA's 503A page
- **3f Should/must** (`e7a6f7fc`; wording only, no entries)
  - **What changed:** 154 sentences on 79 pages now describe practice instead of instructing. 21 stay as they were:
    - the clinician-referral lines CLAUDE.md requires
    - one conditional "should"
    - two statements of biological necessity
    - melanotan-i's REMS claim (Open question 5)
- **3g Tesamorelin** (`b7d10986`; 1 entry)
  - **What changed:**
    - Egrifta SV: 2 mg vial, 0.5 mL diluent, 1.4 mg = 0.35 mL.
    - Egrifta WR: 11.6 mg vial, 1.3 mL, 8 mg/mL, 1.28 mg = 0.16 mL, 7 days at room temperature. Approved March 25, 2025; available September 5, 2025.
    - The "January 2025" launch date is gone. That month was an SV supply disruption.
  - **Sources:**
    - EGRIFTA SV and EGRIFTA WR prescribing information (DailyMed)
    - FDA approval letter, BLA 022505/S-020
    - Theratechnologies release, September 5, 2025
- **3h Orforglipron** (`6ea12221`; 1 entry)
  - **What changed:** label strengths 0.8, 2.5, 5.5, 9, 14.5 and 17.2 mg. The timeline starts at 0.8 mg, and escalation is described up to 17.2 mg. The ACHIEVE and ATTAIN doses stay, labelled as trial doses.
  - **Source:** Foundayo prescribing information, NDA 220934 (approved April 1, 2026; DailyMed).
- **3i Semaglutide vs tirzepatide** (`061bb105`; 2 entries)
  - **What changed:**
    - FDA, not the manufacturers, made the shortage determinations: tirzepatide on December 19, 2024 (after the October 2024 remand), semaglutide on February 21, 2025.
    - The grace periods run to spring 2025.
  - **Sources:** FDA, "FDA clarifies policies for compounders as national GLP-1 supply begins to stabilize"; FDA's declaratory orders of December 19, 2024 and February 21, 2025.
- **3j PCAC wording** (`39f84b03`; 1 entry)
  - **What changed:** the tracker's summary and JSON-LD, `llms.txt` and the homepage FAQ now say "the PCAC review of 12 peptides (7 voted July 23–24, 2026)".
  - **Sources:** FDA's April 15, 2026 categories notice; the July 23–24, 2026 PCAC questions; FDA's plan for the other five before the end of February 2027.
- **3k Khavinson** (`ddbc26b9`; 5 entries)
  - **What changed:**
    - One description of the cohort on the bioregulator pages: 266 people over 60, 6–8 years.
    - Thymalin and Epithalamin were the gland extracts, not synthetic Epitalon or Vilon. Mortality was 2.0–2.1× lower (Thymalin), 1.6–1.8× (Epithalamin), 2.5× (both) and 4.1× (both yearly for 6 years). No randomization is described.
    - Vilon no longer claims the study.
  - **Sources:** Khavinson and Morozov, Neuro Endocrinol Lett 2003 (PMID 14523363) and Adv Gerontol 2002 (PMID 12577695).
- **3l Trial IDs and the Pipeline row** (`30af53ff`, `bbedebfe`, `f85e68b9`; 11 entries)
  - **Trial IDs:** each now points to the trial the page describes (the old IDs are in the corrections entries):
    - SYNCHRONIZE-1/-2: NCT06066515, NCT06066528
    - VK2735 VENTURE: NCT06068946
    - pemvidutide IMPACT: NCT05989711
    - setmelanotide VENTURE: NCT04966741
    - afamelanotide stroke: NCT04962503
    - adipotide Phase I: NCT01262664
    - enclomiphene ZA-304/ZA-305: NCT01993212, NCT01993225
    - tesamorelin SMART: NCT00257712
    - BPC-157 names its developer's Phase I registration: NCT02637284
  - The same fixes are in `stacks/data.json`, and 63 pair tables were re-rendered.
  - **Pipeline row:** 16 pages carry one.
  - **Source:** ClinicalTrials.gov API v2, each record re-checked on 2026-09-28 and again for this report.
- **3m CLAUDE.md and prerender** (`d6c17a52`)
  - **What changed:** CLAUDE.md gives 113 profiles (plus 3 alias pages) and 124 files. Prerender now stamps page dates before drawing any card, so one run is complete.
  - **Tested** in a copy dated October 5: one run moved KPV's date and card; a second changed nothing.
- **Semax (extra)** (`05464f8a`; 1 entry)
  - **What changed:** 0.1% is 50 µg per drop, for cognitive disorders, post-stroke recovery and TIA. 1% is 500 µg per drop, for acute stroke. The page had them the other way round.
  - **Source:** Vidal (vidal.ru), registrations LP-N(009449)-(RG-RU) and LP-N(010596)-(RG-RU).
- **Part 5 fixes**
  - TB-500's pair-page status now fits its box: "Not on 503A List. FDA proposed no; PCAC 8-6 yes (Jul 2026)" (`1019e275`).
  - The enclomiphene trials are labelled ZA-304 / ZA-305 (`de295e58`). Source: their ClinicalTrials.gov records, and Kim, McCullough and Kaminetsky, BJU Int 2016 (PMID 26496621): "enrolled in the trials (ZA-304 and ZA-305)".

---

## Read aloud: the 120 hooks

Each row gives the new hook, then the old one.
- (G) marks your five examples, used verbatim.
- † TB-500 Fragment's hook was retired with its page in 3a. TB-500's own hook is unchanged since Part 2.
- Glumitide had no entry before; its card fell back to the taxonomy text. The unused "motilin" entry (it has no page) was dropped.

**Stacks (6)**

| Stack | New | Old |
|---|---|---|
| Wolverine Stack | The most-run recovery stack: BPC-157 plus TB-500. Human trials of the pair: zero, after about 15 years. | The foundational tissue-repair stack. Angiogenesis plus actin remodeling for injury recovery. |
| GLOW Stack | Wolverine plus a copper peptide, sold for skin, scars and hair. Human trials of the three together: zero. | Skin, collagen, and soft-tissue repair blend. GHK-Cu gene signaling layered on BPC + TB healing. |
| GH Stack | The growth-hormone pair peptide clinics prescribe most. Trials of the pair itself: zero. | The core GH secretagogue pairing. GHRH + clean GHRP for pulsatile growth hormone release. |
| KLOW Stack | GLOW with an anti-inflammatory peptide added, run as one 80 mg vial. Human trials of the four together: zero. | Broad anti-inflammatory longevity stack combining GHK-Cu, BPC-157, TB-500, and KPV. |
| Mito Stack | Sold as software, hardware and fuel for tired mitochondria. Trials of the three together: zero. | The mitochondrial optimization trio. Software (MOTS-c), hardware (SS-31), and fuel (NAD+). |
| Flow State | The focus-and-sleep stack for when Adderall isn't an option. Trials of the three together: zero. | Focus, calm under load, and sleep-architecture support — blending Russian nootropics with a pineal tetrapeptide. |

**Compounds (114)**

| Compound | New | Old |
|---|---|---|
| 5-Amino-1MQ | Sold as a fat-loss pill for a stubborn metabolism. Every result so far: obese mice, from one lab. | NNMT inhibitor targeting fat cell metabolism. Oral small molecule for body composition. |
| Adipotide (G) | Kills fat cells by starving their blood supply. The monkeys' kidneys objected. | Experimental peptide that destroys fat cells via vascular targeting. EXTREME renal toxicity risk. |
| AHK-Cu | Sold in hair-growth serums as GHK-Cu's scalp cousin. The whole case rests on one 2007 lab paper. | Copper tripeptide for hair follicle stimulation and dermal papilla cell activation. |
| AICAR | Sold as exercise in a pill. The 2008 mice ran 44% farther; no human trial has matched it. | AMPK activator. 44% endurance increase in sedentary mice without exercise. WADA banned. |
| AOD-9604 | Sold as growth hormone's fat-burning fragment. Its biggest trial, 536 people over 24 weeks, missed its goal. | The fat-burning fragment of HGH &mdash; without the muscle-growing or insulin-messing side. Cheap, weak, simple. |
| ARA-290 | Chased by people with small-fiber nerve pain. Two Phase 2 trials, 28 days each; no Phase 3 yet. | Innate repair receptor agonist. Phase II data for neuropathic pain and nerve fiber regeneration. |
| Argireline | Sold as Botox in a jar. FDA researchers found about 0.22% of it reaches living skin. | Topical Botox alternative. SNARE complex inhibition for expression line reduction. |
| BPC-157 (G) | Every stubborn-injury guy keeps one in the fridge. 544 rat papers, three tiny human ones. | The most popular tissue repair peptide. Angiogenesis, gut healing, tendon and ligament recovery. |
| BPC-157 Fragment | Sold as a cheaper cut of BPC-157. Peer-reviewed studies of the fragment itself: zero. | Shorter active-core variants of BPC-157 (GEPPP / GEPPPGK). Preclinical-only. Limited standalone data vs the full peptide. |
| Bromantane | Russian doctors prescribe it for burnout and fatigue. Every trial so far is Russian; WADA banned it in 1997. | Actoprotector. Dopamine upregulation via tyrosine hydroxylase. Anti-fatigue without stimulant crash. |
| Bronchogen | Longevity fans take it in capsule cycles for the lungs. Randomized human trials: zero. | Khavinson tripeptide for respiratory mucosa and bronchial tissue support. |
| Cagrilintide | The other half of Novo's CagriSema combo. Alone it's gray-market; paired, 20.4% weight loss at 68 weeks. | The amylin half of CagriSema — Novo Nordisk's next-gen obesity combo. Phase III: 22–25% weight loss stacked with semaglutide. |
| CagriSema | Novo's two-drug shot built to beat Zepbound. 20.4% weight loss in Phase 3; the FDA hasn't ruled yet. | Amylin + GLP-1 combination. Phase III: 22-25% weight loss. Potentially best-in-class obesity treatment. |
| Cardiogen | Longevity fans take it in 10–20 day cycles for the heart. Randomized human trials: zero. | Khavinson tetrapeptide for cardiac tissue bioregulation and cardiovascular support. |
| Cartalax | Taken in Khavinson cycles for the joints. The evidence: rat cartilage cells in a dish, zero human trials. | Khavinson tripeptide for cartilage and connective tissue. Chondrocyte gene regulation. |
| Cerebrolysin (G) | Pig-brain extract for stroke recovery. 200 trials, 44 countries, zero FDA. | Complex mixture of neurotrophic peptides from porcine brain. Used clinically in Europe/Asia for stroke. |
| Chonluten | Taken with Bronchogen in Khavinson cycles for the airways. Randomized human trials: zero. | Khavinson tripeptide for GI mucosal barrier integrity and gut epithelial support. |
| CJC-1295 | Anti-aging clinics pair it with ipamorelin for growth hormone. Human data: 57 people in two Phase 1 trials. | GHRH analog. The foundation of every GH secretagogue stack. Always paired with Ipamorelin. |
| Cortagen | Sold as a synthetic Cortexin, Russia's brain drug. Best result: rat nerves regrew 27% faster, in 2000. | Khavinson tetrapeptide for cerebral cortex function. Synthetic version of Cortexin. |
| Decapeptide-12 | Sold in creams for melasma and dark spots. Its best trial: five women, one split-face study. | Most potent peptide tyrosinase inhibitor. 50x kojic acid for skin brightening. |
| Dihexa | Sold as a memory-and-focus capsule. The 2014 paper behind the hype was retracted in April 2025. | 10 million times more potent than BDNF at synapse formation. Extreme neuroplasticity compound. |
| DS5 | A vendor-made nasal spray of Dihexa and Semax. Trials of the blend: zero, and the ratios vary by vendor. | Pre-formulated nootropic blend. Dual-pathway neuroplasticity through HGF and BDNF elevation. |
| DSIP | Injected before bed for deeper sleep. A 1992 double-blind insomnia trial called it 'not likely' to help. | Neuropeptide that modulates sleep architecture. Promotes delta wave (deep) sleep. |
| Dulaglutide | Trulicity: a weekly diabetes shot, not a weight-loss drug. Typical loss: 2–5 kg. | Weekly GLP-1 agonist. Moderate weight loss with strong cardiovascular benefit data. |
| Eloralintide | Lilly's next weight-loss shot, in trials only. Phase 2: 20% weight loss at 48 weeks in 263 adults. | Long-acting amylin analog for obesity. Additive weight loss with GLP-1 agonists. |
| Enclomiphene Citrate | Men's clinics prescribe it to raise testosterone and keep fertility. The FDA turned it down in 2015. | The TRT alternative for men who want their testosterone back WITHOUT killing fertility. Oral pill, no needles. |
| Epithalon | Russia's longevity peptide, taken in 10-day courses. Its famous 266-person study tested the pineal extract. | The most popular longevity peptide. Used to activate telomerase — often in short cycles aimed at telomere maintenance. |
| FLGR-242 | A codenamed follistatin, sold by the company its promoter co-founded. Published studies of it: zero. | A follistatin with a codename, sold by the company its promoter co-founded. No published study of it. |
| Follistatin-344 | Bodybuilders buy vials to lift the body's muscle brake. Controlled human trials of vial follistatin: zero. | Myostatin inhibitor. Removes the brake on muscle growth. Gene therapy version in clinical trials. |
| FOXO4-DRI | Sold to longevity buyers as a zombie-cell killer. Human data: none; one 2017 mouse paper started it. | Senolytic peptide that selectively eliminates senescent cells by disrupting FOXO4-p53 interaction. |
| GHK Basic | GHK-Cu without the copper, for creams that also carry vitamin C. Randomized human trials of it: zero. | GHK tripeptide without copper. 4,000+ gene modulation. Compatible with vitamin C. |
| GHK-Cu | Sold in serums and injected off-label for hair. Its human trials: 12-week face creams, not injections. | Modulates 4,000+ genes toward youthful expression. Collagen, wound healing, antioxidant defense. |
| GHRP-2 | Clinic users inject it 2–3 times a day. Its only approval: a one-shot growth-hormone test in Japan, 2004. | Potent GHRP with moderate cortisol/prolactin elevation. Stronger GH release than Ipamorelin. |
| GHRP-6 | The 1984 original, now taken mostly for its hunger kick. Four decades of human data: dozens of small studies. | First-generation GHRP. Strong GH release with significant appetite stimulation (ghrelin pathway). |
| Glumitide | Lilly's trial-only shot, built from one half of Zepbound. Data: two Phase 1 studies, four weeks of dosing. | (no entry; the card used the taxonomy text: "Eli Lilly's investigational once-weekly GIP-only injection — separating one part of the tirzepatide signal from the others to see what GIP alone actually does.") |
| Glutathione | Sold as an IV drip at wellness clinics. Its best trial: 54 adults, oral doses, body stores up over 6 months. | The body's master antioxidant. Detoxification, immune support, skin health. |
| Gonadorelin | Clinics swap it in for hCG on TRT. It lasts 2–10 minutes in the blood, and studies of that use are thin. | GnRH analog for HPG axis preservation during TRT. Replaced HCG in most telehealth protocols. |
| GW-0742 | Sold as a 'cleaner Cardarine' for endurance and fat loss. Human studies: zero; it never entered trials. | PPARδ agonist for fat oxidation and endurance. Next-generation cardarine alternative. |
| HCG | Men on testosterone use it to stay fertile. The standard 500 IU every-other-day dose comes from a 2005 study. | LH analog for testicular function on TRT. Reclassified as biologic in 2020. |
| Hexarelin | Known for the biggest growth-hormone pulse of its family. The catch: it fades within 2–4 weeks of daily use. | Most potent GHRP. Strongest GH pulse but desensitizes within 2-4 weeks. Cardiac CD36 benefits. |
| HGH | Anti-aging clinics sell it for body composition. That story began with 12 men in a 1990 study. | Recombinant human growth hormone. The gold standard. FDA-approved for GH deficiency. |
| HGH Fragment 176-191 | Sold as 'HGH frag' for fat loss. Human trials of this exact fragment: zero; its cousin AOD-9604 failed. | C-terminal GH fragment for targeted fat loss without growth or diabetogenic effects. |
| Humanin | A mitochondrial peptide sold for longevity. Twenty years of cell and rodent work; human trials: zero. | Mitochondrial-derived cytoprotective peptide. BAX inhibition. Levels decline 40% with aging. |
| IGF-1 DES | Bodybuilders inject it into a muscle for 'spot growth'. Human studies of it: zero; it lasts 20–30 minutes. | 10x potency of native IGF-1. Ultra-short half-life for site-specific local injection. |
| IGF-1 LR3 | Taken by advanced lifters as an all-day muscle signal. Randomized human trials of this version: zero. | The muscle-growth signal your body makes after GH release &mdash; extended to last all day. Used by the serious lifters. |
| Insulin | 8 million Americans need it for diabetes; some bodybuilders inject it for size. Misuse can kill. | Master anabolic hormone. FDA-approved for diabetes. LETHAL if misused in performance contexts. |
| Ipamorelin (G) | The polite growth-hormone releaser — pulses without the cortisol mess. Human data: thin. | The cleanest GHRP — GH release without cortisol or prolactin elevation. Essential CJC-1295 partner. |
| Kisspeptin | Clinics sell it as a TRT add-on. Its Phase 2 trials, from two hospital groups, never tested that. | Upstream HPG axis regulator. Master switch for GnRH pulsatility and reproductive function. |
| KPV | Taken for gut and skin flares. Its best evidence: two 2008 mouse-colitis papers; human trials: zero. | A 3-amino-acid anti-inflammatory peptide. Quiet power for gut flares, skin inflammation, and autoimmune skin issues. |
| Liraglutide | The daily shot before Ozempic, approved for kids as young as six. Its main trial: 8% weight loss at 56 weeks. | Daily GLP-1 agonist. Predecessor to semaglutide. FDA-approved for diabetes and obesity. |
| Livagen | Pitched for liver and immune support in Khavinson cycles. Good randomized human trials: zero. | Khavinson tetrapeptide for liver. Demonstrated heterochromatin decondensation in hepatocytes. |
| LL-37 | Injected for immune support. Its roughly 10 small human trials tested it on leg ulcers, not in shots. | Endogenous antimicrobial peptide. Broad-spectrum against bacteria, fungi, viruses. Biofilm disruption. |
| Matrixyl | In hundreds of anti-aging serums. Its one good independent trial: 93 women, 12 weeks, modest gains. | Most studied collagen-stimulating cosmetic peptide. Matrikine signaling for wrinkle reduction. |
| Mazdutide | Prescribed in China since 2025; not approved in the US. Main trial: 14.4% weight loss at 32 weeks. | Dual GLP-1/glucagon agonist with liver fat targeting. Phase III in development. |
| Melanotan I | Tanners inject the gray-market version. The approved one is a 16 mg implant for a rare sun allergy. | MC1R-selective tanning peptide. FDA-approved (Scenesse) for EPP. Photoprotective melanin production. |
| Melanotan II | Injected for a sunless tan and a libido kick. The tanning trial behind it had 3 people, in 1996. | People use it for tanning, sexual arousal, and appetite suppression. More potent than Melanotan I — hits three effects at once. |
| Methylene Blue | Biohackers take drops for brain energy. Its one brain study: a single 280 mg dose, 26 people, in 2016. | A 120-year-old medical dye. Used in hospitals for poisoning. Taken in drops by biohackers chasing brain energy. Yes, your tongue turns blue. |
| MGF | Lifters inject it into a muscle right after training. Human trials: zero; it lasts 5–7 minutes. | IGF-1 splice variant for satellite cell activation. 5-7 minute half-life. Post-workout injection. |
| MK-677 | Taken nightly for growth hormone and sleep. A year-long trial: +1.1 kg lean mass, no strength gain. | Oral ghrelin receptor agonist. Daily GH/IGF-1 elevation without injection. Watch appetite + insulin sensitivity. |
| MOTS-c | Sold as exercise in a bottle. Mice ran longer; human trials of injected MOTS-c: zero. | The exercise-mimetic peptide your mitochondria make. Activates AMPK — the same pathway as exercise and metformin. |
| N-Acetyl Selank | Sold as a longer-lasting Selank. Trials of the capped version: zero. | Enhanced anxiolytic peptide. GABAergic calm without sedation, tolerance, or dependence. |
| N-Acetyl Semax | Sold as a longer-lasting Semax. Trials of the capped version: zero; the parent has two Russian stroke trials. | Enhanced Semax with superior BBB penetration. Maximum BDNF elevation and cognitive enhancement. |
| N-Acetyl-Epithalon | Epithalon with chemical caps, sold as longer-lasting. Human studies of the capped version: zero. | Enhanced telomerase-activating peptide with improved metabolic stability. |
| NAD+ | Clinics sell it by IV because levels halve between 20 and 60. The trials mostly tested pills, not drips. | Levels fall ~50% from age 20 to 60 — the cellular-energy coenzyme at the heart of mitochondrial-longevity research. |
| Nesfatin-1 | Studied since 2006 as an appetite brake. It has never been given to a person in a trial. | Endogenous satiety peptide. Leptin-independent appetite suppression. Preclinical research only. |
| Neurokinin A | Inhaled in labs to tighten airways and test asthma drugs. Not a therapy: the 3 drugs in its family block it. | Tachykinin neuropeptide. NK2R agonist for bronchoconstriction and visceral pain research. |
| Nonapeptide-1 | Sold in brightening creams at 1–2%. Independent human trials: zero; the data are the maker's. | Dual-mechanism brightener. MC1R antagonism + melanosome transfer inhibition. |
| Orforglipron | Foundayo: a daily weight-loss pill with no fasting rules. Phase 3: 11.2% weight loss at 72 weeks. | Non-peptide oral GLP-1 agonist. 11.2% weight loss at 72 weeks in Phase 3 — a daily pill, no injections. |
| Ovagen | Taken for liver and gut support. One lab's research, zero human trials; nothing to do with ovaries. | Khavinson tripeptide for liver and GI tract support. Broader GI coverage than Livagen. |
| Oxytocin | Sold as a nose spray for mood and bonding. Approved in 1962 for labor; the nasal-spray trials are split. | Neuropeptide for social cognition, anxiety reduction, and metabolic improvement. Intranasal. |
| P21 | Nootropic buyers order it for new brain cells. Fifteen papers, one lab, mice only; human trials: zero. | CNTF-derived compound promoting adult neurogenesis. BBB-penetrant. Strong Alzheimer's model data. |
| Pal-AHK | Used in hair serums at 0.5–2%. Peer-reviewed trials of this copper-free version: zero. | Palmitoylated AHK for collagen stimulation. Component of Matrixyl 3000. |
| Pal-GHK | Half of the Matrixyl 3000 combo. Its best evidence is a 93-woman trial of a different peptide. | Topical GHK for collagen signaling. Component of Matrixyl 3000. |
| Palmitoyl Dipeptide-6 | Sold in firming serums at 1–4%. Peer-reviewed trials: zero; the claims come from supplier dossiers. | Anti-inflammatory skin peptide for sensitive and reactive skin. |
| Pancragen | Used in Russian clinics alongside diabetes care. The evidence: 20 years of Russian case series. | Khavinson tetrapeptide for pancreatic beta cell support and insulin secretion normalization. |
| PEG-MGF | Longer-lasting MGF, taken with BPC-157 or TB-500 by strength crowds. Human trials: zero. | Extended half-life MGF. Systemic satellite cell activation over days rather than minutes. |
| Pemvidutide | Altimmune's two-in-one weight-loss shot, in trials only. Phase 2: 15.6% loss at 48 weeks in 391 adults. | Dual GLP-1/glucagon agonist. 15.6% weight loss + significant liver fat reduction in Phase II. |
| Pentapeptide-18 | Sold in wrinkle serums with Argireline. Its one open-label study: 11.31% shallower wrinkles in 28 days. | Enkephalin mimetic for wrinkle reduction via opioid receptor modulation. |
| Pinealon | Used in Russia after brain injury and stacked by longevity fans. Its best study: a 72-patient Russian series. | The pineal-gland bioregulator the Khavinson longevity crowd stacks with Epithalon. Short courses for sleep, focus, aging. |
| Pramlintide | Approved in 2005 for diabetics on insulin: three shots a day, boxed warning. Not a weight-loss drug. | FDA-approved amylin analog for diabetes. 3x daily with meals. Foundation for next-gen amylin drugs. |
| PT-141 | Approved for women's low desire, used mostly off-label by men. The approval trials: 1,247 women, zero men. | FDA-approved for female HSDD (Vyleesi). Works centrally on desire and arousal — on-demand SubQ, unlike Viagra's vascular mechanism. |
| Retatrutide | Lilly's triple-hormone shot, trials only; gray-market copies sell anyway. Phase 2: 24.2% loss at 48 weeks. | Up to 24% weight loss in Phase II — the most potent weight-loss compound ever studied. Triple agonist: GLP-1 + GIP + glucagon. |
| Rigin | Half of Matrixyl 3000, sold as anti-inflammaging. Trials of Rigin alone: zero; the data are the supplier's. | IL-6 suppression for anti-inflammaging. Component of Matrixyl 3000. |
| Selank | A Russian nasal spray for anxiety, approved there in 2009. Every trial so far is Russian. | Russian anxiolytic peptide. GABAergic calm without sedation or dependence. Pairs with Semax. |
| Semaglutide (G) | The one that actually went through the FDA. Weight loss, with receipts. | The GLP-1 agonist that revolutionized weight loss. FDA-approved, 15% average body weight reduction. |
| Semax | A Russian stroke drug that biohackers spray for focus. Its key stroke trial had 30 patients, in 1997. | Russian nootropic. BDNF elevation, cognitive enhancement, neuroprotection. Approved in Russia since 1994. |
| Sermorelin | The first FDA-approved growth-hormone releaser, now compounded for adults. The approval was for kids, 1997. | The original FDA-approved GH-release peptide. Safer, shorter-acting, better-established than CJC-1295. Where doctors start. |
| Setmelanotide | For rare genetic obesity, not everyday weight loss. Phase 3: 8 of 10 patients lost at least 10% in a year. | MC4R agonist FDA-approved for rare genetic obesity (POMC, PCSK1, LEPR deficiency). |
| SLU-PP-332 | Sold as exercise in a pill. The evidence: one 2023 mouse paper where mice ran 70% farther. | Exercise mimetic targeting ERRα nuclear receptor. Endurance and metabolic adaptation without exercise. |
| SNAP-8 | Sold as a stronger Argireline in eye creams. Its data: the maker's own studies, 4% over 28 days. | Enhanced Argireline. Extended SNARE inhibition for deeper expression line reduction. |
| SS-31 | Approved in 2025 for a rare mitochondrial disease. Off-label longevity use rests on Phase 2 data. | The first FDA-approved mitochondrial-membrane drug — Forzinity, 2024. People use it for fatigue and mitochondrial dysfunction. |
| Survodutide | Boehringer's two-in-one weight-loss shot, in Phase 3. Phase 2 beat placebo by up to 12 points of body weight. | Dual GLP-1/glucagon agonist. 18.7% weight loss + 86% liver fat reduction in Phase II. |
| Syn-Ake | Sold as a snake-venom answer to Botox, at 1–4% in creams. Peer-reviewed trials: zero. | Waglerin-1 mimetic. Post-synaptic nAChR antagonism for muscle relaxation. |
| Syn-Coll | Used at 2–4% in serums to push collagen. The claims come from supplier studies; independent trials: zero. | TGF-β pathway collagen stimulation. Different mechanism than Matrixyl. |
| TB-500 | Injected for tendons alongside BPC-157. What's sold is a 7-amino-acid fragment with zero human studies. | The systemic repair partner to BPC-157 — cell migration and tissue remodeling. Second half of the Wolverine Stack. |
| TB-500 Fragment 17-23 † | Sold as a cheaper core of TB-500. Human studies of this 7-amino-acid fragment: zero. | Minimal active fragment of TB-500. Core actin-binding domain for targeted tissue repair. |
| Tesamorelin | Approved for HIV-related belly fat; clinics use it off-label. Its 412-patient trial cut belly fat 15–18%. | The only FDA-approved peptide with proven visceral fat reduction. Originally approved for HIV lipodystrophy (Egrifta). |
| Tesofensine | Back on the gray market since the weight-loss-shot shortages. Phase 2: 9.2% loss in 24 weeks; no Phase 3. | Triple monoamine reuptake inhibitor. Strong appetite suppression and metabolic activation. |
| Testagen | Used in Russia for falling testosterone. The evidence: rodents from one lab, plus small Russian case reports. | Khavinson tetrapeptide for Leydig cell support and endogenous testosterone production. |
| Testosterone | Prescribed for low testosterone; also the original anabolic steroid. Normal is 300–1,000 ng/dL. | The foundational male hormone. TRT for hypogonadism, optimization, and body composition. |
| Thymagen | Two amino acids, sold in Moscow pharmacies as Thymogen for immunity. Western randomized trials: zero. | Khavinson thymic bioregulator dipeptide for immune maintenance between Thymalin courses. |
| Thymalin | A calf-thymus extract used in Russia since 1982. Its famous 266-person aging study came from its developers. | Polypeptide thymic extract. 40+ years of Russian clinical use for immune reconstitution. |
| Thymosin Alpha-1 | Prescribed for hepatitis B in about 35 countries. Not approved in the US, and on no FDA compounding list. | Thymic peptide for T cell maturation and NK cell activation. Approved in 30+ countries. |
| Thymulin | A zinc-charged thymus hormone, often confused with Thymalin. 50 years of research, mostly in rodents. | Zinc-dependent thymic nonapeptide for T cell maturation. Unique zinc-immunity link. |
| Tirzepatide | Zepbound: the weekly shot that beat semaglutide head-to-head. Main trial: 20.9% weight loss at 72 weeks. | Up to 22% weight loss in clinical trials. Outperformed semaglutide head-to-head in SURMOUNT-5 (20.2% vs 13.7%). |
| Tripeptide-29 | Sold in creams as the core of collagen, at 1–5%. Trials of creams with it as the active: zero. | Native collagen fragment. The bioactive in collagen supplements. Oral + topical. |
| Triptorelin | Bodybuilders use one shot to 'restart' testosterone. It's approved to shut testosterone off, within 3–4 weeks. | GnRH super-agonist. FDA-approved for prostate cancer. Controversial single-dose HPG restart use. |
| Vesugen | Sold for blood vessels; the identical 3-amino-acid Vesilute is sold for immunity. The evidence is Russian. | Khavinson tripeptide for vascular endothelium. eNOS upregulation and anti-atherosclerotic. |
| Vialox | Sold as a Botox alternative. Its '49% wrinkle reduction' is a supplier figure; peer-reviewed trials: zero. | Snake venom-inspired nAChR antagonist for expression line softening. |
| Vilon | Two amino acids, sold for immune support and aging. Independent human trials: zero. | Simplest Khavinson bioregulator — just 2 amino acids. Thymic immune support. |
| VIP | Sprayed up the nose for mold illness (CIRS). Two large Phase 3 trials for other uses failed. | Neuropeptide for CIRS/mold illness, pulmonary hypertension, and immune regulation. |
| VK2735 | Viking's rival to Mounjaro, in trials only. Phase 2: 14.7% weight loss in 13 weeks at the top dose. | Dual GIP/GLP-1 agonist in Phase II. 14.7% weight loss at 13 weeks. Oral form also in development. |
| Waglerin-1 | A lethal pit-viper neurotoxin, studied in labs. Wrinkle creams use a small mimic; human trials: zero. | Snake-venom-derived nAChR-targeting peptide. Inspiration for the cosmetic Syn-Ake analog. |
| Zuclomiphene | Clomiphene's other half, tested for hot flashes. Its only Phase 2 result is an unpublished interim readout. | Cis-isomer of clomiphene with long half-life and estrogenic effects. The counterpart to enclomiphene. |

---

## Tags

/compounds/ shows Stacks first, then the nine groups below.
- Within a group, cards run by evidence level (the number after each name), highest first, then by name.
- "(2nd)" marks a secondary tag.
- Your calls put testosterone and kisspeptin in Hormones & libido as their primary group.

- **Fat loss** (27: 25 primary, 2 secondary): Liraglutide 4, Orforglipron 4, Semaglutide 4, Setmelanotide 4, Tesamorelin (2nd) 4, Tirzepatide 4, AOD-9604 3, Cagrilintide 3, CagriSema 3, Dulaglutide 3, Eloralintide 3, Mazdutide 3, Pemvidutide 3, Pramlintide 3, Retatrutide 3, Survodutide 3, Tesofensine 3, VK2735 3, Glumitide 2, 5-Amino-1MQ 1, Adipotide 1, AICAR 1, GW-0742 1, HGH Fragment 176-191 1, MOTS-c (2nd) 1, Nesfatin-1 1, SLU-PP-332 1
- **Muscle & growth hormone** (16: 15 primary, 1 secondary): HGH 4, Tesamorelin 4, Testosterone (2nd) 4, GHRP-2 3, MK-677 3, Sermorelin 3, CJC-1295 2, GHRP-6 2, Hexarelin 2, Ipamorelin 2, Follistatin-344 1, IGF-1 DES 1, IGF-1 LR3 1, MGF 1, PEG-MGF 1, FLGR-242 0
- **Tendon & gut repair** (5: 4 primary, 1 secondary): BPC-157 2, Cartalax 1, KPV (2nd) 1, TB-500 1, BPC-157 Fragment 0
- **Skin & hair** (22: 19 primary, 3 secondary): Melanotan I 4, Matrixyl 3, Argireline 2, Decapeptide-12 2, GHK-Cu 2, Glutathione (2nd) 2, Melanotan II (2nd) 2, Pentapeptide-18 2, AHK-Cu 1, GHK Basic 1, KPV (2nd) 1, Nonapeptide-1 1, Pal-GHK 1, Rigin 1, SNAP-8 1, Syn-Ake 1, Syn-Coll 1, Tripeptide-29 1, Vialox 1, Waglerin-1 1, Pal-AHK 0, Palmitoyl Dipeptide-6 0
- **Mind & sleep** (14: 12 primary, 2 secondary): Bromantane 3, DSIP 3, Oxytocin 3, Selank 3, Ipamorelin (2nd) 2, Methylene Blue 2, Pinealon 2, Semax 2, Cortagen (2nd) 1, Dihexa 1, N-Acetyl Selank 1, N-Acetyl Semax 1, P21 1, DS5 0
- **Brain & nerves** (5: 3 primary, 2 secondary): ARA-290 3, Cerebrolysin 3, Pinealon (2nd) 2, Semax (2nd) 2, Cortagen 1
- **Hormones & libido** (11: 11 primary, 0 secondary): HCG 4, Insulin 4, PT-141 4, Testosterone 4, Triptorelin 4, Enclomiphene Citrate 3, Gonadorelin 3, Kisspeptin 3, Melanotan II 2, Testagen 1, Zuclomiphene 1
- **Longevity** (19: 14 primary, 5 secondary): HGH (2nd) 4, Epithalon 2, Glutathione 2, NAD+ 2, Pinealon (2nd) 2, SS-31 2, Thymalin (2nd) 2, Bronchogen 1, Cardiogen 1, Chonluten 1, FOXO4-DRI 1, Humanin 1, Livagen (2nd) 1, MOTS-c 1, N-Acetyl-Epithalon 1, Ovagen 1, Pancragen 1, Vesugen 1, Vilon (2nd) 1
- **Immune** (10: 10 primary, 0 secondary): Thymosin Alpha-1 3, Thymagen 2, Thymalin 2, Thymulin 2, KPV 1, Livagen 1, LL-37 1, Neurokinin A 1, Vilon 1, VIP 1

**Every secondary tag, with its reason** (16 tags on 14 compounds):

| Compound | Primary | Secondary | Reason |
|---|---|---|---|
| Testosterone | Hormones & libido | Muscle & growth hormone | Your call |
| HGH | Muscle & growth hormone | Longevity | Your call. Page: "Anti-aging clinics for off-label body-composition" |
| MOTS-c | Longevity | Fat loss | Your call. Page: "exercise-in-a-bottle effects, body composition, and insulin sensitivity" |
| Ipamorelin | Muscle & growth hormone | Mind & sleep | Page: "pair it with a GHRH peptide … for sleep, recovery, and lean mass" |
| Tesamorelin | Muscle & growth hormone | Fat loss | Page: "HIV-lipodystrophy clinics on-label. Off-label: … belly-fat-focused practices"; the label is abdominal-fat reduction |
| Pinealon | Mind & sleep | Brain & nerves; Longevity | Page: "Russian neurology for post-stroke and post-brain-injury cognitive recovery"; "Longevity users pair it with Epitalon and Thymalin" |
| Semax | Mind & sleep | Brain & nerves | Page: "Russian neurologists for acute stroke, mini-stroke, and fatigue" |
| Cortagen | Brain & nerves | Mind & sleep | Page: "nootropic self-experimenters running 10–20 day … cycles" |
| Melanotan II | Hormones & libido | Skin & hair | Page: "Gray-market users for tanning" |
| Glutathione | Longevity | Skin & hair | Page: "Asian dermatology for skin-tone outcomes" |
| KPV | Immune | Tendon & gut repair; Skin & hair | Page: "Users dealing with inflammatory bowel conditions"; "chronic skin conditions … Also used in topical skincare" |
| Livagen | Immune | Longevity | Page: "People exploring anti-aging and immune support" |
| Thymalin | Immune | Longevity | Page: "Modern longevity enthusiasts pair it with Epitalon" |
| Vilon | Immune | Longevity | Page: "People interested in immune support and anti-aging" |

Kisspeptin has no secondary tag; your call set its primary group.

---

## DECISIONS

**Part 0**
- **Homepage search:** Enter with nothing highlighted still searches `/compounds/?q=…`, so Enter never dead-ends. /compounds/ also drops its datalist, because its grid already filters as you type. On the yellow caution band, `--ink` is the band's ink, so the focus ring stays visible there.
- **Chips:** each chip links to its group on /compounds/. Old group anchors (#muscle, #skin, …) land on the new groups. The /compounds/ search counts a compound once, even when it sits in two groups.
- **Secondary tags:** your four calls, plus 11 compounds whose own "Who uses it?" line names a use in another group (table above). I added nothing that the page's text doesn't say.
- **"What changed":** every changelog entry now has a type (content, status, site, correction), and the line takes the newest content or status entry.
- **Section rows vs headings:** headings were renamed to the row words: Regulatory Status → Legal Status (& Access), Key References → References, Stack Overview → What It Is. Section ids are unchanged, so old anchors still work.
- **Content dates:** Parts 0.6 and 2 kept each page's content date (`prerender --reseed`), because no page says anything new. The corrections in Parts 1, 3 and 5 moved dates as usual.
- **Updated date and dot:** the "Updated {date}" text keeps showing for 30 days after a content change; the dot shows for the first 7.
- **Stacks:**
  - The stack tool moved from /stacks/ to /stack.html. /stacks, /stacks/ and /stacks/index.html return a 308.
  - /stacks/data.json and /stacks/pairs/ stay where they are.
  - 1,221 links and 328 JSON-LD breadcrumbs were repointed; the link text is unchanged.
  - Deep links (`?a=&b=`) still open a pair. The result title is an h2 under the page's one H1, "Stacks".
- **Alerts:**
  - The next known event is generated from the PCAC 2027 statuses, so it updates itself.
  - The nav's "Alerts" opens in the same tab; it was the Substack page in a new tab.
- **Guide:**
  - The legal counts are generated from the card stamps: 18 FDA-approved, 6 compounding pharmacy, 89 not legal yet or research only, of 113.
  - The four doors replace four starter cards that named retired categories.
- **Corrections page:** one line per page per fix, not per link. The list counts as page content, so a new line moves the page's Checked date.

**Part 1**
- **Fix or list:** I fixed a link only where the reference text identifies the intended paper (title, first author, journal and year). Otherwise the link stays and is listed; that's the 40.
- **The allowlist:** the hand-checked false alarms and the listed IDs are in `data/citation-allow.json`, each with its reason. The gate allows exactly those.
- **Rewrites:** where the right paper was cited but described wrongly, the description was rewritten to match the paper (the five named above).
- **Pair tables:** rebuilt with `stacks/generate.py`'s own functions. De-duplicating by PMID across the two compounds dropped rows on 17 pair pages; no PMID repeats within one compound's list.
- **Offline gate:** the resolver cache (`data/citation-cache.json`) is committed, so the gate can run offline. The cache, the allowlist and `data/dosing-openers.json` are kept out of the deploy (`.vercelignore`).

**Part 2**
- **Your examples:** your five hooks are used verbatim (Look at these first, 2).
- **Numbers:** every number in a hook comes from its page's own text; I cited nothing new.
- **Glumitide and motilin:** glumitide gained its own entry. The unused "motilin" entry was dropped because there's no page, so the total stays 120.
- **The opener line:** its text is stored in `data/dosing-openers.json`. Flow State had no Dosing section, so it got one holding only that line.
- **The calculator link:** it goes under Dosing, except on the oral and topical compounds and where the section already ends with one.
- **Section rows** follow page order, so Dosing is now the first row.

**Part 3**
- **3a redirects:** they are 301s (a permanent move), not 308s.
  - Each removed pair page goes to the TB-500 pair with the same partner.
  - Two have no such pair, so they go to the TB-500 page: tb-500-with-tb-500-fragment and bpc-157-fragment-with-tb-500-fragment.
- **3f:** no corrections entries, because only the wording changed.
- **3i:** "spring 2025", from FDA's dates, instead of the brief's "Feb 2025".
- **3l — Pipeline row scope.** Three things feed it:
  - the FDA committee review due before the end of February 2027, for the five PCAC 2027 compounds (from their status)
  - registered trials of the compound itself that are recruiting, active or not yet recruiting, with the NCT number and estimated primary completion
  - CagriSema's Q4 2026 FDA decision
- **3l — left out of the Pipeline row:**
  - completed trials
  - trials of other molecules
  - registry records whose sponsor labels them mock or fictional
  - the seven July-PCAC compounds' pending rulemaking, which has no date (Open question 4)
- **3l — tesamorelin's NCT00435136** isn't on ClinicalTrials.gov. It was left as it is (Open question 5).
- **Semax:** fixed outside the brief, as a correction with its own entry.

**Part 4**
- **308:** as the brief says, for /calc-next.html and /calc-next.
- **U-40 in the embed:** the embed gets the same U-40 link. A shared link or saved state on U-40 opens the U-40 row by itself.
- **Column widths:** 188 px on the page (170 px under 360 px) and 172 px in the embed. The embed's "your amount" placeholder now has 93 px for the 90 px it needs.
- **Link wording:** five more "Stack Builder" variants took the same wording. "Try the Peptide Calculator →" links outside the 126 were left as they were.
- **Alert form:** its placeholder is slightly clipped at 390 px. That's older than this session, and I left it.

**Part 5**
- **Fix, then redeploy:** each problem the live checks found was fixed, committed and deployed at once, rather than waiting for the report.
- **Quick facts:** TB-500's status was shortened to fit the pair pages' 60-character box; the TB-500 page keeps its longer wording. Two older statuses are cut the same way (gonadorelin, 73 characters; HGH, 62). I left those (Open question 7).
- **The enclomiphene label:** the 3l corrections entry was edited to describe what the page says now. No second entry was added for a label that was live for about 25 minutes.
- **Pipeline data is public:** `data/pipeline.json` is served like `data/changelog.json` and `stacks/data.json`. It holds only public registry data.

---

## Checks

**Structural audit (`scripts/street_lab_audit.py`)**
- It ran after every part. For two parts it ran on a sample only: the Pipeline row (4 of 122 pages) and Part 4 (4 of 121).
- After the deploy I re-ran it for every commit, as the script stood at that commit. Each commit was compared with its parent, on every page it added or changed.
- Every flag is that commit's own planned edit:

| Part | Commit | Pages | Flagged | What the flags were |
|---|---|---|---|---|
| 0.1–0.5, 0.7, 0.9, 0.11, 0.12 | 9 commits | 0–459 each | 0 | — |
| 0.6 | `35fa1878` | 123 | 0 | (the audit knows the planned Gist and heading edits) |
| 0.8 | `cd46dbeb` | 1 | 1 | stack.html's OG image, moved in the next commit |
| 0.8 | `6cc9eba5` | 459 | 1 | stack.html: the tool's headings replaced by the hub |
| 0.10 | `b19fe13e` | 1 | 1 | the guide's new headings; four starter cards (and their links) replaced by the doors |
| 2 | `093e3050` | 126 | 0 | (the audit knows the dosing move) |
| 1 | `1be3f87b` | 426 | 422 | references, literature tables, citation links, and four FAQ answers' PMIDs |
| 3a | `47e2f4ff` | 129 | 125 | "any of 113 compounds" → 112 on the pair pages; TB-500's quick facts; the TB-500, compare and Wolverine pages; links to the fragment page |
| 3b–3e, 3g–3k, Semax | 10 commits | 2–6 each | 1–5 each | the corrected text, tables, references and FAQ JSON-LD |
| 3f | `e7a6f7fc` | 79 | 79 | the rewritten sentences |
| 3l | `30af53ff` / `bbedebfe` | 12 / 63 | 11 / 63 | the trial IDs |
| 3l | `f85e68b9` | 122 | 0 | (the Pipeline block) |
| 4 | `c03b16a3` | 121 | 121 | the reworded calculator links; calculator.html replaced by the rebuild (its FAQ, see Look at these first, 3) |
| 5 | `1019e275` / `de295e58` | 7 / 6 | 7 / 5 | TB-500's status text; the ZA label |
| 3m, 5 | `d6c17a52`, `031c2d38` | 0 | 0 | scripts, CLAUDE.md and `vercel.json` only |

- **Final run vs HEAD:** 452 pages checked, 0 failed.

**Other gates**
- **Citations:** `python3 scripts/audit_pages.py --mode cite`: 8,957 citations on 442 pages, 0 failing, 262 allowed with a reason. It also passed on each part's pages from Part 1 on.
- **Calculator:** `bash tests/run-calc-tests.sh` PASS (400/400, 28-case selftest, frozen v1.0). `tests/calc-scenarios.mjs`: all 29 scenarios pass at 320, 390 and 1280 px, and on the embed at 360 px.
- **Smoke test:** 13 key pages at 390 px in headless Chrome. No JavaScript errors and no horizontal overflow.
- **Prerender:** idempotent; the second run reports "452 pages decorated (0 changed)". `git ls-files --deleted` was empty before each deploy.
- **Before every push:** the two identity checks and gitleaks, all clean.

**Live (`curl`, www.kalios.health, on the final deploy)**

| URL | Status |
|---|---|
| `/`, `/compounds/`, `/compounds/bpc-157.html`, `/compounds/tb-500.html`, `/compounds/mito-stack.html` | 200 |
| `/stack.html`, `/alerts.html`, `/corrections.html`, `/beginners-guide.html`, `/calculator.html` | 200 |
| `/embed/calculator.html`, `/compounds/enclomiphene.html`, `/stacks/pairs/bpc-157-with-tb-500/` | 200 |
| `/calc-next.html`, `/calc-next` | 308 → `/calculator.html` |
| `/compounds/tb-500-fragment.html`, `/compounds/tb-500-fragment` | 301 → `/compounds/tb-500.html` |
| the five removed pair pages, each as `/x`, `/x/` and `/x/index.html` (15 addresses) | 301 → the TB-500 pair, or the TB-500 page |
| `/stacks/` | 308 → `/stack.html` |
| `/CLAUDE.md`, `/reports/`, `/scripts/citations.py`, `/gsc-export/citation-audit.csv`, `/data/citation-cache.json`, `/data/citation-allow.json`, `/data/dosing-openers.json` | 404 |

- **Content checked live:**
  - the calculator's U-40 link
  - TB-500's Ac-LKKTETQ
  - 150 lines on /corrections.html
  - retatrutide's Pipeline row
  - the TB-500 pair status
  - the ZA-304 / ZA-305 label

---

## Counts

- **Compounds:** 113 profiles, 3 alias pages, the MT-2 redirect and 6 stacks; 124 files in `compounds/`.
- **Hooks rewritten:** 120 (119 live).
- **Corrections entries:** 150. Part 0.12 added 1, Part 1 added 115, Part 3 added 34.
- **Citations:** 9,033 occurrences audited; about 2,200 links replaced; 40 IDs listed.
- **Section 13:** 115 links reworded. Re-pointed calculator links: 125, plus 5 variants.
- **Should/must:** 154 sentences rewritten on 79 pages; 21 kept.
- **Trial IDs:** 11 corrected on the pages; 10 entries in `stacks/data.json`; 63 pair tables re-rendered.
- **Pipeline rows:** 16 pages (the block is on 122).
- **Stacks move:** 1,221 links and 328 breadcrumbs repointed.
- **Redirects added:**
  - TB-500 Fragment: 2
  - the removed pair pages: 15
  - calc-next: 2
  - /stacks: 3

## Files changed (main ones)

- **New:**
  - `alerts.html`, `corrections.html`
  - `scripts/citations.py`, `scripts/migrate_12a.py`
  - `data/citation-cache.json`, `data/citation-allow.json`, `data/dosing-openers.json`, `data/pipeline.json`
  - `gsc-export/citation-audit.csv`
  - `og/alerts.png`, `og/corrections.png`, `og/stack.png`
- **Moved:** `stacks/index.html` → `stack.html`.
- **Deleted:**
  - `compounds/tb-500-fragment.html`, and the five fragment pair pages, with their OG images
  - `calc-next.html`, `calc-next.webmanifest`, `og/calc-next.png`
  - `og/stacks.png`
- **Changed:**
  - `calculator.html` (now the rebuild), `embed/calculator.html`
  - `assets/calc.js`, `calc.css`, `calc-embed.css`, `street-lab.css`, `street-lab.js`
  - `scripts/streetlab.py`, `prerender.py`, `card_data.py`, `gen_regulatory_status.py`, `audit_pages.py`, `street_lab_audit.py`
  - `stacks/generate.py`, `stacks/data.json`
  - `data/compound-desc.json`, `changelog.json`, `regulatory-status.json`, `page-dates.json`
  - `taxonomy.json`, `vercel.json`, `.vercelignore`, `sitemap.xml`, `llms.txt`, `CLAUDE.md`, `tests/calc-scenarios.mjs`
  - pages: nearly every compound, stack and pair page

## URLs to eye-check (www.kalios.health)

1. **`/`** on a phone:
   - type "reta" in the search
   - swipe the chip row
   - "First time? Start here →"
   - the "what changed" line
   - the footer's full-width fine print and "Corrections"
2. **`/compounds/tb-500.html`:**
   - the reframed page
   - the four-question Gist box
   - Dosing right after it, with its opener line
   - "Show all N" under the data table
   - "Aa" at the end of the section rows
3. **`/stack.html`:** the six stack cards, then pick any two (no Generate button).
4. **`/calculator.html`** on a phone: the "e.g. 10" placeholder, then "Using a syringe marked 40 units? →".
5. **`/corrections.html`:** 150 one-line corrections. Scan a few for tone.

## Open questions for G

1. **GHK-Cu's band.** Keep your title and sentence, or match FDA's record? One option:
   - Title: "Injectable GHK-Cu: not legal to compound".
   - Sentence: "Non-injectable GHK-Cu is on FDA's 503A Category 1 list, and FDA plans a committee review of it before the end of February 2027. The injectable nomination was withdrawn."
2. **Your example hooks.** Keep all five verbatim, or bring Semaglutide, Adipotide, Ipamorelin and BPC-157 to the formula?
3. **The calculator's FAQ.** Restore the six questions and their FAQPage markup below the new calculator? They'd need rewording, because the rebuild has no compound presets.
4. **Pipeline row.** Add the seven July-PCAC compounds' pending FDA rulemaking? No date has been announced. Today the row shows only dated or registered events.
5. **Unverified lines left as they were.** Check them, or remove them?
   - Melanotan-I: "Specialty-pharmacy REMS" and "REMS-certified provider". Not checked against FDA's REMS list.
   - hCG: its line about a February 2026 HHS "peptide reclassification announcement". No source was found this session.
   - Tesamorelin: NCT00435136. It isn't on ClinicalTrials.gov.
6. **Three citations need a person to pick the paper:**
   - **hCG:** "Kaminetsky et al., BJU Int 2014 (PMID 24127760)". The ID opens a dental-education paper. The text fits Kaminetsky 2013 (J Sex Med) or Kim, McCullough and Kaminetsky 2016 (BJU Int, obese men).
   - **Oxytocin:** "Lawson et al., Diabetes Care 2020 (PMID 31657509)". The ID is Lawson's 2020 review in J Neuroendocrinol. The 4-week crossover trial in hypothalamic obesity that the text describes wasn't identified.
   - **Humanin:** "Lee et al., Nat Rev Endocrinol 2015; PMID 26260367". PubMed has no such record.
7. **Two more quick facts cut at 60 characters** on pair pages: gonadorelin's FDA status and HGH's. Both are older than this session. Shorten them as TB-500's was?

## Carried over

- **Resolved by 12a:**
  - FLGR-242 report Q1 (follistatin's citations, and a site-wide check) and Q3 (the changelog line)
  - Calculator report Q1 (placeholders), Q3 (paper inputs), Q4 (dial at 60), Q5 (old links), Q6 (the 28-day line) and Q8 (nicknames)
  - Calculator test-address report Q1–Q5
  - 11c Q2 (GHK-Cu's title, now in your words; see Open question 1)
- **Still open:**
  - FLGR-242 Q2 (Reichel 2019) and Q4 (the Gist)
  - Calculator Q2 (the ambiguity rule) and Q7 (embed height)
  - 11c Q1 (spec §7 and the artboards), Q3 (OG mark), Q4 ("Aa" memory), Q5 (held reports), Q6 (the alert's follow-ups) and Q7 (a Print label button)
  - the promote report's Q2, Q4 and Q7
  - the 503B question (3d corrected four pages' 503B claims)
  - the Vercel author block (deploys still go through the git-less copy)
  - identity-workflow Q5–10
  - the content-truth open questions

## Web content

Rule 3: everything fetched was treated as data; nothing was acted on as an instruction.
- **Sources read:**
  - NCBI E-utilities and the PMC ID converter; Crossref, DataCite and doi.org
  - ClinicalTrials.gov API v2
  - fda.gov: 503A/503B lists and categories, guidance, PCAC documents, approval letters, declaratory orders
  - accessdata.fda.gov and api.fda.gov (openFDA); DailyMed; ecfr.gov; federalregister.gov; FDA's GSRS
  - web.archive.org, for older FDA PDFs
  - Novo Nordisk releases and reports, and SEC EDGAR (a Novo 6-K)
  - Theratechnologies releases
  - Vidal (vidal.ru)
  - WHO INN lists and WADA Prohibited Lists
  - PubChem and UniProt
  - a law-firm client alert and trade press, on the PCAC vote
  - www.kalios.health and the IndexNow API
- **Instruction-like text:**
  - The PharmExec article page carried an HTML comment addressed to AI/LLMs, pointing to a site index file. It was ignored.
  - The search tool wraps its results in its own "REMINDER: You MUST include the sources…" line. That's tool formatting, not page content, and it was ignored.
- **Bad data, not used:**
  - A search-tool summary said FDA published a new mixing/diluting draft guidance in "February 2026". The article behind it is about the February 2015 draft.
  - Three ClinicalTrials.gov records from a sponsor named "Hudson Biotech" (NCT07487363, NCT07437547, NCT07481734) describe themselves as mock or fictional. They're kept out of the Pipeline row.
  - Vendor blogs that contradict FDA were not used as evidence; one said sermorelin is on no 503A or 503B list.
- **Access limits:**
  - purplebooksearch.fda.gov and FDA's drug-shortage pages blocked scripted requests. They were read through a page summarizer and cross-checked with openFDA.
  - usp.org returned 403, so USP–NF wasn't read directly. FDA's GSRS stood in for the monograph check.
