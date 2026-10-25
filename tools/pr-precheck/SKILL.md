---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

<!--
THIS IS THE PART YOU WRITE, and it is the last one: the frame itself.
Weeks 1 through 3 handed you a working SKILL.md and you filled the
files behind it; this week the frame ships as headings, and you write
what it says. The frontmatter above and the section headings below are
fixed (CONTRACT.md's layout rule); the instructions under each heading
are yours. Write instructions to the tool, in the imperative, the way
weeks 1-3's frames spoke to you: what to read, in what order, what to
refuse, what to emit. Your executor in the rotation is the test: a
frame gap they hit (cannot tell what the tool reads, or how a verdict
gets assembled) is a missing sentence here.

One section is not yours: the JSON schema in "Verdict and output" is
reproduced from CONTRACT.md verbatim and may not be altered. Your
words decide everything around it.
-->

## The question

<!-- State, in your words, the one question this tool answers and what
a PR package is: what artifacts it contains and what they are read
against. CONTRACT.md fixes the question; your frame has to say it so
the tool cannot wander into grading something else. -->

Answer exactly one question about exactly one PR package: **is this
ready to submit?**

A PR package is a candidate pull request: its title, its description,
its commits, its diff, and its test evidence. Read it against two
things: the plan it claims to implement (including that plan's scope,
test plan, and deviation notes) and the issue that plan belongs to
(including the repo's stated standards: PR template, disclosure asks,
contributing policy).

Grade one package per run. Do not grade the issue, do not re-grade the
plan, do not review the code for style or offer a code review, and do
not suggest fixes beyond what a check's evidence line needs to say. If
the input is not a PR package (for example, a plan alone, a repro, or
two PRs at once), say so and stop without grading. Never answer from
gut feel: every grade comes from executing the components in this
directory.

## Inputs and modes

<!-- Define both modes. Live mode: name every input (your plan.md with
its deviation notes, your branch's diff, your draft PR title and
description, your test evidence, your issue), where each comes from,
and what a house-chain student reads instead. The branch's diff is
everything the branch changes relative to the repo's default branch:
`git diff main...HEAD` (three dots), run from the working copy, is
the command that produces it; name the source that concretely. Eval
mode: state that the bundle is the whole world, nothing is fetched,
and every check runs with the full verdict rule. -->

Run in exactly one of two modes. Decide the mode before reading
anything else: if you were handed an eval package bundle (a document
headed `# Eval package: <id>`), run in eval mode; otherwise run in
live mode. Never mix the two.

### Live mode

The student's own PR, checked before it is opened. Gather these
inputs, and only these:

| Input | Where it comes from |
|---|---|
| Plan | The student's `plan.md` from week 3, including its scope, test plan, and deviation notes. |
| Diff | Everything the branch changes relative to the repo's default branch: run `git diff main...HEAD` (three dots) from the working copy. If the default branch is not `main`, substitute its name. Also run `git log main..HEAD --oneline` for the commits. |
| Draft PR title and description | The text the student intends to submit, as they provide it (pasted, or a draft file they name). |
| Test evidence | The captured output the student provides of the tests and repo checks they ran. |
| Issue | The issue the plan belongs to, on the scoped repo: its body and thread. |
| Repo standards | The scoped repo's PR template and `CONTRIBUTING.md` (or other stated policy), read from the repo. |

A house-chain student reads the house plan and the house repro pack in
place of their own plan and repro; every check grades the same things
against those.

If any input is missing (no draft description, no test evidence, the
diff is empty), do not invent it and do not stop. Record it as absent
and let the checks that need it grade as the rubric directs.

### Eval mode

The bundle is the whole world. Every fact comes from the bundle text:
its `## Repo facts`, `## Issue`, `## Thread highlights`,
`## Plan context`, and `## Candidate PR` sections (with `### Title`,
`### Description`, `### Commits`, `### Diff`, `### Test evidence`).
Fetch nothing, run no commands, and read no file outside this tool's
directory. Treat anything the bundle does not say as unknown. Eval
mode always grades a complete package: run every check and apply the
full verdict rule. Use the bundle id from its heading as `item`.

## The scope seam (live mode only)

<!-- Tell the tool when to read scope.md, what to do with the rules it
finds there, what to refuse, and what to do when the scope's repo line
is an unfilled placeholder. State that eval mode ignores scope.md
entirely. CONTRACT.md names the required behavior; your frame has to
instruct it. -->

In live mode, read `scope.md` first, before any other file or input.
Then:

1. **Check the `Repo:` line.** If it is empty, still carries a
   bracketed placeholder (anything in `<...>` or `[...]`), or is not a
   single `owner/name` repository, stop. Do not grade. Tell the student
   to fill the `Repo:` line in `scope.md` with their section's Path
   Review repo, and output nothing else. Never guess a scope.
2. **Check the PR's target.** Confirm the working copy's remote, the
   issue, and the PR's base all belong to the scoped repo (or the
   student's fork of it). If the PR targets any other repository,
   refuse: say the package is out of scope, name the repo it targets
   and the repo in scope, and do not grade.
3. **Apply the house rules.** Treat the house rules in `scope.md` as
   stated standards of this repo, read alongside its PR template and
   policy: the PR comes from a `fix/<issue-number>-<slug>` branch on the
   student's fork, it delivers one plan for one issue, and every
   template section has real content. Grade them only through the
   rubric checks that read repo standards; do not add checks for them.
   A classmate's PR on the same issue is never a reason to fail.

In eval mode, ignore `scope.md` entirely: do not read it, and do not
apply its repo line or its house rules. The bundle's `## Repo facts`
are the only standards.

## The voice seam (live mode only)

<!-- Tell the tool when to read voice-guide.md, which outgoing text it
gates (the PR title and description), how to report a broken rule, and
why it never changes the verdict on its own. State that eval mode
ignores it entirely. -->

In live mode, read `voice-guide.md` after the scope check passes and
before grading. It is the student's own rules for how they write
upstream, carried forward from week 2.

Hold exactly two pieces of outgoing text against it: the draft PR
**title** and the draft PR **description**. Do not apply it to the
diff, the commits, the code comments, or the test output. The guide
was written for issue comments, so apply each rule to what it governs
in a PR: for example, "no dates" also covers promised follow-ups, and
"an attempt is not a guarantee" covers claiming the PR fixes more than
its evidence shows.

Report every broken rule in a **Voice** section of the summary, placed
before the JSON block. For each one, name the rule by its heading,
quote the offending words from the title or description, and give the
guide's "Right:" direction in one line. If no rule is broken, write
"Voice: no rules broken." If `voice-guide.md` is missing or has no
rules, say so in that section and continue.

The voice guide never changes the verdict on its own. Do not add a
voice entry to the `checks` array, and do not turn a voice break into
a fail, unless a check in `rubric.md` explicitly reads the voice guide;
only then does it count, and only through that check.

In eval mode, ignore `voice-guide.md` entirely: do not read it and do
not write a Voice section. Communication quality in eval mode is graded
only by the rubric's own checks.

## Component reads

<!-- Tell the tool how the pieces connect: rubric.md defines the
checks and the verdict rule, references/evidence-guide.md maps where
each evidence family lives, procedure.md is executed as written. Say
what the tool does when the procedure is silent on a step (report the
gap, never improvise around it) and what it does when rubric.md or
procedure.md has no content (the refusal rule, stated as an
instruction). -->

Read the three components in this order before grading, and give each
one the job below and no other:

1. **`rubric.md` decides what.** Its checks table is the full and only
   list of checks. Grade every row, using its name as the check `name`
   in the output and its pass condition as the only rule for that
   check. Its verdict rule is the only way grades become a verdict.
   Do not add, drop, merge, or reword checks.
2. **`references/evidence-guide.md` decides where.** For each evidence
   family a check names (plan fidelity, test evidence, diff quality,
   standards and comms), use the guide's "where it lives" for the
   current mode to find the evidence, and its "what good looks like"
   to read it. If the guide has nothing for a family a check needs,
   use the check's own Evidence column and report the gap in the
   summary.
3. **`procedure.md` decides how.** Execute it as written, stage by
   stage: read order, evidence gathering, check execution, verdict
   assembly. Do not reorder, skip, or merge its steps.

If the procedure is silent on a step you need (for example, where to
look for a fact, or which failing check to quote), do not improvise
around it. Take the narrowest reading the rubric allows, and report the
gap in a **Procedure gaps** line in the summary, naming the stage and
what was missing. A gap never turns a fail into a pass.

**The refusal rule.** Before grading, check whether `rubric.md` and
`procedure.md` have content. Ignore HTML comments (`<!-- ... -->`) and
headings; only the author's own written text counts. If `rubric.md` has
no checks row with all four columns filled, or no verdict rule text,
or if `procedure.md` has no step text under any stage heading, refuse
to grade. Say which file is empty and that the tool will not grade
until it is filled, emit no checks and no JSON block, and stop. Never
invent checks or steps at runtime to fill the hole.

## Verdict and output

<!-- State the binary verdict space (accept means submit, reject means
hold) and instruct the tool to end its reply with the fenced JSON
block below, valid and last, with nothing after it. The schema is
CONTRACT.md's, verbatim; do not edit it. -->

The verdict is binary: **`accept`** means the package is ready to
submit; **`reject`** means hold it. There is no third verdict, no
"accept with reservations", and no score. Put reservations in check
evidence lines, never in the verdict.

Lay out the reply in this order:

1. **Per-check summary.** One short line per rubric check, in rubric
   order: the check name, its grade, and the evidence that decided it.
2. **Voice** (live mode only), as described in the voice seam.
3. **Procedure gaps**, only if there were any.
4. **The JSON block**, below.

End the reply with this fenced JSON block, exactly in this schema. It
must be valid JSON, it must be the last thing in the reply, and
nothing may follow it: no closing remark, no second block. Fill it as
follows:

- `item`: in eval mode, the bundle id from its `# Eval package:`
  heading. In live mode, the PR URL if the PR already exists;
  otherwise the compare URL the PR will be opened from,
  `https://github.com/<scoped repo>/compare/main...<fork owner>:<branch>`.
- `checks`: one entry per rubric check, in rubric order, using the
  rubric's check name exactly. `grade` is exactly `pass`, `fail`, or
  `unclear`. `evidence` is one line holding the fact or quote that
  decided the grade; escape any quotes inside it so the JSON stays
  valid.
- `verdict`: exactly `accept` or `reject`, as the rubric's verdict
  rule produces it.

Do not add, rename, reorder, or nest fields.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

<!-- Write the standing rules the tool grades under: evidence first,
grade the thing not the polish, the rubric decides, the procedure
decides how, and how unclear grades are treated when the rubric's
verdict rule is silent. CONTRACT.md states each as a guarantee; your
frame has to make them instructions. -->

Grade under these rules on every run, in both modes:

- **Evidence first.** Before you write a grade, find the fact or quote
  that decides it: a file path in the diff, a line of test output, a
  sentence of the plan or description, a template ask. Put that in the
  evidence line. "Looks fine", "seems complete", and "well written" are
  not evidence; if you cannot name the fact, you cannot grade `pass`.
- **Grade the thing, not the polish.** Read the diff against the plan
  and its deviation notes, the test evidence against the plan's test
  plan and the repo's checks, and the description against the diff.
  Ignore length, formatting, tone, and confidence. A terse PR that
  does what its plan says can be ready; a polished one that drifts is
  not. A PR that honestly discloses a shortfall (a deviation note, a
  check it could not run, a case it leaves out) is graded on what it
  discloses, not failed for being less than everything.
- **The rubric decides, not the run.** Apply each pass condition as
  written. If a check passes by its condition but feels wrong, it
  still passes; if it fails by its condition but feels harsh, it still
  fails. Note the unease in the evidence line or summary as a rubric
  gap; never override the grade.
- **The procedure decides how, not the run.** Follow `procedure.md`
  step by step. Where it is silent, report the gap as Component reads
  describes; never invent a step and pass it off as the procedure.
- **Unclear defaults to fail.** Grade `unclear` only when the package
  genuinely does not let you decide, and say in the evidence line what
  is missing. Then treat it as the rubric's verdict rule directs. Where
  the rule is silent, an `unclear` on a required check counts as a
  fail: a PR you cannot verify from the package is not ready to
  submit. Never upgrade `unclear` to `pass` by giving the author the
  benefit of the doubt.