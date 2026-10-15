# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

jaz4Aung

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/53#issuecomment-6185032218

Posted 2026-10-15 by jaz4Aung. Text as posted:

Plan for #53, built from my reproduction above (https://github.com/codepath/pathreview-ai301-fa26-s1/issues/53#issuecomment-5904253289).
Several classmates have posted plans here. This one is from my own repro
and will be built on my own fork.

**What my repro showed.**  
On `f89c06f`, `scrub('Call me at (555) 123-4567 or 555-123-4567')` returned
`'Call me at (555) 123-4567 or [REDACTED]'`, `detect()` returned `[]`
for the parenthesized number, and the four tests the issue names failed
with `--runxfail` (`4 failed, 21 deselected`).

**Diagnosis.** Every separator position in `PII_PATTERNS["phone_us"]`
(`safety/pii_scrubber.py:16`) is `[-.]?`: a dash, a dot, or nothing,
never a space. `(555) 123-4567` has a space after `)`, and
`555-123-4567` does not. That fits everything in my repro, and both
`scrub()` and `detect()` read the same pattern.

A check I ran while planning (not in my repro report above) shows the
space isn't the whole story. With only the space added,
`'Call me at (555) 123-4567'` scrubs to `'Call me at ([REDACTED]'` and
`'Contact: +1 555 123 4567'` to `'Contact: +[REDACTED]'`: the match
starts after the `(` or `+` and leaves it behind. On the shipped
pattern, `(555)123-4567` already gives `'([REDACTED]'`. My reading is
that the leading `\b` can't sit between a space and `(` or `+`. I
haven't confirmed that against an alternative yet.

**Scope.** I'll attempt one change, to the `phone_us` line only: make
the three `[-.]?` separators `[-. ]?`, and replace the leading `\b` with
`(?<!\w)` so the match can start at `(` or `+` without starting inside
a longer number. I'll use a literal space, not `\s`, because in my
repro `\s` joined two unrelated numbers across a newline
(`'555\n123 4567'`) and the literal space did not. I'll also remove the
`xfail(strict=True)` markers from the four phone tests, as CONTRIBUTING
asks. If the start change doesn't hold up on the branch, I'll say what
I used instead here. Not touching:
`test_mixed_pii_and_text`, whose failure is the `street_address`
pattern's `Pl` suffix matching into "applications" (shown in my repro),
or any other pattern.

