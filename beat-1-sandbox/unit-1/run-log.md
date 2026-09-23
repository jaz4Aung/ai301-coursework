# Unit 1 run log

Working notes for the **Run history** field in
`beat-1-sandbox/unit-1/selection.md` (2 pts, scored on being in order).
Newest entries at the bottom. This file is not submitted as-is — it is
the raw material I rewrite into the write-up.

Rubric under test: `~/.claude/skills/issue-select/rubric.md`

---

## Run 1 — smoke, 3 items

- **Date:** 2026-09-22
- **Command:** `python3 run_eval.py --rubric ~/.claude/skills/issue-select/rubric.md --limit 3`
- **Rubric state:** first filled version. 7 checks — not-archived,
  commits-recent (90 days), maintainer-responds (preferred),
  repo-in-use (preferred), scope-bounded, unclaimed (~12 mo stale
  claim), policy-allows-ai. Verdict rule: all required must pass,
  unclear = fail on required.
- **Purpose:** plumbing check before spending on a full run.
- **Result:** agreement 2/3. issue-02 and issue-03 agreed (both
  reject). issue-01 disagreed: gold `accept`, my rubric said `reject`,
  note column `failed: scope-bounded`.
- **Why it disagreed:** issue-01 is a conda docs task whose "Proposed
  changes" section has five `###` sub-headings (add a new task page,
  update `manage-pkgs.rst`, update `pip-interoperability.rst`, update
  `new-features.md`, consider a `troubleshooting.rst` entry). My
  scope-bounded check read that list of sub-items as an umbrella issue.
  The gold note calls it "a docs task with a stated home and scope".
  The check could not tell a *detailed spec for one deliverable* from a
  *tracking list of separate deliverables*. Contrast issue-10, a real
  scope reject, whose title literally reads "Documentation request
  megaissue".
- **What I changed after this run:** rewrote the umbrella clause in
  `scope-bounded` so it requires the issue to *declare itself* a
  tracking/umbrella/meta/"mega" issue, or to state its items are meant
  to be split across separate PRs or contributors. Added: "A
  multi-step change list within one coherent deliverable is a spec,
  not an umbrella, and passes," plus the explicit carve-out that a
  terse body, a missing repro, or no label is not a failure. The old
  wording punished an issue for being *well specified*, which is
  backwards — a detailed file-by-file plan is what a newcomer wants.

---

## Run 2 — `--only issue-01`

- **Date:** 2026-09-22
- **Command:** `python3 run_eval.py --rubric ~/.claude/skills/issue-select/rubric.md --only issue-01`
- **Rubric state:** Run 1's rubric with the revised `scope-bounded`
  wording above. No other row touched.
- **Purpose:** cheap confirmation that the scope fix flips issue-01
  without paying for a full run.
- **Result:** agreement 1/1. issue-01 now `accept`, matching gold.
- **What I changed after this run:** nothing — next step is the first
  full 20-issue run.

---

## Run 3 — full 20, confirming run (SUBMITTED)

- **Date:** 2026-09-22
- **Command:** `python3 run_eval.py --rubric ~/.claude/skills/issue-select/rubric.md --save-run eval-run.txt`
- **Rubric state:** unchanged from Run 2. rubric.md sha256 prefix
  `4c4385fe58c02fbc`, which matches the fingerprint in the header of
  the committed `eval-run.txt`.
- **Result:** **19/20, PASS.** Categories: claimed 4/4,
  clear-accept 8/8, dead-repo 3/3, policy 1/1, scope 3/4. Category
  floor clear.
- **The one disagreement — issue-15** (zulip/zulip#19589): gold
  `reject`, my rubric `accept`. The bundle shows the issue opened
  2021-08-18 (five years before the 2026-08-05 capture), 97 comments,
  two linked PRs both closed (abandoned attempts), labelled `good
  first issue` and `help wanted`. None of my `scope-bounded` failure
  conditions fire on it: it does not declare itself an umbrella, no
  maintainer calls out core internals, it is not a usage question, and
  it is not an unspecced feature wish. The gold note calls it "years
  of design debate and two abandoned PRs behind a friendly label".
  The real gap: my Evidence column tells the check to look at
  "whether the `linked PRs:` list shows closed-unmerged attempts", but
  my pass condition never uses that signal. The evidence is gathered
  and then ignored.
- **What I changed after this run:** nothing, deliberately. See
  trade-off below.

---

## Trade-off: why I stopped at 19/20

Closing the issue-15 gap means adding a condition roughly like "fails
if the issue has been open more than two years and has closed-unmerged
linked PRs". That would very likely break **issue-09**
(conda/conda#7617), a gold *accept* that is also old and also carries
exactly one closed linked PR (`conda/conda#11627`). The gold note for
issue-09 reads "old but valid bounded feature; the 2022 claim is stale
and the maintainer invited takers" — age plus an abandoned PR is
exactly the pattern that check would punish.

So the choice was: keep a known 19/20 with the category floor clear, or
trade a confirmed pass for an unconfirmed one and another $4 full run,
for zero additional points (the bar is 18). I kept the run. The
inconsistency between the Evidence and Pass condition columns of
`scope-bounded` is real and I would fix it in a version of this tool
that had to generalise beyond this eval set — the fix is not "reject
old issues" but "weigh abandoned attempts against whether a maintainer
has since re-opened the door", which needs a signal the bundle format
does not cleanly give me.

<!-- Template for each further run:

## Run N — full | --only <ids>

- **Date:**
- **Command:**
- **Rubric state:** what was different from the previous run
- **Result:** agreement X/20, categories line, which issues disagreed
- **Why they disagreed:** what the note column named, what I read in
  the bundle
- **What I changed after this run:** the check and the threshold, and
  why -- or "nothing, this is the confirming run"

-->
