# Identity, workflow and root truth fixes — 2026-09-27

Parts A–E are done. The whole git history is now authored by **Kalios**, and a public reports repo exists with a guarded publish script. Five root-page truth fixes are live, and CLAUDE.md is updated.

- **Deploy:** `dpl_FJqG2UzgLdH9J7Acc5ANWZYqwwUD`, built from commit `97b6feb`, serving www.kalios.health. IndexNow accepted 448 URLs (HTTP 200).
- **Vercel author block:** still **BLOCKED** after the rewrite (A.5). The git-less deploy method stays.
- **Reports repo:** https://github.com/kalios-health/kalios-reports (public). 3 reports are published, including this one, and 3 are held (see DECISIONS 12).

**Session start (rule 2)**
- Claude Code 2.1.283.
- Vercel CLI 60.1.3, the same as the latest on npm.
- `brew outdated` lists 39 formulae, including `git`, `node` and `python@3.12`. None were updated. `git-filter-repo` 2.47.0 was newly installed.
- There are no pinned CDN libraries to bump: the site only loads the unversioned Tailwind Play CDN and the Umami script.

---

## Status

| Part | What | Result | Commit |
|---|---|---|---|
| A1 | `origin` → `github.com/kalios-health/kalios` | **DONE**: `git remote -v` and `git fetch` OK | — |
| A2 | Rewrite all history to Kalios; scrub identity strings | **DONE**: 285/285 commits | history rewrite |
| A2 | Re-scrub the repo-visibility question file | **DONE** | `481a92b` |
| A3 | Repo-local identity Kalios; no trailers | **DONE** | — |
| A4 | Force-push and verify | **DONE**: 1 identity, 0 hits, gitleaks clean | `481a92b` |
| A5 | Preview-deploy test of the Vercel author block | **Still BLOCKED**, so the git-less method is kept | — |
| B | Public reports repo + `scripts/publish_report.sh` + CLAUDE.md rule 1 | **DONE** | `f8bf105` |
| C | Five root truth fixes, prerender, deploy, IndexNow, curl | **DONE**: 5/5 live pages verified | `97b6feb` |
| D | CLAUDE.md: word floor retired, identity rule, repo private | **DONE** | `38abf82` |
| E | This report, published with the new script | **DONE** | this commit |

---

## Part A: git identity

**What changed**
- Every commit's author and committer is now `Kalios <kaliospeptides@proton.me>`. Before the rewrite there were 3 distinct identities: 2 commits, 69 commits and 214 commits.
- `git filter-repo --replace-text` ran on every file version and `--replace-message` on every commit message. The expressions covered:
  - the legal name,
  - the ISP hostname (and its masked form),
  - the machine hostname,
  - the personal handle: the account's former GitHub username and the Vercel team slug.
  - The expressions file lived only in the session scratchpad and was never committed.
- In file contents, these strings existed only in the reports and in `voice-rewrite-log.md`. They are replaced with `[redacted]`-style placeholders. The former GitHub username became `kalios-health`, which is the same account after the rename.
- The repo-visibility question file now opens with an "Answered" banner. Its old note, which said the first version spelled the strings out, is gone.
- 54 commit SHAs cited in reports, logs and `.gitleaksignore` were remapped to the rewritten history through filter-repo's commit map, so `git show <sha>` and `git revert <sha>` work again.
- Repo-local config is `user.name "Kalios"`, `user.email kaliospeptides@proton.me`, and new commits carry no `Co-Authored-By` trailer.
- Force-push used `--force-with-lease` pinned to the pre-rewrite remote SHA.

**Verification (after the push)**
- `git log --all --format='%an <%ae> %cn <%ce>' | sort -u` prints one line: `Kalios <kaliospeptides@proton.me> Kalios <kaliospeptides@proton.me>`.
- An identity-string grep over **every object in the store**, 15,808 objects including binary and unreachable ones, found **0** hits. The same grep over all commit messages, the rest of `.git` and the working tree also found 0.
- `gitleaks git --log-opts="--all"` scanned 283 of 286 commits (3 only delete files) and found **no leaks**. The 3 triaged false positives still match after the remap.
- GitHub's commits API now attributes the commits to `kalios-health`. Before the rewrite it returned `author: null`.

