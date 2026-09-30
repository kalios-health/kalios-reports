# 12d — Truth, second layer + stamps

**Date:** 2026-09-30 · **Brief:** 12d (Parts 1–6) · **Branch:** master, five commits, pushed
**Deploy:** production, `dpl_AwoHyUmKMusjtWiEDEgiwszEWg1h` (Ready, aliased to kalios.health). IndexNow accepted 174 URLs.
**Live:** every URL in the brief answers 200; the stamps, stance lines and today's corrections are on the live pages.

---

## Look at these first

1. **COA Check still fails closed on production: the Turnstile secret is refused.** The diagnostics
   Part 5 added now say so out loud. One line from this deployment's log:

   ```
   coa: turnstile 400 ["invalid-input-secret"] hostname -
   ```

   That is Cloudflare refusing the **secret**, not a reader's token. Until it is fixed, every check on
   kalios.health answers 503 "unavailable". Nothing else on the site is affected. The fix is yours (the
   secret lives in your Vercel environment; I never read or set it):
   - In Cloudflare Turnstile, copy the **secret key** of the same widget whose **site key** is in
     `coa/index.html`. A site key from one widget and a secret from another always gives this error.
   - Put it in the Vercel Shared Environment Variable `TURNSTILE_SECRET_KEY` for **Production**.
   - Redeploy (an environment change only reaches new deployments), then POST any JPEG to
     `/api/coa` with header `x-turnstile-token: XXXX.DUMMY.TOKEN.XXXX`. **403 "challenge"** means the
     secret is right and the dummy token was refused. 503 means it is still wrong.

2. **128 sentences on the site said something their own citation does not say.** They are fixed and
   each is on /corrections.html. The pattern worth knowing: most were not invented numbers but
   *drifted* ones — a species swapped (gerbils written as rats, sheep as rats, an analogue's data as
   the compound's), a design upgraded (a retrospective review called prospective, a 37-woman
   open-label study called an 8-week n=79 trial, "Phase 3" on papers that give no phase), a figure
   rounded the wrong way or attached to the wrong arm. Twelve pages carried four or more.

3. **19 data-table values were wrong**, including ten molecular weights (MOTS-c was out by 294 Da) and
   two WADA rows that said a compound is not named on the Prohibited List when the 2026 List names it
   (kisspeptin, tesofensine). Fixed, with corrections.

4. **Two figures the site quotes have no source I could find.** They are now marked on the pages
   rather than removed, and they are the two things on this list I would like your call on:
   - humanin: "HNG is ~1000-fold more potent than native humanin" appears four times. Neither cited
     Hashimoto 2001 paper's abstract carries it and both are closed access.
   - bromantane: "~11.5 hours" half-life. Nothing on the page or in PubMed reports it. The row now
     says so.

---

## What shipped

| Part | What | Commit |
|---|---|---|
| 1 | Three stamps with stance lines, everywhere | `01e482e2` |
| 2 | Every cited sentence checked against its paper | `b5376b40` |
| 3 | Every data-table value checked | `5eb469b5` |
| 4 | Remainders (the 8 PMIDs, SS-31, pinealon, Tesa/Ipa, thymosin alpha-1, …) | `5f2303fb` |
| 5 | COA Check diagnostics | `239e2758` |
| 6 | Gates, deploy, IndexNow, this report | — |

Parts 1, 4 and 5 were built and pushed in the first half of the session; Parts 2, 3 and 6 in the
second. Parts 1, 4 and 5 are described in their commit messages and are unchanged since.

---

## Part 1 — the stamp mapping (all 137 pages)

Three legal stamps, from the regulatory vocabulary already in the data:

| Stamp | Pages | What it maps from |
|---|---|---|
| **GRAY MARKET** (red) | 111 | RESEARCH ONLY, PCAC RECOMMENDED, PCAC 2027 — 100 compounds, 4 alias pages, all 7 stacks |
| **APPROVED DRUG** (green) | 20 | FDA APPROVED |
| **COMPOUNDING PHARMACY** (green) | 6 | COMPOUNDED RX |

Under each stamp, one mono stance line. Your words wherever they were true; where a page or FDA's own
lists contradicted them, the compound keeps your first clause and the rest is its own, with the reason
recorded per compound in `gsc-export/card-data.csv`:

