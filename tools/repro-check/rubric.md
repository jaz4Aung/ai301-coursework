# Rubric: is this reproduction package ready to post?

Author: Aung Aung (section 4). Revised after the rubric swap, where
Ethan graded `calib-03` with the first draft of this rubric.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `environment-recorded` | The repro report's environment record, read against the versions, platform, and configuration the issue names in its body and repo-facts block. | Pass if the report names the operating system, the project version or commit, and any dependency or configuration the issue's own text treats as relevant, in enough specificity that a reader could stand the same conditions up. A difference from the issue's stated target does not fail this check when the report calls the difference out. Fail if a value the issue turns on is absent or given only as a vague range ("latest", "recent version"). | required |
| `steps-followable` | The repro report's reproduction steps, read as a stranger starting from a clean checkout. | Pass if a reader can get from the stated starting state to the trigger without inventing anything: every command, input file, and value needed is either given or obtainable from the repo. Judge only whether the path is walkable, not how it is formatted, numbered, or how many steps it takes. Fail if any step requires a value, file, or setting the package never supplies, and fail — never grade `unclear` — if the report gives no steps at all: an absent path is an unwalkable one. | required |
| `behavior-matches-issue` | The artifacts in the repro report (output excerpt, log, stack trace, screenshot) read line by line against the specific failure the issue describes, together with the report's own stated outcome. | Read the stated outcome first, because it decides which question this check asks. **If the report claims the behavior occurred:** pass if the artifact shows the same failure mode the issue reports — the same error, the same wrong value, the same symptom — not merely a failure in the same area. Fail if the artifact shows an adjacent or upstream problem (a syntax error, an argument-validation error, a config error, a different exception) while the report presents it as the issue's bug. **If the report states the behavior did NOT occur** — an honest cannot-reproduce — then the artifact is not expected to show the bug, and demanding that it does would contradict `outcome-stated-honestly`. Ask instead whether the attempt was aimed at the right thing: pass if the steps and artifacts show a faithful attempt at the specific mechanism the issue describes — the right feature, the right trigger conditions — and the artifact shows what that attempt produced instead. Fail if the attempt tested something other than what the issue describes, or skipped the trigger, since a negative result about the wrong scenario says nothing about the issue. Either way, fail — never grade `unclear` — if the report shows no artifact at all. This check fails even when the steps are followable, the environment is complete, and the report is long and well formatted. | required |
| `evidence-present` | The repro report's artifacts, and the relationship between each assertion it makes and the artifact backing it. | Pass if the report's central claim about what happened is backed by something observed and shown — pasted output, a log excerpt, an image, a test result. Fail if the outcome is only asserted in prose ("I ran it and it crashed", "confirmed on my machine") with nothing shown. | required |
| `outcome-stated-honestly` | The repro report's conclusion, read against the artifacts it actually shows. | Pass if the stated outcome is no stronger than the evidence supports. A clear "I could not reproduce this", with the environment and steps recorded and the observed non-failure shown, passes: that is a real result. Fail if the report asserts a confirmed reproduction the artifacts do not show, or hedges away a result the artifacts do show. | required |
| `repo-conventions-met` | The repo's stated **contribution policy** as recorded in the repo-facts block, read for one question above all: does its AI rule reach *issue comments*? Read that against the claim comment and the repro report as written. Deliberately NOT the bug-report template's content asks — those are graded by `environment-recorded` and `evidence-present`, and reading them here would score one gap twice. | Treat the candidate comments as AI-assisted work. The question is never whether assistance was used — assume it was — but whether the repo required that to be said out loud here. **Fail** if the policy requires disclosing AI assistance in issue comments — either by naming issues or comments, or by a blanket ask covering "all AI usage in any form" — and neither comment contains an explicit disclosure. Silence never satisfies a disclosure requirement; only an affirmative statement does, so absence of proof that AI was used is not a defence. **Pass** if the repo states no AI policy at all, or if its disclosure ask is scoped to pull requests or code contributions rather than issue comments (a policy asking contributors to state the tool "in the pull request", or silent on issue comments, imposes nothing on a comment). A policy that asks only for human understanding, human voice, or responsibility for AI output is not a disclosure requirement: it passes unless the comments plainly break it. | required |
| `claim-names-specifics` | The claim comment, read against the issue's title and body. | Pass if the claim comment identifies this issue by its actual content — the specific symptom, component, or behavior at stake — and says what the author will do next. Naming an intended line of investigation or a candidate approach counts as saying what comes next and passes ("plan: find where the prompt decides to appear and make the untracked case warn"): that is a direction, not a debt. Fail on either of two things: the comment is interchangeable boilerplate ("I'd like to work on this", "can I take this?") that would read identically on any other issue; or it commits to a deliverable or a time — a promised fix, a promised PR, or any date or soon-ness ("shortly", "by the weekend") — which is a promise the author cannot yet keep. The line is between describing where you will look and owing the thread a result. | required |
| `thread-courtesy` | The claim comment and repro report read against the issue thread's existing comments in the package's thread highlights. | Pass if the comments neither restate what the thread already established nor lean on another commenter's proof in place of their own. Fail if a comment piggybacks ("same as above, can confirm") instead of showing its own work. Never changes the verdict. | preferred |

## Verdict rule

The verdict is **accept** if and only if every `required` check grades
`pass`. Any single `required` check grading `fail` produces
**reject** — required checks do not trade off against one another, and
a package strong everywhere else is still held by one failed required
check.

`preferred` checks are reported with their grade and evidence but
never change the verdict.

`unclear` on a required check counts as **fail**. Proof that cannot be
located in the package is proof that is not ready to post; the burden
sits with the package, not with the grader.

One exception, which `SKILL.md` governs: in live mode on a claim-only
draft, the checks whose evidence is the repro report
(`environment-recorded`, `steps-followable`, `behavior-matches-issue`,
`evidence-present`, `outcome-stated-honestly`) grade `unclear` with
evidence `not yet applicable: claim-only draft` and are left out of
the verdict rule entirely. The verdict then rests on
`claim-names-specifics` and `repo-conventions-met` alone, and answers
only: is this claim comment ready to post?