**A.5: Vercel preview test.** `vercel` (a preview deploy) from `~/kalios` came back **Blocked**: `seatBlock: TEAM_ACCESS_REQUIRED`, `isVerified: false`, deployment `dpl_J8MC1iZmgj62zLLVpJS3DxUmg4ak`.
- The deployment metadata carries the author's name, email and GitHub org, but **no `githubCommitAuthorLogin`**. Vercel is not resolving the commit to a GitHub login, even though GitHub itself now does.
- The project has no Git link (`link: null`).
- So the fix now sits on the Vercel side (open question 1). Production deploys keep using the git-less copy, which is now documented in CLAUDE.md rule 4.

## Part B: reports flow

- **`kalios-health/kalios-reports`** is public, with wiki and issues off. It holds top-level `*.md` reports and nothing else. The local clone is `~/kalios-reports`, outside the deploy tree, with repo-local identity Kalios.
- **`bash scripts/publish_report.sh reports/<file>.md [...]`** gates every file before the public repo is touched. It refuses a file that:
  - is outside `reports/` or isn't `.md`,
  - matches the identity denylist (`~/.config/kalios/identity-denylist`, outside the repo, mode 600; a missing or empty denylist fails closed),
  - or is flagged by gitleaks.
- One bad file stops the whole run. The script commits as Kalios, pushes, and is idempotent.
- All refusal paths were tested with a throwaway token before first use. Published files were fetched anonymously from GitHub and are byte-identical to the originals.
- CLAUDE.md rule 1 now says reports are written to `/reports/` **and** published with the script. Reports are public: no identity strings or secrets, and deploys are cited by `dpl_…` ID.

## Part C: root truth fixes (sources below)

1. **`fda-pcac-2026.html`**: the removal story is corrected everywhere on the page: the lead, the "Three legal events" box (no longer framed as a "precondition"), the Key Dates timeline and the FAQ JSON-LD.
   - FDA's list updated **April 15, 2026** gave seven days' notice that the 12 peptides would leave Category 2 "because the nominations were withdrawn by the nominators".
   - The list updated **April 22, 2026** records them as removed.
   - The page no longer says they were "pulled for scientific review", and no longer says the removal was done April 15.
   - The timeline now has separate April 15 (notice) and April 22 (effective) entries, and says the Federal Register meeting notice was published April 16.
   - Both archived FDA lists were added to Primary Sources.
2. **`index.html`**:
   - The FAQ answer's pre-meeting "12 peptides under PCAC review" line now reads: 7 voted July 23–24, 2026; 5 scheduled before the end of February 2027; a PCAC vote is advisory, so none of the 12 is legal to compound today.
   - The orforglipron card hook drops the Phase 2 14.7% figure. It now reads "11.2% weight loss at 72 weeks in Phase 3 — a daily pill, no injections." That's the profile's own Phase 3 language (ATTAIN-1), confirmed against the NEJM abstract.
   - `DESC_MAP`, `data/compound-desc.json` and the prerendered card all carry the new hook.
3. **`beginners-guide.html`**: "In 2026, that's being reviewed" is replaced with the current state: April 2026 removal after the withdrawn nominations, 7 voted, 5 scheduled, none legal to compound today.
4. **`compounds/tesamorelin.html`**: EGRIFTA SV 2 mg vials are stored at **20–25°C (68–77°F)**, with excursions permitted to 15–30°C, in the original box to protect from light. That is the label, section 16. The page said "refrigerated 2–8°C". The label is cited inline and added to Key References.
5. **`compounds/semaglutide.html`**: FDA determined the semaglutide injection shortage **resolved on February 21, 2025**. This was checked against FDA's declaratory order of that date and FDA's own statement page. Both "late 2024" claims and "2022–2024" are corrected, and the order is added to Key References. Tirzepatide's "late 2024" is correct and was left alone.

Checks:
- Every edit was an exact-match replacement asserted to occur once.
- Tag sequences changed only where rows were added (1 timeline row and 2 source rows on the tracker; 1 reference each on tesamorelin and semaglutide).
- All JSON-LD blocks parse.
- Prerender changed only the orforglipron card.
- `gen_sitemap.py` reproduces `sitemap.xml` exactly after the commit.
- Live: all 5 pages return 200, are **byte-identical to the commit**, contain the new text, and no longer contain the old text.
- `/reports/*`, `/CLAUDE.md`, `/scripts/*`, `/.claude/*` and `/.gitleaksignore` still return 404.

