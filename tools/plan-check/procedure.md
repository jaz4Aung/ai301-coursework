# Procedure: how this skill grades a plan package

These are steps for GRADING a plan, not for writing or fixing one. Do
not rewrite the plan, suggest a better fix, or debug the issue
yourself. Your only job is to decide whether this package is ready to
post and build from.

Rebuilt from repro-check's Workflow steps 1-7, rewritten for plans.

## Read order

1. **Live mode only:** read `scope.md`.
   - If the `Repo:` line still has a placeholder, stop and tell the
     student to get the scope file from their instructor.
   - If the issue is not in the scoped repo, stop and refuse to grade.
   - Write down the house rules: own plan in own words, no
     piggybacking, branch `fix/<issue-number>-<slug>` on own fork.

   **Eval mode:** skip `scope.md` and `voice-guide.md` entirely, and do
   not fetch anything. The bundle is the only evidence.
2. Read `rubric.md`.
   - Write down each check's name exactly as it appears in the table,
     and its weight.
   - Write down the verdict rule.
3. Read `references/evidence-guide.md`. It is your map of which
   package block holds each kind of evidence.
4. Read the package in this order, and write down the listed items
   before you open the plan:
   1. **Issue:** the reported behavior, the expected behavior, and the
      version.
   2. **Repro evidence:** the environment, every numbered step,
      Expected/Actual, and any control step (for example, step 2 in
      calib-01, which proves the push succeeded). Note which step is
      the failing one.
   3. **Thread highlights:** any maintainer request, rejected approach,
      or claim/duplicate signal. "(no comments)" means none.
   4. **Repo facts:** testing conventions, contribution policy,
      review-bandwidth notes, and any AI-disclosure requirement.
   5. **Candidate plan:** the cause, the change (with In/Out), the
      files named, the tests, any risks or unknowns, and the
      `## Deviations` section if the plan has one. Note whether
      Deviations is empty (the build hasn't happened yet), says
      nothing changed, or records changes.
   6. **Candidate plan comment:** the claims it makes, including any
      promises about timing or outcome.

   Read the evidence before the plan. That way you know what the
   cause has to explain before the plan's wording can sway you.

## Evidence gathering

5. The rubric names plan parts with bold labels such as **Diagnosis**,
   **Scope**, **Changes**, **Test plan** and **Summary**. Packages often
   don't use those labels. Translate them like this:

   | Rubric label | What to find in the plan |
   |---|---|
   | **Diagnosis** | The sentence(s) claiming why the bug happens (e.g. `Cause:`) |
   | **Scope** | The In/Out statement and the named files or areas |
   | **Changes** | The sentence(s) describing what edit will be made where (e.g. `Change:`) |
   | **Test plan** | The sentence(s) describing how success will be checked (e.g. `Test:`) |
   | **Summary** | The plan's opening statement of what it will achieve, or the comment's one-line restatement |

   If a label has no matching sentence anywhere in the plan, write
   "absent". Do not invent one.
6. For each check, in rubric table order, copy the exact lines from
   the package that the check's Evidence column names. In eval mode,
   quote the bundle text word for word. In live mode, use the drafts
   for the plan side, and the issue page, the student's posted repro
   comment and the repo's CONTRIBUTING/templates for everything else.

   **Live mode, fetching the issue side.** Try `gh` first:
   `gh issue view <n> -R <owner>/<repo> --comments`. If `gh` is not
   installed (`command not found`, exit 127) or not logged in, use the
   public API instead, with no login needed for a public repo:
   - Issue: `curl -s https://api.github.com/repos/<owner>/<repo>/issues/<n>`
   - Comments: `curl -s "https://api.github.com/repos/<owner>/<repo>/issues/<n>/comments?per_page=100&page=<p>"`,
     raising `<p>` until a page comes back empty.

   Read each comment's `user.login`, `author_association` (`OWNER`,
   `MEMBER` or `COLLABORATOR` means maintainer) and `body`. Say in the
   summary which method you used. If neither works, grade the
   issue-side checks `unclear`. Do not grade from memory.

   **Live mode, already-posted comment.** Search the thread for a
   comment by the student whose text matches the draft plan comment,
   ignoring line wrapping. If one exists:
   - Grade the draft as usual. The verdict then means "was ready when
     posted" rather than "ready to post".
   - Name the posted comment's ID in the summary.
   - Quote any sentence where the posted text and the draft differ.
     Grade the comms check on the posted text, since that is what
     maintainers read.
