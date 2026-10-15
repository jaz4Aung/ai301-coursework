# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Diagnosis matches evidence | Read the plan's stated cause in **Diagnosis** and **Candidate plan comment** against every step of **Repro evidence**, especially its control steps: any step where a condition was varied (an input, a flag, a setting, a format) and the behavior did or did not change. | Passes only if the proposed cause explains the observed behavior across the reproduction, is consistent with every control step, and distinguishes confirmed evidence from an unverified hypothesis. A cause fails if a control step contradicts it and the plan neither explains that step nor acknowledges it: for example, blaming a component the repro shows can be switched off without changing the failure. A control step the plan explicitly accounts for, with a mechanism consistent with the rest of the evidence, does not fail the check. | required |
| Scope is bounded | Read **Scope** and **Changes** in the candidate plan. | Passes if the plan identifies the file or files to change, describes the intended edits, preserves unrelated behavior, and explicitly excludes unrelated subsystems. The proposed work must be a contained fix rather than an open-ended redesign. | required |
| Implementation is actionable | Read **Diagnosis**, **Changes**, and **Test plan**. | Passes if a stranger can begin implementation without guessing the relevant component, code location, mechanism of change, or expected behavior. The plan must connect each proposed edit to the diagnosed cause. The code location may be a named file or component plus a concrete, already-working way to pin the exact function (for example, a named debug log or trace that shows it); "will figure out where" with no method fails. | required |
| Tests validate the actual bug and fix | Read the plan's **Test plan** against every step in **Repro evidence**, including the failing step and each control step. | Passes if the tests re-run the reported failure, exercise the proposed fix, and verify an observable improvement at the step that failed. Fails if the tests only check something adjacent to the failure: a smoke test, a test that merely runs the changed code, or a check of a component the repro shows is not where the failure happens. | required |
| Expected behavior is verifiable | Read **Repro evidence**, the plan's **Summary**, and **Test plan**. | Passes if the plan gives a concrete success condition that can be checked by another person, such as reaching the last line promptly while preserving search/navigation behavior, with a comparison to the failing case where appropriate. | required |
| Comment is aligned with project evidence and context | Read **Candidate plan comment** against the **Thread highlights** and **Repo facts** blocks. | Passes if the comment accurately summarizes the issue discussion, does not present a disputed or unsupported cause as settled fact, and stays consistent with the repository's stated contribution/testing context. | preferred |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->
Accept (ready) only if every `required` check passes. Preferred checks never change the verdict. Treat `unclear` (`?`) as a failure for the check; therefore, any required `F` or `?` results in **hold**.