| Stance line | Pages |
|---|---|
| No FDA ruling — not on any list | 86 |
| Prescribed by a doctor, made by a manufacturer. | 20 |
| FDA panel said yes on July 23–24, 2026 — not legal yet | 6 |
| On FDA's 503A list — a licensed pharmacy can compound it for you. | 4 |
| FDA said no — on the do-not-compound list | 4 |
| No FDA ruling — on FDA's 503A Category 3 list, nominated without enough data | 3 |
| No FDA ruling — on FDA's 503B Category 3 list, nominated without enough data | 2 |
| The active ingredient of a discontinued FDA-approved drug — a licensed pharmacy can compound it for you. | 2 |
| a stack naming its most restrictive member (Ipamorelin ×2, BPC-157 and TB-500, MOTS-c, Selank) | 5 |
| GHK-Cu's own two lines (the injectable) | 3 |
| FDA said no — its advisory panel voted against it on July 24, 2026 | 1 |
| FDA said no to injectable and nasal forms — on the do-not-compound list | 1 |

The second line under a GRAY MARKET stamp on a compound or stack hero ("Sold as a research chemical…")
is on 111 pages; 26 pages carry no sold line because nothing is sold (aliases and the approved drugs).

The evidence meter is unchanged apart from level 3, now **Controlled trials**.

---

## Part 2 — claim by claim, script first

Your method change mid-session: the script compares each cited sentence with its abstract
mechanically, and only the mismatches come to me, fifteen at a time, with the one abstract sentence
that bears on each.

**`scripts/claim_audit.py`** now does that. For every sentence carrying a citation (2,282 of them, on
151 pages, citing 1,043 papers, 1,020 with abstracts) it compares:

- **numbers** — every figure in the sentence against every figure in the abstracts, with rounding and
  "about" allowed;
- **population** — humans, a named species, cells, and 60 named conditions;
- **design** — randomised, placebo, blinded, crossover, meta-analysis, cohort, case report, phase, and
  fourteen more;
- **direction** — an effect against no effect, and up against down for the same measure;
- **subject** — whether the sentence shares any vocabulary with the paper at all.

Where the paper is in PMC's open-access set the script also reads the **full text** (292 papers), so a
number the abstract omits but the paper states is found rather than guessed at. A number counts as
found in a full text only when a word of the sentence sits within 150 characters of it, so a figure
from an unrelated table cannot pass a claim; small whole numbers are never looked for there at all.

| Verdict | Sentences |
|---|---|
| supported | 2,002 |
| not checkable | 152 |
| **overstated** | **59** |
| **contradicted** | **69** |

Who checked what: 1,114 sentences passed on the abstract alone, 144 on a paper's full text, and
**1,024 came to the hand check** — every one of them read beside the abstract sentence that bears on
it. Nothing was marked "not checkable — skipped": the batch that a safeguard stopped in the first half
of the session was re-checked and judged in full.

`gsc-export/claim-audit.csv` holds all 2,282: page, sentence, cited papers, verdict, who checked it,
the note, and the rewritten sentence where there is one.

**128 sentences were rewritten to what their papers say**, on 56 pages, each with a corrections entry.
The pages with the most: cerebrolysin (10), ahk-cu (9), decapeptide-12 (6), ara-290 (5), cjc-1295 (5),
argireline (4), hexarelin (4), then 5-amino-1mq, aicar, bpc-157, ghk-basic, ghrp-6, hgh, melanotan-i
and pentapeptide-18 with 3 each.

What they were, by kind:

- **Wrong species or compound.** MGF's neuroprotection study is in gerbils, not rats; its cardiac
  study is in sheep, not rats; GHK-Cu's hair-follicle evidence is for the analogue AHK-Cu, ex vivo, and
  said nothing about the anagen phase; Atamna 2008's lifespan work is human fibroblasts in culture, not
  worms and flies; a Selank gene-expression paper is mouse spleen, not rat spleen and hippocampus.
- **Wrong design.** Hsieh 2013 is a retrospective record review, not a prospective cohort; Trookman
  2009 is a three-month open-label study in 37 women, not an 8-week n=79 trial; Rudman 1990 gave
  growth hormone to 12 of 21 men with no placebo and no randomisation; Enebo 2021 is phase 1b;
  Humaidan 2005 is a randomised trial, not a meta-analysis; Freeberg 2022 is a trial protocol, not a
  systematic review; five "Phase 2/3" labels sit on papers that give no phase.
