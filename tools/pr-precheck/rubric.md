# Rubric: is this pull request ready to submit?

<!--
THIS IS THE PART YOU WRITE (fourth week running; this is the rubric's
final form in the sandbox). Your frame in SKILL.md executes whatever
checks you define here, via your procedure.md. It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the diff read against the plan's scope, the test
     evidence read against the plan's test plan, the description read
     against the diff, the repo-facts block's template asks) or a
     location from your references/evidence-guide.md. "The PR" is not
     a source; "the diff's changed files read against the plan's
     stated boundary" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself
     (does the diff fall inside the plan plus its deviation notes? is
     the claimed evidence observable?), never the write-up's shape
     (how long the description is, how many commits there are).
     Structure-shaped checks are what make graders disagree with
     themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (submit) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. State the `unclear`
   treatment explicitly: the frame here is YOUR SKILL.md, so a rubric
   that stays silent is only covered if your frame's grading
   discipline says what happens (the contract's own default is that
   an unverifiable claim fails).

Cover what actually gets bad PRs submitted. The failure families the
lecture named ARE the harness's scoring categories, same names as the
eval README: silent drift (the diff silently does more or less than
the posted plan, or the description claims fidelity the diff
contradicts), not tested (the evidence proves nothing observable, or
the repo's own checks were never run), unreviewable (debris or
unrelated hunks bury the change), and standards wall (the repo's
stated template and disclosure asks are ignored). Your evidence
guide's four headings map onto these one to one (plan fidelity =
silent drift, test evidence = not tested, diff quality =
unreviewable, standards and comms = standards wall), and the category
floor is scored on exactly these names plus clear accept. A rubric
that ignores a category will fail the eval packages built around
that category. And remember the honest-outcome
rule, fourth week running: a PR that honestly discloses a shortfall
can be ready; a rubric that equates "less than everything" with
"hold" fails the set.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Diff matches the plan | List every file and hunk in the **Diff** (live: `git diff main...HEAD`). Read each against the plan's scope, its named files, its "not in scope" boundary, and its deviation notes in **Plan context** (live: `plan.md`). | Passes only if (a) every changed hunk implements a step the plan names or a change a deviation note discloses, and (b) the plan's core change is actually present in the diff. Fails if any hunk touches a file or behavior the plan excludes or never names with no deviation note, or if the plan's fix is missing or replaced by a different change (for example, a dependency bump in place of the planned code change). A mismatch that a deviation note in the plan or the PR description openly states and justifies does not fail. Tests, fixtures, and changelog entries the plan or repo asks for count as in scope. | required |
| Description matches the diff | Read every factual claim in **Description** (and **Title**) against the **Diff** and **Test evidence**: what was changed, which files, what is "not changed", what was tested. | Passes if every claim about the change is true of the diff and evidence. Fails if the description claims something the diff contradicts (for example, "no changes beyond the plan" over a hunk the plan never names, "tests added" with no test file in the diff, or a fix described that the diff does not contain). Omissions alone do not fail this check; false or contradicted claims do. | required |
| Reported failure re-tested | Read **Test evidence** against the plan's **Test plan** and the repro evidence in **Plan context**. | Passes only if the evidence shows the reported failure re-run on the branch with an observable result (command plus before/after output, a failing-then-passing test named, or equivalent) that matches the plan's expected behavior, AND every failure mode or repro the plan's test plan names is either shown or explicitly disclosed as not run with a reason. Fails if the evidence is missing, is only "tests pass" or "works for me" with nothing named, exercises a path the diff did not change, admits the fix was not tried on the failing case, or silently skips a failure mode the test plan named. | required |
| Repo checks run | Read **Test evidence** for the test suite, linter, or build the plan's test plan names or the repo's facts and template ask for (live: `CONTRIBUTING.md`, the PR template, captured command output). | Passes if the evidence names the suite or command run on the branch and its result (for example, "`cargo test` passes (614 tests)", "`yarn test tests/format/js/if` passes"), covering what the plan's test plan lists. A check the student could not run passes only if the PR says so and why. Fails if no suite is named, only "all tests pass" is claimed with nothing identified, or the plan's named suite is absent without disclosure. If neither the plan nor the repo names any suite or check, this check passes. | required |
| Diff is reviewable | Read each hunk of the **Diff** and each line of **Commits** for content not needed by the change. | Passes if every hunk contributes to the planned change or its tests, changelog, or docs. Fails if the diff contains any of: debug prints or logging left in, commented-out or dead code, formatting or whitespace churn in lines the change does not otherwise touch, reordered or rewritten code unrelated to the fix, unrelated dependency or lockfile changes, or stray files (editor, build, or local config). One such hunk is enough to fail. | required |
| Repo template and asks met | Read **Repo facts** (PR template sections and checklist, contribution policy, required artifacts such as changelog or whatsnew entries, issue-reference format) against **Title**, **Description**, and **Diff** (live: the repo's PR template and `CONTRIBUTING.md`, plus `scope.md` house rules). | Passes if every section or checklist item the repo's template or policy asks for is present in the description with real content (a reason counts for "n/a"), any artifact the repo requires (changelog entry, whatsnew note, test) is present in the diff or its absence is explained, and the issue is referenced in the form the repo asks. Fails if a required section or checklist is visibly absent or left as template placeholder, or a required artifact is missing without explanation. If the repo states no template and no asks, this check passes. | required |
| AI-use disclosure met | Read the contribution policy in **Repo facts** (live: `CONTRIBUTING.md`, the PR template, any AI policy page) against **Description** and **Commits**. | If the repo states an AI-use policy, passes only if the PR does what that policy asks (disclosure in the stated place, a required checkbox, or the stated restriction respected); fails if the policy is ignored, including a PR that mentions AI help where the policy forbids it or omits a required disclosure. If the repo states no AI policy, this check passes. | required |
| Description orients a reviewer | Read **Title** and **Description**. | Passes if the title names the specific behavior changed and the description states what changed, why (linking the issue), and how it was verified, in terms specific to this issue. Fails if the title or description is generic ("fixed the bug", "updates") or does not say how it was verified. | preferred |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->

Accept (submit) only if every `required` check grades `pass`. Any
required check graded `fail` or `unclear` makes the verdict reject
(hold): `unclear` counts as fail, because a claim the package does not
let you verify is not ready to submit. Preferred checks are graded and
reported but never change the verdict.

An honest shortfall is not a failure: a deviation note, an openly
disclosed check that could not be run, or a stated limitation passes
the check it touches whenever that check's pass condition allows
disclosure. Grade what the PR discloses, not whether it did
everything.