## Part D: CLAUDE.md

- **Quality standard:** truth and clarity, not length. The 4,000-word floor and its word-count rules are retired.
- **New rule 7 (Identity):**
  - The author is Kalios, with the repo-local config above and no attribution trailers.
  - No legal names, personal hostnames or handles in the repo, the reports or commits.
  - The denylist stays outside the repo.
  - Two pre-push checks (author list and denylist grep) were tested in zsh and bash.
- **Tech stack:** the repo is private by decision. If that ever changes, publish through a fresh repo, because GitHub keeps this force-pushed repo's pre-rewrite objects reachable by SHA until its own GC runs.
- **Rule 4:** the blocker is retested and still present; the git-less copy method is written out.

---

## DECISIONS

1. **Scrub scope.** The expressions covered the machine hostname, which contains the first name, and the personal handle as well as the two named strings. The handle appears as the former GitHub username and as the Vercel team slug inside `*.vercel.app` deploy URLs. It is the local part of a personal email, and reports are now public. The rewrite was happening anyway, so this added no extra SHA churn.
2. **Identities.** Name and email callbacks mapped *every* historical author and committer to Kalios, including the old automation identity whose email carried the ISP hostname.
3. **Trailers.** Old `Co-Authored-By` lines in historical messages were left as they are, since the instruction was "going forward". New commits have none.
4. **Backup.** A verified `git bundle` of the pre-rewrite history is at `~/kalios-backup/kalios-pre-identity-rewrite-2026-09-27.bundle`, outside the repo and never pushed. It still contains the old identity strings. Delete it once you're satisfied.
5. **Force-push safety.** `--force-with-lease` was pinned to the pre-rewrite remote SHA, so the push couldn't clobber an unexpected remote change.
6. **SHA remap.** Cited SHAs in reports, logs and `.gitleaksignore` were rewritten to the new history. Stale SHAs would break `git revert` (rule 6) and would reopen the 3 triaged gitleaks findings.
7. **A.5 outcome.** The preview was BLOCKED, so the git-less method stays. The blocked preview deployment was left in place; it was never served.
8. **Reports repo shape.** Wiki and issues are disabled so the repo holds nothing but reports. The default branch is `main`, and the clone sits outside `~/kalios` so `vercel --prod` can never upload it.
9. **Fail-closed script.** A missing or empty denylist, a gitleaks hit, a non-report path or a non-`.md` file refuses the whole run.
10. **Denylist location.** `~/.config/kalios/identity-denylist` is outside the repo and deploy tree, so the guarded strings never enter git.
11. **Reports clone identity.** `~/kalios-reports` got repo-local identity Kalios. The script's own commits already force it, but a manual commit there would otherwise use the machine's global git identity.
12. **First publish: 2 published, 3 held.**
    - Published: `overnight` and `content-truth`, after a full read. Both are clean of identity strings and secrets.
    - Held: `ALERT-brave-key` and `workflow-closeout`. They describe an open security item in enough detail to help an attacker until it's closed. Publish them once it is (open question 2).
    - Held: `question-repo-visibility`. Its subject is the owner's identity exposure. It is scrubbed in the private repo but not needed publicly.
13. **Orforglipron hook.** I chose the profile's Phase 3 language over "no number". It matches the profile's Gist and was confirmed against the primary paper. It carries "in Phase 3" as a qualifier.
14. **Homepage FAQ.** It states 7 voted, 5 scheduled and none legal, and points to the tracker for vote counts. It does not restate vote outcomes, because FDA's meeting page (content current as of 08/06/2026) posts no vote summary to re-verify them against.
15. **Dates.**
    - The tracker is now "Last updated: September 27, 2026" with a matching `dateModified`. Tesamorelin and semaglutide are now "Last updated: September 2026" with `dateModified` 2026-09-27. These are real content changes.
    - The beginners-guide has no date field.
    - The sitemap lastmod for the tracker and the guide was set to the commit date, so Part C stays one commit and `gen_sitemap.py` reproduces it (verified).
16. **Scope.** Inaccuracies found while verifying, but outside the five items, were **flagged, not fixed** (open questions 5–10).

## Counts