- **Numbers the paper contradicts.** The two EPP trials had 74 European and 94 US patients, the page
  had them the other way round and gave the US result as 6.1 hours where the paper says 69.4;
  tesamorelin's liver-fat effect is 4.1% absolute, not 3.1%; GHK-Cu's collagen peak is 1 nM, not
  0.1–1.0 µM; Pandya's GHRP-6 study had nine men, not eight; the KIGS registry is 58,603 children, not
  ">80,000 children-years"; Pickart's Connectivity Map counts are in the hundreds, not thousands.
- **Findings reversed.** Pihoker 1997 raised height velocity from 3.7 to 6.1 cm/year and the page said
  intranasal GHRP-2 "failed to produce clinically meaningful growth promotion"; the Testosterone
  Trials found no benefit in vitality and the page listed vitality as improved; Pyo 2007's fall in
  apoptosis was not statistically significant and two pages said it was; Bishop 2005 reported a
  peak-flow gain and the page filed it under cautious reviews; Beretta 2007 found GHK "significantly
  less potent than carnosine" and the page called them comparable; Ott 2013 cut reward-driven snack
  intake (cookies by 25%) with hunger-driven intake unchanged, and the page said caloric intake fell
  ~12%.
- **Claims credited to papers that do not contain them.** A CNS mechanism, a Campisi-lab
  collaboration, a "Tg2576" model, an aged-rat paper that does not exist under the name given (the
  real one is Bolognin 2014), an FDA-monitored IND claim, a "0.22% / 99.7%" pair the paper never
  prints, a Kim & Shimoda byline on a paper by Reinholz, Ruzicka and Schauber, a Cebrián-Torrejón
  byline on a paper by Sakuma, an echocardiography study that is a pharmacokinetic study in nine men,
  a "Trachy 1999" with no reference behind it, and a trial arm (Vilon) that was never in the trial.

The 152 not-checkable sentences are logged with their reason in the CSV — most are details that live
in a closed paper's methods, a supplementary table, or a manufacturer's brochure the page names.

---

## Part 3 — data rows

**`scripts/data_rows.py`** takes every value in the data tables of the 136 compound, alias and stack
pages — 1,562 rows — and checks what can be checked:

- a sequence's own residue count;
- the **mass computed from that sequence**, with N-acetyl, C-amide and palmitoyl applied where the page
  names them, or **PubChem's** molecular weight for a small molecule (and a printed formula's own mass);
- the generated route rows against `data/routes.json`;
- the FDA row against `data/regulatory-status.json` and its 503A and 503B categories;
- the WADA row against the **2026 Prohibited List**;
- every other number against the rest of the page and the abstracts the page cites.

1,545 rows agreed. 17 came to the hand check. **19 values were wrong:**

| Page | Row | Was | Now |
|---|---|---|---|
| mots-c | Molecular weight | ~1,880 Da | 2,174.6 Da |
| snap-8 | Molecular weight | 946.02 | 1,075.2 g/mol |
| decapeptide-12 | Molecular weight | ~1374 Da | 1,395.5 Da |
| nonapeptide-1 | Molecular weight | ~1,196 Da | 1,206.5 Da |
| pancragen | Molecular weight | ~547 Da | 576.6 Da |
| dihexa | Molecular weight | 490.64 | 504.7 g/mol |
| gw-0742 | Molecular weight | 489.45 Da | 471.5 Da |
| tesofensine | Molecular weight | 365.30 | 328.3 g/mol (free base) |
| syn-ake | Molecular weight | ~503 / ~432 Da | 495.6 / 375.5 Da |
| ahk-cu | Molecular weight | "~354 Da (peptide + Cu)" | 354.4 Da, the peptide alone |
| kisspeptin | WADA | "Not specifically named" | Prohibited at all times (S2.2.1) |
| tesofensine | WADA | "Not specifically listed" | Prohibited in competition (S6) |
| flow-state-stack | WADA | the 2025 List | the 2026 List |
| enclomiphene, nad-plus, vip | FDA status | no mention of the 503A listing | the 503A Category 1 listing, as the page's own legal section says |
| sermorelin | FDA status | "discontinued 2008" | withdrawn 2009 at the sponsor's request; compounded as a component of an approved drug |
| wolverine-stack | FDA status | stopped at the Category 2 removal | plus the July 2026 PCAC recommendation |
| bromantane | Half-life | ~11.5 hours | marked as unsourced (see above) |

