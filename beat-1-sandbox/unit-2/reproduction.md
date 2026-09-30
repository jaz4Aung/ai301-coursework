# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

jaz4Aung

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/53#issuecomment-5903853207

Posted 2026-09-30 by jaz4Aung. Text as posted:

Picking this up: the issue reports that `scrub()` leaves
`(555) 123-4567` unredacted while redacting `555-123-4567` in the same
string, and that `detect()` returns `[]` for the parenthesized format.
I'll check it on a clean checkout of `main` at `f89c06f`. I'm a student
contributor and this is my first contribution to this repo.

Several classmates are on this too; I'm posting my own claim and my own
work.

I have not reproduced it yet. What I'm going to do is verify both of
those behaviors directly, then run the four tests the issue names in
`tests/unit/test_pii_scrubber.py` — `test_us_phone_number_redaction`,
`test_us_phone_formats`, `test_detect_phone_pii`, and
`test_phone_at_start_of_text` — and record which of them fail and how
they fail.

`PII_PATTERNS["phone_us"]` in `safety/pii_scrubber.py` is where I'll
start reading.

I'll follow up on this thread with a reproduction report: my
environment, the exact commands I ran, and the output I actually got.
If it turns out I can't reproduce it, I'll post that result plainly
rather than going quiet.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/53#issuecomment-5904253289

Posted 2026-09-30 by jaz4Aung. Text as posted:

## Reproduction report

**Result: reproduced.** All four tests the issue names fail on a clean
checkout, and the issue's own snippet produces exactly the output the
issue shows.

**Environment.** macOS 26.6.2 (arm64), Python 3.14.4, pytest 9.1.1,
zsh. This repo at commit `f89c06f` (`main`, 2026-09-16), working tree
clean, in a fork I cloned for the work. Two things about this
environment are not the documented path, in case either matters to
someone re-running it:

- The repo requires Python >=3.11 and `[tool.ruff] target-version` is
  `py311`; I ran 3.14.4. `pip install -e ".[dev]"` resolved without
  pins or overrides on that version.
- Docker is not installed on this machine, so I ran no
  `docker compose up -d`, no `make setup`, and created no `.env`. The
  reproduction below is from that state.

**Steps**, from a fresh clone:

```
$ git clone https://github.com/codepath/pathreview-ai301-fa26-s1.git
$ cd pathreview-ai301-fa26-s1
$ git checkout f89c06f           # the commit this report is from
$ uv venv --python 3.14          # or: python3 -m venv .venv
$ uv pip install -e ".[dev]"     # or: .venv/bin/pip install -e ".[dev]"
$ .venv/bin/python -m pytest tests/unit/test_pii_scrubber.py -v -m unit
```

**Observed — which tests fail** (excerpt: the four lines this issue
names, out of 25 collected; the full run ends `20 passed, 5 xfailed`):

```
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_number_redaction XFAIL [ 12%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_formats XFAIL [ 16%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_detect_phone_pii XFAIL [ 48%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_phone_at_start_of_text XFAIL [ 72%]

======================== 20 passed, 5 xfailed in 1.27s =========================
```

**Observed — how they fail.** An `XFAIL` line reports only the status,
not the assertion behind it. Re-running with `--runxfail` to surface
it:

```
$ .venv/bin/python -m pytest tests/unit/test_pii_scrubber.py -m unit --runxfail \
    -k "us_phone_number_redaction or us_phone_formats or detect_phone_pii or phone_at_start_of_text"

________________ TestPIIScrubber.test_us_phone_number_redaction ________________
E       AssertionError: assert '[REDACTED]' in 'Call me at (555) 123-4567'
tests/unit/test_pii_scrubber.py:43: AssertionError
____________________ TestPIIScrubber.test_us_phone_formats _____________________
E           AssertionError: assert '[REDACTED]' in 'Contact: (555) 123-4567'
tests/unit/test_pii_scrubber.py:62: AssertionError
____________________ TestPIIScrubber.test_detect_phone_pii _____________________
E       assert 0 > 0
E        +  where 0 = len([])
tests/unit/test_pii_scrubber.py:140: AssertionError
_________________ TestPIIScrubber.test_phone_at_start_of_text __________________
E       AssertionError: assert '[REDACTED]' in '(555) 123-4567 is my phone number.'
tests/unit/test_pii_scrubber.py:200: AssertionError

======================= 4 failed, 21 deselected in 0.18s =======================
```