- **History:** 285 commits rewritten, identities 3 → 1, 16 replace-text expressions. Identity strings were removed from 5 files' history. 10,309 blobs were scanned before and after, and 15,808 objects after the rewrite, with 0 hits.
- **SHA remap:** 54 cited SHAs, 51 in 6 reports and logs plus 3 in `.gitleaksignore`.
- **Commits this session:** 5 (`481a92b`, `f8bf105`, `97b6feb`, `38abf82` and this report), plus the rewritten history.
- **Part C:** 20 exact-match edits in 6 files plus 2 sitemap lastmods. 5 live pages verified, and IndexNow accepted 448 URLs.
- **Reports repo:** 3 published, 3 held.

## Files changed

- **Part A:** full history (content changes in 4 reports and `voice-rewrite-log.md`). The follow-up commit changed `.gitleaksignore`, the 5 existing `reports/2026-09-27-*.md` files and `voice-rewrite-log.md`.
- **Part B:** `scripts/publish_report.sh` (new), `CLAUDE.md`.
- **Part C:** `fda-pcac-2026.html`, `index.html`, `data/compound-desc.json`, `beginners-guide.html`, `compounds/tesamorelin.html`, `compounds/semaglutide.html`, `sitemap.xml`.
- **Part D:** `CLAUDE.md`.
- **Part E:** `reports/2026-09-27-identity-workflow.md`.
- **Outside the repo:**
  - `~/.config/kalios/identity-denylist` (new)
  - `~/kalios-reports` (clone)
  - `~/kalios-backup/…bundle`
  - GitHub repo `kalios-health/kalios-reports` (new, public)

## Deploy

- **Production:** `dpl_FJqG2UzgLdH9J7Acc5ANWZYqwwUD`, 18:37 ET, built from `97b6feb`.
  - It was deployed from a git-less copy: `rsync` without `.git`, `.claude`, `reports` or `.DS_Store`, and `diff -rq` showed only those paths differ.
  - It serves www.kalios.health; the apex returns 308 to www.
- **IndexNow:** 448 URLs, HTTP 200.
- Parts D and E only touch `*.md` files, which are excluded from deploys, so they need no redeploy.

## URLs to eye-check

1. https://www.kalios.health/fda-pcac-2026.html: the "Three legal events" box, Key Dates and Primary Sources.
2. https://www.kalios.health/: the Orforglipron card (Weight Loss). `view-source:` for the FAQ JSON-LD.
3. https://www.kalios.health/beginners-guide.html: the paragraph on 503A pharmacies.
4. https://www.kalios.health/compounds/tesamorelin.html: the Storage bullet under Reconstitution & Storage, and the last reference.
5. https://www.kalios.health/compounds/semaglutide.html: the two compounded-semaglutide paragraphs, and the last reference.
6. https://github.com/kalios-health/kalios-reports

## Web content (rule 3)

- All of it was fetched as data only:
  - fda.gov: the 503A categories PDF (May 14 version), the GLP-1 compounding statement, the semaglutide declaratory order and the PCAC meeting page.
  - web.archive.org: the April 17 and April 22, 2026 snapshots of the categories PDF.
  - federalregister.gov: document 2026-07361.
  - api.fda.gov: the tesamorelin labels.
  - dailymed.nlm.nih.gov: link check only.
  - PubMed E-utilities: ATTAIN-1.
  - A web search restricted to fda.gov.
- No instruction-like text was found in any fetched page, and nothing fetched was acted on as an instruction.

## Sources

