# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/240

**Branch**

`fix/53-phone-paren-space`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Smoke run, `--limit 3` (partial, never scored against the bar): `agreement: 3/3 scored items`
   (pkg-01, pkg-02, pkg-03 all agreed).
2. Full run, saved with `--save-run eval-run.txt`: `agreement: 19/20 scored items  (bar: 18/20: PASS)`

The full run is the last one, and its line matches the agreement line in the committed
`eval-run.txt`. I didn't change any tool file after it: the fingerprints in that file's header
(`SKILL.md sha256:98c61d125049d434`, `rubric.md sha256:72bc8a14540232f0`,
`procedure.md sha256:11badf542247f9a9`, `evidence-guide.md sha256:0115b43c4fb62e48`) match the
files uploaded to `tools/pr-precheck/`.

**Package analysis**

`pkg-16` (minikube, category clear-accept). Gold label: `accept`. My rubric decided `reject`;
the run table reads `pkg-16  clear-accept    accept  reject   NO     failed: Description matches the diff`.

The PR makes `DoLoadImages` return an error when every target fails, and the description says:

> "Deliberately out of scope (recorded in the plan and discussed with @nirs on the thread): the
> start-time image loading path keeps its tolerant behavior; a per-node failure at start is not
> necessarily fatal, and changing that is a separate decision."

My check fails a description that "claims something the diff contradicts". The diff changes
`DoLoadImages` itself, which the start path also calls, so the grader most likely read "the
start-time image loading path keeps its tolerant behavior" as a claim about code the diff
changes, and so as contradicted. (The run records only the failed check's name, not its
reasoning, so this is my reading of it.) The gold note reads the same sentence as an honest
disclosure: "the start-path behavior deliberately deferred with the maintainer discussion cited
in plan and description; arguable on the deferral, ready as scoped". The diff backs that up: it
keeps partial success non-fatal (`// Partial success stays non-fatal for the multi-node start
path.`) and returns an error only `if len(succeeded) == 0`. My check has no clause separating a
disclosed scope decision from a claim about the diff, so it graded the disclosure as if it were
a fidelity claim.

**Check rationale**

From `tools/pr-precheck/rubric.md`:

> | Description matches the diff | Read every factual claim in **Description** (and **Title**) against the **Diff** and **Test evidence**: what was changed, which files, what is "not changed", what was tested. | Passes if every claim about the change is true of the diff and evidence. Fails if the description claims something the diff contradicts (for example, "no changes beyond the plan" over a hunk the plan never names, "tests added" with no test file in the diff, or a fix described that the diff does not contain). Omissions alone do not fail this check; false or contradicted claims do. | required |

It is a separate check from "Diff matches the plan" because the silent-drift category includes
"the description claims fidelity the diff contradicts", which is a different read from
comparing the diff to the plan. calib-03 shows why: its description says "Implements the plan on
#19727 exactly, with no changes beyond it" over a diff with a hunk in a file the plan never
touches, and a grader that trusts the description misses it. I rejected a version that also
failed descriptions for leaving things out, and kept "Omissions alone do not fail this check".
A terse but accurate description has to pass: calib-01's two-sentence description is a clear
accept. Requiring completeness would have turned the check into a grade on the write-up's shape,
not on whether it is true.

**Trade-offs**

The check gives up pkg-16. Because it fails any claim the grader reads as contradicted, it
catches confident fidelity claims like calib-03's, and also catches an honest scope note like
pkg-16's, which it misreads. I considered loosening it with a carve-out for disclosed
out-of-scope decisions, and decided not to. That change could flip silent-drift packages that
agree now (pkg-03, pkg-06, pkg-09, pkg-17 are all `yes`), and with 19/20 I was already above the
18/20 bar, with every category at least 1 (`categories: clear-accept 6/7  not-tested 4/4
silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`). If I make that change later, the
canary run is `--only pkg-16,pkg-03,pkg-06 --include-calibration` to check that pkg-16 flips to
agree while the silent-drift packages and calib-03 still reject.

Nothing else changed after the full run, and here is how I know: the fingerprints in the
`eval-run.txt` header match the uploaded files hash for hash (listed under Run history), so the
committed result is the result of the files being graded.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