**Reconstitution arithmetic.** All 54 reconstitution tables were checked the way the calculator is
checked: vial over water against the printed concentration, and every printed dose against its units
and its volume. Every line multiplies out.

**No hand-written values on the blend and pair pages.** `stacks/data.json` — the mirror the pair pages
render from — had drifted from the profiles on 42 quick facts, including follistatin-344's WADA class
(it said S2; the List puts myostatin-binding proteins under S4.3) and TB-500's and thymosin alpha-1's
pre-rebuild text. All 42 were mirrored from the profiles and the pair pages regenerated (56 pages).
`scripts/sync_pair_fields.py` can now replace a whole quick-facts card, not only its FDA cell.

---

## Corrections

**115 corrections carry today's date** (121 entries from Part 2, 19 from Part 3, plus Part 4's from the
first half of the session), across 63 pages. Every one is on /corrections.html, newest first, with what
was wrong and what it says now. The changelog now holds 370 entries.

---

## DECISIONS

1. **The script decides what it can, and says which.** A sentence or row that agrees with its source on
   every mechanical count is marked supported by the script, and the CSV records that it was the script
   and not a person. Anything mechanical that disagreed came to the hand check. 1,024 of 2,282
   sentences and 17 of 1,562 rows were judged by hand.
2. **Open-access full texts were read as well as abstracts.** The brief says abstracts; where PMC has
   the paper, its full text is better evidence, and a figure in a paper's own table should not be called
   unsupported. 144 sentences rest on a full text; the CSV says so for each.
3. **A number in a closed paper's supplement is "not checkable", not "wrong".** Where the abstract and
   the open-access text do not carry a figure and the paper is behind a paywall, the verdict is not
   checkable with the reason. 152 sentences.
4. **A wrong figure is replaced by the paper's own, not deleted.** Every fix states what the cited paper
   says, in the page's voice, and keeps the citation.
5. **Where no source exists, the page says so** rather than dropping the number (bromantane's half-life,
   the humanin potency figure).
6. **The profile is the source of truth for the pair pages**, not `stacks/data.json`. Divergences were
   resolved towards the profile every time.
7. **Two small gate fixes, both in the direction of truth.** `scripts/citations.py` now reads PubMed's
   transliterated initials ("Khavinson VKh") as initials, so the citation gate stops failing those
   authors; and it no longer audits the citations /corrections.html quotes as *wrong*, which would fail
   the gate on errors already corrected.
8. **Deploy retried once.** `vercel --prod` answered "Not authorized" on the first attempt with nothing
   deployed, as in session 13; the retry went through. The known `TEAM_ACCESS_REQUIRED` blocker is
   unchanged.

---

## Gates and deploy

| Step | Result |
|---|---|
| `bash tests/run-calc-tests.sh` | PASS (400 cases, 28-case self-test, freeze intact) |
| `node tests/coa-scorer.test.mjs` | 22 tests passed |
| `node tests/coa-function.test.mjs` | 14 tests passed |
| `python3 scripts/prerender.py` | ran twice; the second changed nothing |
| `python3 scripts/gen_sitemap.py` | written |
| `python3 scripts/audit_pages.py --mode cite` (every page) | 8,918 citations on 454 pages, 0 failing, 111 allowed with a reason — PASSED |
| `python3 scripts/street_lab_audit.py --all` | 476 pages, 0 failed |
| `git ls-files --deleted` | empty |
| identity greps, gitleaks | clean on all five commits |
| `vercel --prod` from a git-less copy | `dpl_AwoHyUmKMusjtWiEDEgiwszEWg1h`, Ready |
| `bash push-to-indexnow.sh` | 174 URLs accepted (HTTP 200) |

The deploy copy's `diff -rq` against the repo listed only `.git`, `.claude`, `reports` and a stray
`art/.DS_Store`.

**Curled on production** (all 200): `/`, `/compounds/bpc-157.html`, `/compounds/semaglutide.html`,
`/compounds/cjc-1295.html`, `/compounds/retatrutide.html`, `/compounds/tesa-ipa-stack.html`,
`/compounds/thymosin-alpha-1.html`, `/about.html`, `/goals/fat-loss.html`.