- FDA, *Bulk Drug Substances Nominated for Use in Compounding Under Section 503A*:
  - [updated April 15, 2026 (archived)](https://web.archive.org/web/20260417111839/https://www.fda.gov/media/94155/download)
  - [updated April 22, 2026 (archived)](https://web.archive.org/web/20260422215502/https://www.fda.gov/media/94155/download)
  - [current version](https://www.fda.gov/media/94155/download)
- [Federal Register 2026-07361, 91 FR 20465 (published April 16, 2026)](https://www.federalregister.gov/documents/2026/04/16/2026-07361/pharmacy-compounding-advisory-committee-notice-of-meeting-establishment-of-a-public-docket-request)
- [FDA, July 23–24, 2026 PCAC meeting page](https://www.fda.gov/advisory-committees/advisory-committee-calendar/july-23-24-2026-meeting-pharmacy-compounding-advisory-committee-07232026)
- [Wharton S et al. ATTAIN-1, N Engl J Med 2025;393:1796–1806, PMID 40960239](https://pubmed.ncbi.nlm.nih.gov/40960239/)
- [EGRIFTA SV prescribing information, section 16 (DailyMed set 3d783378-b02d-4f19-99dd-0fc91a042224)](https://dailymed.nlm.nih.gov/dailymed/drugInfo.cfm?setid=3d783378-b02d-4f19-99dd-0fc91a042224)
- [FDA, Declaratory Order: Resolution of Shortages of Semaglutide Injection Products, February 21, 2025](https://www.fda.gov/media/185526/download)
- [FDA, "FDA clarifies policies for compounders as national GLP-1 supply begins to stabilize" (2/21/2025 entry)](https://www.fda.gov/drugs/drug-alerts-and-statements/fda-clarifies-policies-compounders-national-glp-1-supply-begins-stabilize)

---

## Open questions for G

1. **Vercel author block.** GitHub now links the commit email to `kalios-health`, but Vercel still records no GitHub login for the author. Options:
   - (a) In Vercel → Account Settings → Authentication, connect the `kalios-health` GitHub login. This is the first thing to check.
   - (b) Connect the project to the GitHub repo. Note that pushes would then auto-deploy, which changes the workflow.
   - (c) Keep the git-less method, which works.
2. **Held reports.** Once the open security item carried over from the last sessions is closed, publish the ALERT and workflow-closeout reports with `bash scripts/publish_report.sh reports/2026-09-27-ALERT-brave-key.md reports/2026-09-27-workflow-closeout.md`. Should the repo-visibility report ever be public? My default is no.
3. **Vercel team slug.** It is the personal handle and appears in every `*.vercel.app` deploy URL and dashboard link. Rename it to a brand slug? Until then, reports cite `dpl_…` IDs, and the denylist blocks the handle.
4. **Housekeeping.**
   - Delete `~/kalios-backup/…bundle` when satisfied.
   - Optionally ask GitHub Support to garbage-collect the private repo, so pre-rewrite objects stop being reachable by SHA.
   - **Resolved after this report:** the machine's *global* git identity is now `Kalios <kaliospeptides@proton.me>` as well. No other scope or environment variable overrides it, so new repos inherit it.
5. **Tracker, GHK-Cu route.** The FAQ and "What happens next" list say the 2027 PCAC review covers "GHK-Cu (noninjectable routes only)". FDA's May 14 list says FDA intends to consult the PCAC on "GHK-Cu", with no route given, and it put non-injectable GHK-Cu back in Category 1. This ties into the open GHK-Cu badge question.
6. **"12 peptides" at the July meeting.** The tracker's dek, its JSON-LD `description` and `llms.txt` call the July 23–24 meeting a vote on (or review of) 12 peptides. It voted on 7. Suggested wording: "the PCAC review of 12 peptides (7 voted July 23–24, 2026)".
7. **beginners-guide.** The sentence before the fixed one says most people get BPC-157, ipamorelin or sermorelin through a 503A pharmacy. BPC-157 and ipamorelin are not legal to compound under 503A today. Rewrite it?
8. **tesamorelin reconstitution table.**
   - The "Egrifta SV (branded)" row says "1.4 mg lyophilized vial, reconstituted with 2.1 mL". The label describes a **2 mg** single-dose vial whose 1.4 mg dose is 0.35 mL of the reconstituted solution.
   - The page also calls EGRIFTA WR "a room-temperature-stable reformulation", but SV's label is room temperature too. WR's label describes an 11.6 mg vial mixed weekly with bacteriostatic water.
   - WR's "January 2025" launch date was not verified.
9. **Orforglipron dose naming.** 2026 ATTAIN-1 papers report the doses as 5.5 / 9 / 17.2 mg, while the profile uses the trial's original 6 / 12 / 36 mg. Reconcile them against the Foundayo label's strengths?
10. **compare/semaglutide-vs-tirzepatide.html.**
    - It says the manufacturers "declared their products shortage-resolved". FDA made those determinations.
    - It says telehealth channels were "active through 2023 and early 2024". The semaglutide shortage ran to February 2025.
11. **Carried over.** Everything open from the content-truth report still stands, apart from the items fixed here: root pages, EGRIFTA SV storage and the semaglutide date. Brew has 39 outdated formulae.
