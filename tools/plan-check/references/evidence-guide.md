# Evidence guide: where evidence lives in a plan package

<!--
The skill's map: for every kind of evidence a rubric check names, where
to find it in a plan package and what good looks like there. Drafted
against eval/packages/calib-01.md (jesseduffield/lazygit#5900); the
examples below quote it, but the locations apply to every package.

Every eval package has the same six blocks, in this order:
  ## Repo facts      - repo rules: bug template, CONTRIBUTING policy, AI disclosure
  ## Issue           - the report as filed: behavior, expected, version
  ## Thread highlights - maintainer/commenter signals (may be "(no comments)")
  ## Repro evidence  - environment, numbered steps, Expected/Actual
  ## Candidate plan  - usually Cause / Change (In/Out) / Test, sometimes
                       other labels or plain prose
  ## Candidate plan comment - the words that would be posted on the issue

Live mode: Issue + Thread highlights = the GitHub issue page and its
comments; Repro evidence = the student's own posted repro comment (or
the house repro pack as quoted in the drafts); Repo facts =
CONTRIBUTING.md, issue/PR templates, README; Candidate plan = plan.md;
Candidate plan comment = the draft comment.

Find each piece by what it says, not by its label: if a plan has no
"Cause:" line, its cause is whatever sentence claims why the bug
happens.
-->

## Diagnosis and grounding

**Where it lives.**
- The cause: the plan's cause statement (`Cause:` in calib-01), plus any
  causal claim in the plan comment ("the post-push refresh scope just
  misses the branch-commits context").
- What the cause must explain: `## Repro evidence`, especially the
  numbered steps and the Expected/Actual lines. Also look for control
  steps that rule things out. In calib-01, step 2 confirms the push
  really succeeded (`git log origin/main -1`), and step 4 shows that
  re-entering the view fixes the color.
- `## Issue` for the reporter's version of the behavior. Repro
  evidence wins if the two differ.

**What good looks like.**
- The cause accounts for every observed step, including the controls,
  and contradicts none of them. In calib-01, "the view's model is not
  refreshed after push, and is rebuilt on re-entry" explains step 3
  (stale color) and step 4 (fixed on re-entry), and it fits step 2
  (the push itself worked).
- The cause names a mechanism rather than restating the symptom. "The
  color is wrong after push" is a symptom. "The commits context is not
  in the post-push refresh scope" is a mechanism.
- If the cause has not been confirmed in code (for example, nobody has
  read the callback yet), the plan says so. A cause that contradicts a
  repro step fails, however confident it sounds.

## Scope

**Where it lives.**
- The plan's change statement and its In/Out lines (`Change:` …
  `In:` … `Out:` in calib-01).
- The named files, functions or areas
  (`pkg/gui/controllers/sync_controller.go`, "the push completion
  callback").
- Indirect scope signals: the Test line ("both share the callback")
  and the comment ("one-change fix", "keeping it minimal").

**What good looks like.**
- One bounded change at a named location, with an explicit statement
  of what will not be touched. calib-01's Out line excludes "how push
  status is computed" and "other views' refresh behavior".
- The size fits the bug: a stale-view bug gets a refresh fix, not a
  rework of the refresh system.
- A drive-by rewrite looks like this: several subsystems, verbs such as
  "refactor", "clean up" or "rework", no Out line, or an In line that
  names a whole package instead of a location.
- Side effects of shared code are acknowledged. calib-01 notes the
  callback is shared with the main commits panel and force push.

**Repo facts interaction.** When `## Repo facts` signals limited review
capacity (calib-01: the maintainer reviews outside PRs "only
selectively"), a tighter scope is more important and a broad one is a
stronger reason to hold.

## Executability

**Where it lives.**
- The plan's change statement: the file, the function or callback, and
  the mechanism of the edit ("adds the commits context to its post-push
  refresh scope").
- Ordering or dependencies, if the plan has any.
- The cause, which every edit should trace back to.

**What good looks like.**
- A stranger could open the repo and start working without asking the
  author anything. They know which file to open, which code path to
  find, what to add or change, and what behavior should result.
  calib-01 names the file, the callback, and the exact edit (add one
  context to a refresh scope).
- Each edit connects to the diagnosed cause. If an edit fixes
  something the cause never mentioned, or the cause names something no
  edit touches, there is a gap.
- Fails: "fix the refresh logic", "update the controller", or a file
  named with no edit described. The executor would have to rediscover
  the diagnosis.

## Test plan

**Where it lives.**
- The plan's test statement (`Test:` in calib-01).
- `## Repro evidence` numbered steps: the test plan should be read
  step by step against these.
- `## Repo facts` for the repo's testing conventions, if stated
  (calib-01 states none, so manual checks are not penalised).

**What good looks like.**
- The test re-runs the failing reproduction and names the observable
  pass condition at the step that failed. calib-01: "at step 3 the
  color must flip without leaving the view".
- It covers the paths the change touches beyond the original repro,
  especially shared code. calib-01 checks the main commits panel and
  force push because "both share the callback".
- Another person could run it and get a yes/no answer.
- Fails: "verify it works", tests that only run code without checking
  the reported behavior, or tests that skip the failing step and check
  something adjacent.

## Honesty

**Where it lives.**
- How the cause is worded in both the plan and the comment. Compare the
  wording against what the evidence actually establishes.
- Any risks, unknowns or assumptions section, or the lack of one.
- Promises in the comment about timing or outcomes ("Will send the PR
  shortly").
- Live mode, after a build: deviation notes added to `plan.md`
  recording what changed from the posted plan and why.

**What good looks like.**
- Confidence matches the evidence. Facts the repro observed are stated
  plainly. Inferences about code that was not read are marked as
  inferences ("likely", "I expect", "to confirm when I open the
  callback").
- Known unknowns are named, for example "not yet checked whether other
  push entry points use this callback."
- Watch for: the comment states the cause more firmly than the plan
  does or the evidence supports. In calib-01, the comment says "just
  misses" as settled fact, while the repro only shows the symptom and
  never inspects the code.
- Watch for: promises about outcomes or timing ("PR shortly") that the
  author cannot guarantee.
- A deviation recorded in the plan is honest. A deviation that appears
  only in the diff is not.

## Comms

**Where it lives.**
- `## Candidate plan comment`: the words that would be posted.
- `## Thread highlights`: maintainer requests, rejected approaches,
  duplicate or claim signals. "(no comments)" means there are no
  constraints from the thread (calib-01).
- `## Repo facts`: the bug template, the CONTRIBUTING policy, and any
  AI-use disclosure requirement. calib-01: selective review of outside
  PRs, no AI disclosure requirement.

**What good looks like.**
- The comment matches the plan: same cause, same scope, same tests, and
  no stronger claims than the plan makes.
- It responds to what the thread and repo actually say. If a maintainer
  asked for something or rejected an approach, the comment addresses
  that. If the repo requires AI disclosure, the comment includes it.
  calib-01's comment cites the review-bandwidth note in CONTRIBUTING
  and keeps the change minimal accordingly.
- It could only have been written for this issue: it names the version,
  the behavior, and the location.
- Fails: boilerplate that could go on any issue, ignoring a
  maintainer's stated preference, presenting a disputed cause as
  settled, or skipping a required disclosure.