Three fail because `scrub()` returned the text unchanged, with the
parenthesized number still in it. The fourth fails because `detect()`
returned an empty list.

**Observed — the issue's own snippet, run directly:**

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

(`detect()` logs through `structlog` on every call, so that `[info]`
line is part of the real output, not noise I added.)

Expected: both numbers redacted, and `detect()` returning one entry for
the parenthesized number. Actual: the parenthesized number is passed
through untouched and `detect()` reports nothing, matching the issue
exactly.

**One test I ran that I have not seen posted here.** AdithNG chose a
literal space over `\s` above, because `\s` would also match newlines,
and flagged that as the one thing not stress-tested beyond this repo's
own suite. This is a test of that concern, not of the bug itself. I
took the shipped `phone_us` pattern and substituted each candidate at
the separator positions, against prose with two unrelated numbers on
consecutive lines:

```
$ .venv/bin/python - <<'PY'
import re
from safety.pii_scrubber import PIIScrubber

cur   = PIIScrubber.PII_PATTERNS["phone_us"]
space = cur.replace("[-.]?", "[-. ]?")
ws    = cur.replace("[-.]?", r"[-.\s]?")
text  = "Reviewer note 555\n123 4567 tickets closed this week."

print("text = %r" % text)
for name, pat in (("[-. ]?  literal space", space), (r"[-.\s]? whitespace   ", ws)):
    m = re.search(pat, text)
    print("%s -> %s" % (name, repr(m.group()) if m else "no match"))
PY

text = 'Reviewer note 555\n123 4567 tickets closed this week.'
[-. ]?  literal space -> no match
[-.\s]? whitespace    -> '555\n123 4567'
```

In this test the `\s` variant matched across the newline and joined two
unrelated numbers into one span; the literal-space variant did not
match. That is one input, not a survey, so I would not call it settled
— but it is a concrete case where the two candidates diverge, on the
kind of multi-line prose this scrubber runs over.

**The fifth xfail, which is not a phone failure.**
`test_mixed_pii_and_text` also xfails, and its marker cites #53 — but
the test contains no parenthesized number (its phone is
`555-123-4567`, which does match). As AdithNG noted above, the
`street_address` pattern is what fails it. On this checkout:

```
$ .venv/bin/python - <<'PY'
from safety.pii_scrubber import PIIScrubber
s = PIIScrubber()
text = "I worked at TechCorp for 5 years developing Python applications."
for d in s.detect(text):
    print("%-16s %r" % (d["type"], d["value"]))
print("scrubbed:", repr(s.scrub(text)))
PY

2026-09-29 21:25:37 [info     ] pii_detected                   count=1 types=1
street_address   '5 years developing Python appl'
scrubbed: 'I worked at TechCorp for [REDACTED]ications.'
```

The match ends at `appl`, and the redaction swallows "Python" — which
is the `assert "Python" in scrubbed` that fails. To check that the
suffix alternation's `Pl` is what reaches into "applications", rather
than assume it, I removed that one alternative and re-ran:

```
$ .venv/bin/python - <<'PY'
import re
from safety.pii_scrubber import PIIScrubber
pat   = PIIScrubber.PII_PATTERNS["street_address"]
text  = "I worked at TechCorp for 5 years developing Python applications."
no_pl = pat.replace("|Pl|", "|")
for name, p in (("shipped pattern      ", pat), ("with |Pl| removed    ", no_pl)):
    m = re.search(p, text, flags=re.IGNORECASE)
    print("%s -> %s" % (name, repr(m.group()) if m else "no match"))
PY

shipped pattern       -> '5 years developing Python appl'
with |Pl| removed     -> no match
```