**Test plan.** Re-run my repro steps on the branch. Both numbers in the
snippet should be redacted with no stray `(`, `+1 555 123 4567` should
redact fully (today it isn't redacted at all), a 3-3-4 run inside a
longer number should stay unredacted, `detect()` should return one
`phone_us` entry, and the four named tests should pass with their
markers removed.
The suite should go from `20 passed, 5 xfailed` to `24 passed, 1 xfailed`,
the newline case should still not match, and `make check && make test-unit`
must be green.

**Unknowns.** Allowing a space widens what the pattern matches, and my
newline test is one input, not a survey of false positives. I ran Python
3.14.4 without Docker, so CI's integration and frontend jobs will run
there first.

Next I'll post the before/after output from the branch
`fix/53-phone-paren-space` on my fork.

---

## Your branch

**Branch**

fix/53-phone-paren-space

https://github.com/jaz4Aung/pathreview-ai301-fa26-s1/tree/fix/53-phone-paren-space
(commit `bf67b7d`, on top of `f89c06f`)

**Evidence**

My Unit 2 reproduction steps, run on the same machine (macOS 26.6.2 arm64, Python 3.14.4,
pytest 9.1.1, the fork's `.venv`).

**Before:** `main` at `f89c06f`, from my Unit 2 reproduction report
(https://github.com/codepath/pathreview-ai301-fa26-s1/issues/53#issuecomment-5904253289):

```
$ .venv/bin/python -m pytest tests/unit/test_pii_scrubber.py -v -m unit
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_number_redaction XFAIL [ 12%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_formats XFAIL [ 16%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_detect_phone_pii XFAIL [ 48%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_phone_at_start_of_text XFAIL [ 72%]

======================== 20 passed, 5 xfailed in 1.27s =========================
```

```
$ .venv/bin/python -m pytest tests/unit/test_pii_scrubber.py -m unit --runxfail \
    -k "us_phone_number_redaction or us_phone_formats or detect_phone_pii or phone_at_start_of_text"

E       AssertionError: assert '[REDACTED]' in 'Call me at (555) 123-4567'
tests/unit/test_pii_scrubber.py:43: AssertionError
E           AssertionError: assert '[REDACTED]' in 'Contact: (555) 123-4567'
tests/unit/test_pii_scrubber.py:62: AssertionError
E       assert 0 > 0
E        +  where 0 = len([])
tests/unit/test_pii_scrubber.py:140: AssertionError
E       AssertionError: assert '[REDACTED]' in '(555) 123-4567 is my phone number.'
tests/unit/test_pii_scrubber.py:200: AssertionError

======================= 4 failed, 21 deselected in 0.18s =======================
```

```
$ .venv/bin/python
>>> from safety.pii_scrubber import PIIScrubber
>>> s = PIIScrubber()
>>> s.scrub('Call me at (555) 123-4567 or 555-123-4567')
'Call me at (555) 123-4567 or [REDACTED]'
>>> s.detect('Call me at (555) 123-4567')
2026-09-29 21:25:29 [info     ] pii_detected                   count=0 types=0
[]
```

**After:** branch `fix/53-phone-paren-space` at `bf67b7d`, the same three steps (the
`-v` run is filtered to its phone lines and summary):

```
$ .venv/bin/python -m pytest tests/unit/test_pii_scrubber.py -v -m unit
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_number_redaction PASSED [ 12%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_formats PASSED [ 16%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_international_phone_redaction PASSED [ 20%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_detect_phone_pii PASSED [ 48%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_phone_at_start_of_text PASSED [ 72%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_phone_at_end_of_text PASSED [ 76%]
======================== 24 passed, 1 xfailed in 0.26s =========================
```

```
$ .venv/bin/python -m pytest tests/unit/test_pii_scrubber.py -m unit --runxfail \
    -k "us_phone_number_redaction or us_phone_formats or detect_phone_pii or phone_at_start_of_text"
======================= 4 passed, 21 deselected in 0.20s =======================
```

```
$ .venv/bin/python
>>> from safety.pii_scrubber import PIIScrubber
>>> s = PIIScrubber()
>>> s.scrub('Call me at (555) 123-4567 or 555-123-4567')
'Call me at [REDACTED] or [REDACTED]'
>>> s.detect('Call me at (555) 123-4567')
2026-10-14 23:24:19 [info     ] pii_detected                   count=1 types=1
[{'type': 'phone_us', 'value': '(555) 123-4567', 'start': 11, 'end': 25}]
```

The one remaining xfail is `test_mixed_pii_and_text`, the `street_address` `Pl` failure
my repro separated out and my plan left out of scope. The full unit suite
(`make test-unit`) on the branch: `379 passed, 49 xfailed`. `make typecheck` did not run
to completion on my Python 3.14 venv (numpy stub error, recorded under Deviations in
`plan.md`); `mypy safety/pii_scrubber.py` reports `Success: no issues found in 1 source file`.

## Eval iterations

**Run history**

1. **Attempted run, refused before grading.** The harness printed
   `error: .../rubric.md has no filled-in checks`. My Weight cells were written
   `` `required` `` in backticks, and the harness only reads a row whose last cell is
   exactly `required` or `preferred`. I removed the backticks. Then the same refusal
   came for `procedure.md`, which still held only the template headings, so I wrote it.
2. **Full run (20 packages): 18/20, PASS.** Misses: `pkg-09` (clear-accept, "failed:
   Diagnosis matches evidence") and `pkg-20` (thread-convention, "graded accept").
3. **Full run (20 packages): 17/20, below the bar.** This was after rewriting the two
   rubric rows that still carried another issue's wording (pager, `--paging=never`,
   "binding smoke test") into general terms. `pkg-09` and `pkg-20` still missed, and
   `pkg-14` newly flipped to reject ("failed: Diagnosis matches evidence, Implementation
   is actionable, Comment is aligned with project evidence and context").
4. **`--only pkg-14,pkg-01,pkg-07,pkg-11,pkg-16,pkg-10,pkg-17,pkg-18`: 8/8.** Canary
   after loosening the Diagnosis and Implementation rows (see Trade-offs).
5. **Full confirming run (20 packages): 19/20, PASS.** clear-accept 7/7, scope-creep 4/4,
   thread-convention 1/2, unbuildable 3/3, wrong-cause 4/4. This is the run saved in
   `eval-run.txt`: `agreement: 19/20 scored items  (bar: 18/20: PASS)`.

**Package analysis**

**`pkg-14`** (zellij-org/zellij#5174, category `clear-accept`).

**My rubric decided:** reject on run 3, accept on run 5. **Gold label:** accept. The
gold note: *"honestly scoped-down: reattach handshake fix with a regression-window repro;
defers the untestable Windows variant and says so; arguable on the deferral, ready as
scoped"*.

**Why my rubric read it as reject.** The repro has a control step that, read
literally, sits awkwardly with the plan's cause: *"After `rm -rf ~/.cache/zellij` (thread
observation): the next attach is clean, the one after leaks again."* The plan blames the
reattach path for wiring stdin before OSC responses are consumed, and that path is
present on that clean attach too. On run 3, my rewritten Diagnosis row said a cause
fails if a control step shows the failure *"does not happen when the cause is
present"*. The grader applied that exactly. It did not credit the plan's own account of
that step: *"The cache control fits: with an empty cache the color data is refetched
along the fresh-attach path once."*

The same run also failed it on Implementation is actionable. The plan says *"exact
functions to be pinned in the PR after tracing the query issuance with debug logs, which
I have working"*. My row asked for no guessing of the "code location", and said nothing
about a plan that names the component and a working method to find the function.

**Why it now passes.** I changed both rows. The Diagnosis row now fails a contradicting
control step only when the plan neither explains nor acknowledges it. The Implementation
row now accepts a named file or component plus a concrete, already-working way to pin
the function. `pkg-14` explains its control step and has its debug logs working, so it
passes both.

**Check rationale**

Quoting `Diagnosis matches evidence` from the `rubric.md` in `tools/plan-check/`, exactly
as it reads now:

> | Diagnosis matches evidence | Read the plan's stated cause in **Diagnosis** and **Candidate plan comment** against every step of **Repro evidence**, especially its control steps: any step where a condition was varied (an input, a flag, a setting, a format) and the behavior did or did not change. | Passes only if the proposed cause explains the observed behavior across the reproduction, is consistent with every control step, and distinguishes confirmed evidence from an unverified hypothesis. A cause fails if a control step contradicts it and the plan neither explains that step nor acknowledges it: for example, blaming a component the repro shows can be switched off without changing the failure. A control step the plan explicitly accounts for, with a mechanism consistent with the rest of the evidence, does not fail the check. | required |

**Why it reads that way.** It took two revisions.

The first version was written around one issue. Its evidence column pointed at *"the
comparison between highlighting enabled/disabled and pager enabled/disabled"*, and its
pass condition said *"A pager-only cause fails when the same delay occurs with
`--paging=never`."* That is a good rule for exactly one bug. On every other package the
grader had to guess what "the no-pager result" meant, and my procedure had to carry a
step (step 8) telling it to apply the check "by intent". So I generalised the idea
behind it: a repro's control steps are where a wrong cause gets caught.

The second revision fixed how strict that generalisation was. My first general wording
failed any cause where a control step showed the failure *"does not happen when the
cause is present"*. That flipped `pkg-14`, because real repros have control steps with
side conditions the cause doesn't touch, like a cleared cache. The present wording turns
on whether the plan *accounts for* the control step, not on whether the step looks
awkward at first sight.

What I rejected was deleting the contradiction clause altogether. That would have fixed
`pkg-14` cheaply, but the four `wrong-cause` packages are exactly the plans whose cause a
control step rules out. Without the clause, the check would only ask whether the cause
"explains the observed behavior", and a confident wrong cause usually says it does.

**Trade-offs**

The trade-off is the loosening in the Diagnosis and Implementation rows, and I checked
it with a canary before spending a full run.

Both changes let more plans pass, so the packages at risk were the rejects each row is
there to catch. The `--only` list was:
- the four `wrong-cause` packages (`pkg-01`, `pkg-07`, `pkg-11`, `pkg-16`), for the
  Diagnosis change;
- the three `unbuildable` packages (`pkg-10`, `pkg-17`, `pkg-18`), for the
  Implementation change;
- `pkg-14` itself, to confirm the fix.

All eight agreed (8/8), and the confirming full run held them at wrong-cause 4/4 and
unbuildable 3/3. It also moved `pkg-09` from reject to accept, a clear-accept that had
failed Diagnosis on both earlier full runs. I can't be sure that came from the wording
rather than run-to-run variation, because it's one run.

**What I accept this check will miss.** "Accounts for, with a mechanism consistent with
the rest of the evidence" is a judgement, not something observable. A plan that offers a
plausible-sounding but wrong explanation for an inconvenient control step will pass,
because nothing in the package says the explanation is wrong. `pkg-14` passes on its
cache explanation, which I can't check either.

**What still misses.** `pkg-20` is graded accept on every run. Its gold note says
*"the comment contains no AI-use disclosure and ghostty's stated policy requires
disclosing all AI usage"*. My only check that reads repo policy, `Comment is aligned with
project evidence and context`, is `preferred`, so by my own verdict rule it can never
hold a package. That is a known gap I left open rather than risk the clear-accepts
without a canary for it.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
