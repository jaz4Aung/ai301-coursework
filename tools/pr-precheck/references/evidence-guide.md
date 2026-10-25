# Evidence guide: where evidence lives in a PR package

<!--
THIS IS THE PART YOU WRITE (third week running: the map stays in your
hands). Your tool uses this guide as its map: for every kind of
evidence a rubric check names, this file says WHERE to find it in a PR
package and WHAT GOOD LOOKS LIKE when you do.

The four families below are the harness's failure categories under
the names the eval README uses: plan fidelity = silent-drift, test
evidence = not-tested, diff quality = unreviewable, standards and
comms = standards-wall. A package that fails none of them is a
clear-accept. Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the plan-context block's scope pair and test
  plan, the candidate PR's diff, commits, description, or
  test-evidence section, the repo-facts block's template asks and
  stated policy). In live mode (where in your working copy and on
  GitHub: your plan.md and its deviation notes, your branch's diff,
  your draft title and description, your captured test output, the
  repo's PR template and CONTRIBUTING.md).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("every changed file falls inside the
  plan's stated boundary or a deviation note") over adjectives ("the
  diff is clean").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts three ways: your
procedure says WHEN to gather each family, this guide says WHERE, and
your SKILL.md says the tool reads both. Write the map you wish your
executor had.
-->

## Plan fidelity (harness category: silent-drift)

<!-- Where the plan states its scope, boundary, and deviation notes,
and where the diff shows what actually changed. What it means for a
diff to match the plan, for an honest deviation to re-tie a mismatch,
and what silent drift looks like in each direction (more than the
plan, or less with no note). The description's fidelity claims read
against the diff live here too: a description claiming more or less
than the diff delivers is silent drift, not a comms problem. -->

**Where it lives.**
- Eval: the **Plan context** block (its "Scope", "Files", "Not in
  scope", and any deviation note) against the **Diff** file headers
  (`--- a/` / `+++ b/`) and hunks, and the **Commits** list. Fidelity
  claims live in **Title** and **Description** ("implements the plan
  exactly", "no changes beyond", "tests added").
- Live: `plan.md` (scope, named files, out-of-scope line, deviation
  notes) against `git diff main...HEAD` and `git log main..HEAD
  --oneline`; claims in the draft title and description.

**What good looks like.** Every changed file and hunk maps to a step
the plan names, a test, fixture, or changelog the plan or repo asks
for, or a deviation note; and the plan's core change is visibly
present. Every fidelity claim in the description is true of the diff.
An honest deviation (stated in the plan's notes or the description,
with a reason) re-ties a mismatch.

**What silent drift looks like.**
- More than the plan: a hunk in a file the plan never names or
  excludes, with no note (for example, a second hunk rewriting an
  unrelated module).
- Less than the plan: the planned fix is missing, or replaced by a
  different change (a dependency bump instead of the named code edit).
- A description claiming fidelity the diff contradicts ("no changes
  beyond the plan" over an extra hunk).

## Test evidence (harness category: not-tested)

<!-- Where the PR shows its proof: the test-evidence section's
before/after against the plan's test plan and the reproduction's own
steps, and the outcome of the repo's own checks or suite. What
decisive looks like (an observable behavior named, the expected-after
stated, the checks' outcome visible) next to "tests pass". -->

**Where it lives.**
- Eval: the **Test evidence** section, read against the plan's "Test
  plan" and "Repro evidence" in **Plan context**; test files in the
  **Diff**.
- Live: the captured terminal output the student provides, read
  against `plan.md`'s test plan; test files in the branch diff; the
  suite and checks `CONTRIBUTING.md` or the PR template names.

**What good looks like.** The reported failure re-run on the branch,
with the command and its observed output, and the before and after
both visible or the before cited from the repro. Every repro, failure
mode, and control the test plan names is shown, or disclosed as not
run with a reason. The repo's suite (or the one the plan names) is
named with its result ("`cargo test` passes (614 tests)").

**What not tested looks like.** "(none provided)"; "tests pass" or
"CI green" with nothing named; evidence that runs a path the diff does
not change; "did not get a chance to test"; a second failure mode the
plan named left out without a word.

## Diff quality (harness category: unreviewable)

<!-- Where the change itself lives: the unified diff and the commit
list. What a reviewable change looks like (the fix visible, nothing
unrelated riding along) and the debris tells: debug leftovers, dead
code, commented-out blocks, formatting churn, drive-by edits. -->

**Where it lives.**
- Eval: every hunk in **Diff** and every message in **Commits**.
- Live: `git diff main...HEAD` and `git log main..HEAD --oneline`;
  `git status` for stray untracked files the student plans to add.

**What good looks like.** The fix is visible in a small set of hunks,
and every hunk is the fix, its tests, its changelog, or its docs.
Nothing unrelated rides along.

**Debris tells.**
- Debug leftovers: `print`, `console.log`, `dbg!`, temporary logging.
- Dead code: commented-out blocks, unused functions or variables.
- Formatting churn: reflowed or reordered lines the fix does not
  otherwise touch ("gofmt", "cleaned up formatting" commits).
- Drive-by edits: unrelated refactors, renames, dependency or lockfile
  bumps the plan never calls for, editor or build files.

## Standards and comms (harness category: standards-wall)

<!-- Where the repo states its asks (the repo-facts block's PR
template sections, contributing instructions, and stated policy,
including AI-use disclosure) and where the PR honors them: the
stated sections filled with real content, the disclosure present,
explicit maintainer direction in the thread engaged. What compliant
looks like next to boilerplate or a visibly ignored ask. (Whether
the description's claims match the diff is plan fidelity, above.) -->

**Where it lives.**
- Eval: the **Repo facts** block's "pull requests" line (template
  sections and checklist, required artifacts such as changelog or
  whatsnew entries, issue-reference format) and "contribution policy"
  line (including any AI-use policy); **Thread highlights** for
  explicit maintainer direction. The PR side is **Title**,
  **Description**, **Commits**, and the **Diff** (for required files).
- Live: the scoped repo's `.github/PULL_REQUEST_TEMPLATE.md` (or
  equivalent), `CONTRIBUTING.md`, any AI policy, the issue thread, and
  `scope.md`'s house rules; against the draft title and description
  and the branch diff.

**What compliant looks like.** Each template section present with real
content specific to this PR (or "n/a" with a reason); each checklist
item addressed; each required artifact in the diff; the issue
referenced in the asked form ("closes #NNNN"); any required AI
disclosure present where the policy says. A repo that states no
template, no asks, and no AI policy imposes none.

**What a standards wall looks like.** A required checklist or section
missing or left as placeholder text; a changelog or whatsnew the repo
requires absent from the diff; no issue reference where one is asked;
AI help undisclosed where the policy requires disclosure, or used where
it is forbidden; a maintainer's explicit direction in the thread
ignored.