So it is the `Pl` alternative, matched case-insensitively (`scrub()`
and `detect()` both pass `flags=re.IGNORECASE`,
`safety/pii_scrubber.py:36` and `:52`). Flagging it so the "5 xfailed"
count is not read as five phone failures, and so that marker's #53
reason does not mislead anyone re-running this.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. **Full run (20 packages) — 19/20 agreement, FAILED the bar.** Every proof-family
   category was perfect (clear-accept 8/8, no-evidence 4/4, unfollowable-comms 3/3,
   wrong-target 4/4) but `disclosure 0/1`, so the category floor was unmet. Nineteen
   right answers could not buy back the one category miss.
2. **`--only pkg-03,pkg-05,pkg-07,pkg-09,pkg-12,pkg-20` — 5/6.** Canary after rewriting
   `repo-conventions-met`. These are the six packages whose repo facts state an AI
   policy, i.e. every package the rewrite could touch. `pkg-20` flipped to reject
   (disclosure 1/1, the fix worked) and the four other clear-accepts held, but `pkg-09`
   flipped accept to reject on `behavior-matches-issue` — a check I had not edited.
3. **`--only pkg-09,pkg-10,pkg-17,pkg-18,pkg-20` — 5/5.** Canary after adding a
   cannot-reproduce branch to `behavior-matches-issue`. That change was a loosening, so
   this list is both cannot-reproduce clear-accepts (`pkg-09`, `pkg-10`), the two rejects
   whose text uses cannot-reproduce language and could therefore slip through
   (`pkg-17` wrong-target, `pkg-18` unfollowable-comms), and `pkg-20` as the
   single-package-category canary. All five agreed.
4. **Full confirming run (20 packages) — 20/20 agreement, PASS.** All five categories
   matched: clear-accept 8/8, disclosure 1/1, no-evidence 4/4, unfollowable-comms 3/3,
   wrong-target 4/4. This is the run saved in `eval-run.txt`.

Before any of these I hand-graded three calibration packages (`calib-01`, `calib-02`,
`calib-03`) against the revised rubric at no credit cost. All three agreed with their
gold labels, and the exercise produced three rubric revisions before I spent anything.

**Package analysis**

