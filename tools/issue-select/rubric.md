# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| not-archived | The `archived:` flag on the `repo:` line of the repo-facts block. Live: the archived banner across the top of the repo front page. | archived:no | required |
| commits-recent | The dates in `last 5 default-branch commits`, measured against the bundle's `captured:` date (live: against today). | newest of the last 5 default-branch commits within 90 days of the capture date   | required |
| maintainer-responds | The five latency figures under `maintainer first-response sample`, including the entries that read `no maintainer comment in thread`. | at least one sampled issue has a maintainer response, at any latency | preferred |
| repo-in-use | The `latest release` line and the star count on the `repo:` line. | a release within ~12 months, or none published while commits-recent passes | preferred |
| scope-bounded | The issue body and the comment thread: whether the issue declares itself an umbrella, tracking, meta or "mega" issue, whether the design is still being debated with no maintainer decision, whether a maintainer says the fix touches core internals, and whether the `linked PRs:` list shows closed-unmerged attempts. | fails if the issue declares itself a tracking, umbrella, meta or "mega" issue, or its items are explicitly meant to be split across separate PRs or contributors; or the thread shows design still being debated with no maintainer decision; or a maintainer says the fix touches core internals; or it's a usage question; or it's a feature wish with no spec and an unmade product decision. A multi-step change list within one coherent deliverable is a spec, not an umbrella, and passes. A terse body, a missing repro, or no label is not a failure. | required |
| unclaimed | The `this issue:` line — `assignees:` and each entry in `linked PRs:` with its state (`open` / `closed` / `merged`) — plus claim comments in the Comments section and their dates. | fails if assignees: is non-empty, or any entry in linked PRs: is open, or an unanswered claim comment is less than ~12 months old. closed and merged PRs don't block | required |
| policy-allows-ai | The `contribution policy` line in the repo-facts block (present in all 20 bundles). Live: `CONTRIBUTING.md` in the root or `.github/`, plus any contributor docs it links to. | fails only on an outright ban on AI-generated contributions (including "PRs suspected of AI use are closed"). Disclosure, testing and human-review conditions pass. Silence passes  | required |

## Verdict rule

Accept if every required check passes; any required fail rejects; preferred checks never change the verdict and only rank accepted issues; unclear counts as fail on required checks and is ignored on preferred ones.