7. For checks that read the plan against the repro evidence
   (diagnosis, tests, expected behavior), make a two-column list: each
   repro step on the left, and the plan line that explains or tests it
   on the right. Leave the right side blank where nothing matches.
8. If a check's Evidence column names something this package does not
   have (a specific flag, mode or control step from another issue),
   apply the check's general intent instead. For example, read a named
   control as "the control steps in this repro evidence". Report this
   gap in the rubric in the summary.

## Check execution

9. Grade the checks one at a time, in rubric table order. Grade each
   check using only its own pass condition and the evidence you
   gathered for it in steps 6-7.
10. Give each check exactly one grade:
    - **pass:** the gathered evidence meets the pass condition as
      written.
    - **fail:** the evidence contradicts the pass condition, or the
      plan part the condition needs is clearly inadequate. Examples: a
      cause that contradicts a repro step, a test that never re-runs
      the failing step, no Out line together with multi-subsystem
      edits.
    - **unclear:** the evidence the condition needs is genuinely
      absent from the package after you checked every location the
      evidence guide names. Do not use unclear because a check is hard
      to decide; if the evidence is there, decide.
11. Write a one-line evidence note for every grade: a quote, or a
    named fact (for example, "Test re-runs step 3 and requires the
    color to flip in view"). "Looks fine" is not evidence.
12. Only re-read the package when the evidence you gathered for a
    check doesn't settle it. Then re-read only the blocks that check's
    Evidence column names.
13. Grade what the plan says, not how it is written. Length, headings
    and confident tone never decide a grade.
14. If a check passes its stated condition but seems wrong to you,
    still grade it pass, and note the tension in the summary.

15. **Grading a Deviations section.** Deviations are part of the plan.
    Grade them under the existing checks; there is no separate check.
    - Empty, or "fill in after the build": not a defect before a build.
      Grade the plan as written.
    - Says nothing changed: confirm that against what the plan records
      (commit, outputs, test counts) if it records any. A claim that
      contradicts the plan's own recorded output fails the check that
      output belongs to.
    - Records changes: re-grade the diagnosis, scope and test checks
      against the plan *plus* its deviations. Each deviation must say
      what changed, why, and the output that showed it. A deviation
      with no reason or no output is unverifiable, so grade the
      affected check `unclear`.
    - If a deviation makes the posted plan comment untrue (a different
      cause, a wider scope, a dropped test or check), the comms check
      fails unless the plan names the follow-up comment that corrects
      it. Also note in the summary any promise from the posted comment
      that the build did not keep.

## Verdict assembly

16. Apply the rubric's verdict rule exactly as written:
    - **accept** only if every `required` check is graded pass.
    - **reject** if any `required` check is fail or unclear (unclear
      counts as fail).
    - `preferred` checks are reported but never change the verdict.
17. Cite the deciding check:
    - On reject: in the summary, name the first `required` check (in
      table order) that failed or was unclear, and quote its evidence
      line.
    - On accept: say that all required checks passed, and quote the
      diagnosis check's evidence line.
18. **Live mode only:** check the plan comment against each rule in
    `voice-guide.md`. Quote every rule it breaks in the summary. Voice
    notes never change the verdict.
19. Report gaps in the summary, including any rubric labels you had to
    translate (step 5) or apply by intent (step 8), and anything this
    procedure did not cover.
20. Output the short summary first, one line per check plus any notes,
    then the fenced JSON block in SKILL.md's format. Use the check
    names exactly as they appear in the rubric. Write nothing after
    the JSON block.