**`pkg-20`** (ghostty-org/ghostty#13604, category `disclosure`).

**My rubric decided:** accept. **Gold label:** reject. This was my only disagreement on
the first full run, and it was the expensive one — `disclosure` is a one-package
category, so this single miss left the category floor unmet and failed a run that was
otherwise 19/20.

**Why my rubric read it that way.** The package is an excellent reproduction on every
proof check, and my rubric graded it honestly as such: environment, steps, artifact
fidelity, and honesty all passed on the evidence. The failure was in
`repo-conventions-met`. I had written that check to "pass by default" and to fail "only
on a violation of a policy the repo actually states", which I did deliberately, to stop
it over-rejecting clear-accepts on inferred etiquette. The grader applied that wording
exactly as written and recorded: *"Repo requires disclosure only when AI is used; bundle
shows no evidence AI was used in either comment, so no stated policy is violated."*

That reasoning is internally coherent, and it is wrong. My wording had quietly made AI
use something the grader must *prove* before the policy bites. But ghostty's
`AI_POLICY.md` requires that all AI usage in any form be disclosed, and the gold note is
explicit that course packages are treated as AI-assisted work. A disclosure requirement
is not satisfied by the absence of visible AI; it is satisfied only by an affirmative
statement. Silence is non-compliance, not innocence. My check had the right subject and
the wrong burden of proof, so it let a package through on the grounds that nothing
proved it needed to disclose.

**Check rationale**

Quoting the pass condition of `repo-conventions-met` from the `rubric.md` I uploaded to
`tools/repro-check/`, exactly as it now reads:

> Treat the candidate comments as AI-assisted work. The question is never whether
> assistance was used — assume it was — but whether the repo required that to be said out
> loud here. **Fail** if the policy requires disclosing AI assistance in issue comments —
> either by naming issues or comments, or by a blanket ask covering "all AI usage in any
> form" — and neither comment contains an explicit disclosure. Silence never satisfies a
> disclosure requirement; only an affirmative statement does, so absence of proof that AI
> was used is not a defence. **Pass** if the repo states no AI policy at all, or if its
> disclosure ask is scoped to pull requests or code contributions rather than issue
> comments (a policy asking contributors to state the tool "in the pull request", or
> silent on issue comments, imposes nothing on a comment). A policy that asks only for
> human understanding, human voice, or responsibility for AI output is not a disclosure
> requirement: it passes unless the comments plainly break it.

**Why it reads that way.** Two revisions got it here, and the second was forced by the
`pkg-20` miss above.

The first revision was a narrowing. My original version read the repo-facts block's
bug-report *template asks* as conventions too, which meant a package missing an
environment record failed both `environment-recorded` and `repo-conventions-met` for one
defect. Worse, it put the 8 clear-accept packages at risk of being failed for not
matching a template's shape. So I cut the template asks out and said so in the check's
evidence column, leaving them to `environment-recorded` and `evidence-present`.

The second revision fixed the burden of proof, and this is the sentence that matters:
*"Silence never satisfies a disclosure requirement; only an affirmative statement does."*

What I rejected in favour of the present wording was the obvious quick fix: fail any
package whose repo mentions AI disclosure and whose comments do not disclose. That would
have caught `pkg-20` and lost `pkg-09`. Six of the twenty packages have an AI policy and
five of them are clear-accepts, so the set is built to punish exactly that shortcut.
`pkg-09`'s repo does require stating the tool and the extent of its use — but *in the
pull request*, and its facts add that the policy states no disclosure ask for issue
comments. So the check turns on **scope**, not on the presence of the word disclosure: a
blanket ask covering "all AI usage in any form" reaches an issue comment, and an ask
scoped to pull requests does not. The final clause exists for the same reason — `pkg-03`,
`pkg-05`, `pkg-07` and `pkg-12` all have AI policies that demand human understanding,
human voice, or responsibility for AI output, and none of those is a disclosure ask.

**Trade-offs**

The trade-off I want on record is the cannot-reproduce branch I added to
`behavior-matches-issue`, because it is the one that changed a package's result and
because I nearly shipped the contradiction it fixed.

My rubric held two rules that could not both be satisfied. `outcome-stated-honestly` says
an evidenced "I could not reproduce this" passes, because that is a real result a
maintainer can act on. `behavior-matches-issue` demanded that the artifact show the
issue's failure mode. An honest cannot-reproduce cannot do both: its artifact is
*supposed* to show the absence of the bug. `pkg-09` is exactly that package, and the
grader resolved the contradiction differently on two runs — pass on the first full run,
fail on the canary, same package, same gold label, same wording. My 19/20 was therefore
partly luck: had that coin landed the other way I would have seen 18/20 with two
packages to chase instead of one.

The fix reads the report's stated outcome first and asks a different question of a
cannot-reproduce: not "does the artifact show the bug" but "was the attempt aimed at the
mechanism the issue names". That is a loosening, so per the eval README I canaried before
spending a confirming run. The `--only` list was both cannot-reproduce clear-accepts
(`pkg-09`, `pkg-10` — `pkg-10` sat on the identical coin-flip and I would not have known
without looking), the two rejects whose reports use cannot-reproduce language and could
plausibly slip through a loosened check (`pkg-17`, wrong-target; `pkg-18`,
unfollowable-comms), and `pkg-20` for the single-package `disclosure` category. All five
agreed, and the confirming full run reproduced that at 20/20.

**What I accept this check will miss.** The branch passes a faithful attempt that names
what differed, and fails an attempt aimed at some other scenario — but the line between
those is a judgement about the mechanism, not something observable in the artifact. A
report that describes a plausible-sounding attempt at the right feature while quietly
getting a trigger condition wrong would pass, because nothing in the package contradicts
it. `pkg-09` itself is close to that line: it passes because the author names the
conditions that differed (uniform name lengths, a 2 MiB `ARG_MAX`) and says what a
triggering setup would likely need. An author who omitted that paragraph would get the
same pass from my rubric with much weaker grounds for it.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