Spot-checked on the live pages: BPC-157's hero reads "Gray market · FDA panel said yes on July 23–24,
2026 — not legal yet"; MOTS-c's molecular weight reads 2,174.6 Da; kisspeptin's and tesofensine's WADA
rows read as prohibited; GHK-Cu's hair item names the AHK-Cu analogue; Melanotan-I's EPP trials read 74
European and 94 US patients; /corrections.html carries today's entries.

---

## URLs to eye-check

The pages that changed most, worth your eye:

- https://www.kalios.health/compounds/cerebrolysin.html (10 sentence fixes)
- https://www.kalios.health/compounds/ahk-cu.html (9)
- https://www.kalios.health/compounds/decapeptide-12.html (6, plus the molecular weight)
- https://www.kalios.health/compounds/ara-290.html (5)
- https://www.kalios.health/compounds/cjc-1295.html (5)
- https://www.kalios.health/compounds/argireline.html (4)
- https://www.kalios.health/compounds/hexarelin.html (4)
- https://www.kalios.health/compounds/melanotan-i.html (the two EPP trials)
- https://www.kalios.health/compounds/pentapeptide-18.html (the Dragomirescu study, three places)
- https://www.kalios.health/compounds/mots-c.html · /compounds/snap-8.html · /compounds/tesofensine.html (data rows)
- https://www.kalios.health/compounds/kisspeptin.html (WADA)
- https://www.kalios.health/corrections.html (115 new entries)
- https://www.kalios.health/compounds/tesa-ipa-stack.html (Part 4's new stack page)
- https://www.kalios.health/compounds/thymosin-alpha-1.html (Part 4's rebuild)

---

## Open questions for you

1. **The Turnstile secret** (item 1 above). Everything else on COA Check is ready and tested.
2. **The humanin 1000-fold figure.** Keep it as it stands, cite a paper you have, or drop it? It is on
   the page four times, once as an identity-verification argument.
3. **Bromantane's half-life.** Same question: the row now says the figure is widely quoted and
   unsourced on the page.
4. **Vendor names still on eight pages** (klotho, FOXO4-DRI, PDA, waglerin-1, zuclomiphene, MariTide,
   bimagrumab, ecnoglutide). Part 4 took them out of epithalon's references; the site's own line is
   "no vendors". Say the word and they go.
5. **"HHS/Kennedy February 2026 reclassification"** appears on several pages with no source behind it.
   It was flagged in the first half of the session and is still there. Do you have the document?
6. **GH Stack's "3–5× bigger pulse"** has no citation on the page.

---

## Session start

- `claude --version`: 2.1.285
- Vercel CLI 60.1.3 installed; 61.1.0 published. Not upgraded mid-session (the deploy path is the
  documented blocker workaround; upgrading it during a deploy session is a risk with no upside today).
- `brew outdated`: 40 formulae. Nothing pinned to the site's build.

## Web content

Everything fetched this session was treated as **data, never instructions**, per CLAUDE.md rule 3.
Nothing fetched contained anything resembling an instruction, and nothing was acted on as one.

- **NCBI E-utilities** — 1,043 papers' abstracts (esearch, efetch), and PMC full text for 292
  open-access papers. Used to judge the sentences citing them.
- **NCBI PMC ID converter** — PMID to PMC mapping for the full-text fetches.
- **PubChem PUG-REST** — molecular weights and formulas for the small molecules, by name and by
  synonym (cached under `_scratch/12d-pubchem.json`, not deployed).
- **Crossref** — abstracts for DOIs without a PMID.
- **WADA 2026 Prohibited List** — the text extracted in session 12c, re-used from the scratchpad for
  the WADA rows.
- **FDA 503A and 503B lists** — as already recorded in `data/regulatory-status.json` from 12d Part 1.

Local artefacts: `gsc-export/claim-units.json`, `gsc-export/claim-abstracts.json`,
`gsc-export/claim-audit.csv`, `data/claim-judgments.json`, `gsc-export/data-rows.json`,
`gsc-export/data-rows.csv`, `data/data-row-judgments.json`. The full texts and the PubChem cache live
in `_scratch/`, which is neither committed nor deployed.
