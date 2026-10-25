# Procedure: how this tool grades a PR package

<!--
THIS IS THE PART YOU WRITE (second week running for the procedure).
Week 3 you wrote these steps for a plan package; this week the graded
object is a PR package, and the read that matters most is a
side-by-side: the diff against the plan, the evidence against the test
plan, the description against both. Your week-3 procedure is the
pattern; do not paste it unchanged, because its read order was built
for a different object.

Your rotation is the design brief again, and this week friction routes
three ways: a stall on WHAT to decide is a rubric gap, a stall on
WHERE to look is a procedure gap (this file), and a stall on what the
tool even reads or outputs is a frame gap (your SKILL.md). A complete
procedure lets someone who has never seen a PR package before grade
one exactly the way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
plan's scope pair before opening the diff, and list the files the plan
names" is a step; "understand the change" is a wish.
-->

## Read order

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides where the plan sits in the order (before the diff? before the
description?) and says why the order matters for the side-by-side
checks that come later. -->

Read in this order. Do not grade anything until all five steps are
done.

1. **Repo facts** (live: the PR template, `CONTRIBUTING.md`, and the
   `scope.md` house rules). Note: every template section and checklist
   item, every required artifact (changelog, whatsnew, tests), the
   issue-reference format, and any AI-use policy, word for word. Write
   "none stated" for any of these the repo does not state.
2. **Issue and thread highlights.** Note the reported failure in one
   line and every failure mode or repro the issue names.
3. **Plan context** (live: `plan.md`). Note, as separate lists: the
   files the plan names; the steps of its change; its "not in scope"
   boundary; its deviation notes; every item in its test plan
   (each repro, each control, each named suite or command) with the
   expected result.
4. **Diff and commits.** List every changed file and, under each, every
   hunk in one line ("removes line X", "bumps tcell", "reformats
   assignments"). Note each commit message.
5. **Title, description, and test evidence, last.** Note every factual
   claim in the title and description, and every command and output in
   the test evidence.

Read the plan before the diff so each hunk is read against a boundary
you have already written down, and read the description last so its
claims cannot shape how you read the diff. A confident description
read first is how drift gets missed.

## Evidence gathering

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which file, diff,
or page per your evidence guide) to pull the fact from, and what to
record. The load-bearing gathers this week are pairings: diff files
against plan scope, claimed evidence against the plan's test plan and
the repo's checks, description claims against diff contents. A
complete procedure leaves no check whose evidence an executor would
have to hunt for. -->

Build these pairings from your notes, one per evidence family. Record
each line exactly; each check's evidence line is taken from them.

- **Plan fidelity (diff vs. plan).** For each hunk in your step-4 list,
  write which plan step or deviation note it implements, or "none".
  Then, for each step of the plan's change, write which hunk
  implements it, or "missing". A hunk is in scope if it is a test,
  fixture, changelog, or docs entry the plan or repo asks for.
- **Description vs. diff.** For each claim from step 5, write the hunk
  or evidence line that confirms it, or the hunk that contradicts it,
  or "unverifiable".
- **Test evidence vs. test plan.** For each test-plan item from step 3,
  write the evidence line that shows it run on the branch with its
  observed result, or "disclosed as not run: <reason>", or "absent".
  Separately, write which suite or repo check the evidence names and
  its result, or "none named". Check that the re-run exercises the
  changed code path, not an untouched one.
- **Diff quality.** For each hunk, mark it "needed" (pairs with a plan
  step, test, changelog, or docs) or name the debris type: debug print,
  dead or commented-out code, formatting churn, unrelated rewrite,
  unrelated dependency or lockfile change, stray file.
- **Standards.** For each template section, checklist item, required
  artifact, issue-reference ask, and AI policy from step 1, write where
  the description or diff meets it (quote it), or "absent".

In eval mode every fact comes from the bundle. In live mode the diff
comes from `git diff main...HEAD`, commits from `git log main..HEAD
--oneline`, and repo facts from the scoped repo's files.

## Check execution

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->

1. Run the checks in the rubric's table order, one at a time.
2. Grade each check by applying its pass condition to the pairing for
   its family. Do not re-read the whole package: the pairing lines are
   the evidence. Re-open the package only to confirm a quote you are
   about to cite.
3. Grade `pass` when the pairing satisfies the condition, `fail` when it
   breaks it, and `unclear` only when the evidence the condition needs
   is genuinely absent from the package (for example, a diff that is
   empty or truncated so hunks cannot be listed). "absent" in a pairing
   is not `unclear`: absence the condition names (no test evidence, a
   missing template section) is a `fail`.
4. Where a condition allows disclosure, a "disclosed as not run" or a
   deviation note satisfies it; do not fail an honest shortfall.
5. Write one evidence line per check: the single fact or quote that
   decided it. For a fail, quote the offending hunk, claim, or missing
   ask; for a pass, name the line that satisfied the condition.
6. Grade each check independently. A failure in one check does not
   change another check's grade, and grade every check even after the
   verdict is already decided.

## Verdict assembly

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check: the
check whose failure the verdict turned on. When more than one check
failed, your procedure picks which one gets quoted (first failing
required check in rubric order is a fine rule); nothing picks it for
you, so write the rule down. A complete procedure produces the same
verdict from the same grades, every time. -->

1. Apply the rubric's verdict rule: accept only if every `required`
   check is `pass`; any required `fail` or `unclear` makes it reject.
   Preferred checks never change the verdict.
2. On reject, the deciding check is the first required check, in rubric
   table order, that is `fail` or `unclear`. Name it in the summary
   and quote its evidence line there. On accept, state that all
   required checks passed.
3. Write the output as SKILL.md's "Verdict and output" lays it out: the
   per-check summary in rubric order, then (live mode) the Voice
   section, then any procedure gaps, then the JSON block last, with
   every rubric check in it in table order.
