# Plan: issue #53, parenthesized US phone numbers pass through the PII scrubber

Issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/53
My repro report: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/53#issuecomment-5904253289
Commit: `f89c06f` (`main`, 2026-09-16)
Branch for the build: `fix/53-phone-paren-space` on my fork (jaz4Aung)

## Summary

`PIIScrubber.scrub()` leaves `(555) 123-4567` unredacted and `detect()`
returns `[]` for it, while the dashed `555-123-4567` is caught. I plan
one change to the `phone_us` regex in `safety/pii_scrubber.py` so the
parenthesized format is redacted and detected. I'll also remove the
`xfail` markers from the four phone tests the issue names. Done means:
the issue's snippet redacts both numbers, `detect()` returns one
`phone_us` entry for the parenthesized number, and those four tests
pass with their markers removed.

## Repro evidence this plan relies on

All quoted from my posted reproduction report (macOS 26.6.2 arm64,
Python 3.14.4, pytest 9.1.1, commit `f89c06f`).

1. The issue's snippet, run directly:

   ```
   >>> s.scrub('Call me at (555) 123-4567 or 555-123-4567')
   'Call me at (555) 123-4567 or [REDACTED]'
   >>> s.detect('Call me at (555) 123-4567')
   2026-09-29 21:25:29 [info     ] pii_detected                   count=0 types=0
   []
   ```

   The dashed number in the same string is redacted, so the scrubber
   runs. Only the parenthesized format is missed.

2. The four named tests fail, three on `scrub()` and one on `detect()`:

   ```
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

   Baseline without `--runxfail`: `20 passed, 5 xfailed in 1.27s`.

3. Separator choice, literal space vs `\s`, on text with two unrelated
   numbers on consecutive lines:

   ```
   text = 'Reviewer note 555\n123 4567 tickets closed this week.'
   [-. ]?  literal space -> no match
   [-.\s]? whitespace    -> '555\n123 4567'
   ```

   With `\s`, the pattern joined two unrelated numbers across a newline.
   With a literal space it did not. That is one input, not a survey.

4. The fifth xfail is not a phone failure. `test_mixed_pii_and_text`
   fails because `street_address` matches into "applications":

   ```
   street_address   '5 years developing Python appl'
   scrubbed: 'I worked at TechCorp for [REDACTED]ications.'
   shipped pattern       -> '5 years developing Python appl'
   with |Pl| removed     -> no match
   ```

5. **Plan-time check, not part of my posted repro.** I ran this while
   writing the plan (plain `python3`, `re` only, same `f89c06f`
   pattern). It runs `re.sub` with the shipped pattern and with the
   space added (`[-.]?` -> `[-. ]?`):

   ```
   shipped       '(555)123-4567'                -> '([REDACTED]'
   shipped       'Call me at (555) 123-4567'    -> 'Call me at (555) 123-4567'
   shipped       'Contact: +1 555 123 4567'     -> 'Contact: +1 555 123 4567'
   [-. ]? space  '(555)123-4567'                -> '([REDACTED]'
   [-. ]? space  'Call me at (555) 123-4567'    -> 'Call me at ([REDACTED]'
   [-. ]? space  'Contact: +1 555 123 4567'     -> 'Contact: +[REDACTED]'
   ```

   Adding the space makes the number match, but the match starts after
   the `(` and after the `+`, so both are left in the scrubbed text.
   The shipped pattern doesn't redact `+1 555 123 4567` at all today.

## Diagnosis

The shipped pattern (`safety/pii_scrubber.py:16`):

```
r"\b(?:\+?1[-.]?)?\(?([0-9]{3})\)?[-.]?([0-9]{3})[-.]?([0-9]{4})\b"
```

Every separator position is `[-.]?`, which allows a dash, a dot, or
nothing, but never a space. `(555) 123-4567` has a space after `)`, so
the pattern cannot match it. `555-123-4567` uses dashes only, so it
matches. This explains every step above: the dashed half of the snippet
is redacted (evidence 1), the parenthesized half is not, and all four
named tests use `(555) 123-4567` (evidence 2). `scrub()` and `detect()`
both loop over the same `PII_PATTERNS` dict (`:35` and `:51`), so one
pattern defect explains both failing methods.

What is confirmed and what is not:
- **Confirmed by my repro:** the behavior. The parenthesized format
  fails in both methods, the dashed format passes, and the four tests
  fail for that reason.
- **Confirmed by the plan-time check (evidence 5):** the missing space
  is what blocks the match. With the space added, the number matches.
- **Second defect, also shown by evidence 5:** the space alone is not
  enough. The match starts after a leading `(` or `+`, leaving that
  character behind (`'Call me at ([REDACTED]'`, `'Contact: +[REDACTED]'`).
  Already true today for `(555)123-4567` -> `'([REDACTED]'`.
- **My reading of why, not yet confirmed by an alternative:** the
  pattern begins with `\b`. A word boundary needs a word character on
  one side, and between a space and `(` or `+` there is none, so the
  match can only begin at the first digit.

## Scope

**In scope:**
- The `phone_us` entry in `PIIScrubber.PII_PATTERNS`
  (`safety/pii_scrubber.py:16`). Nothing else in that file changes.
- Removing `@pytest.mark.xfail(strict=True, reason="issue #53: ...")`
  from the four phone tests in `tests/unit/test_pii_scrubber.py`:
  `test_us_phone_number_redaction`, `test_us_phone_formats`,
  `test_detect_phone_pii`, `test_phone_at_start_of_text`. CONTRIBUTING
  requires this: the markers are `strict=True`, so a fixed test fails
  CI with `XPASS(strict)` until its marker is removed.

**Out of scope:**
- `test_mixed_pii_and_text` and the `street_address` pattern. Evidence 4
  shows that failure is the `Pl` suffix, not a phone format. Its xfail
  marker stays. If anything, I'll note it in the thread as a separate
  defect.
- Other patterns (`email`, `phone_intl`, `ssn`), `scrub()`/`detect()`
  logic, and the `# noqa: B007` line at `:35` (CONTRIBUTING: deliberate,
  don't clean up).
- Wider phone coverage the issue doesn't ask for (extensions,
  international formats beyond the existing `+1` prefix).

## Files I'll touch

- `safety/pii_scrubber.py`: one line, `phone_us`.
- `tests/unit/test_pii_scrubber.py`: remove four `xfail` decorators. No
  test bodies change.

## Approach

1. Branch from `main` at `f89c06f`: `git checkout -b fix/53-phone-paren-space`.
2. Re-run the evidence 5 check on the branch so the before output is
   recorded from the build environment (`.venv`, Python 3.14.4).
3. Edit `phone_us`: change each `[-.]?` to `[-. ]?`. Use a literal space,
   not `\s`, because of evidence 3.
4. In the same line, replace the leading `\b` with `(?<!\w)` ("not
   preceded by a letter, digit or underscore"), so the match can start
   at `(` or `+` but still can't start in the middle of a longer
   word or number. I've tried this only on single strings while
   planning. The test plan rows for `(` and `+` confirm it on the
   branch. If it fails those rows, I'll record what I used instead
   under Deviations.
5. Remove the four `xfail` decorators.
6. Run the test plan below, then `make check && make test-unit`.
7. Commit as `fix(safety): match space-separated and parenthesized US phone numbers`
   with `Fixes #53`, and push to my fork.

## Test plan

Re-run each repro step on the branch. Here's what I expect after the
change:

| Repro step | Before (quoted from repro) | Expected after |
|---|---|---|
| `s.scrub('Call me at (555) 123-4567 or 555-123-4567')` | `'Call me at (555) 123-4567 or [REDACTED]'` | Both numbers replaced. No digits of `(555) 123-4567` remain. |
| `s.detect('Call me at (555) 123-4567')` | `count=0`, `[]` | One entry, `type` `phone_us`, `value` covering the number |
| `pytest tests/unit/test_pii_scrubber.py -m unit --runxfail -k "us_phone_number_redaction or us_phone_formats or detect_phone_pii or phone_at_start_of_text"` | `4 failed, 21 deselected` | `4 passed, 21 deselected` |
| `pytest tests/unit/test_pii_scrubber.py -v -m unit` (markers removed) | `20 passed, 5 xfailed` | `24 passed, 1 xfailed`. The remaining xfail is `test_mixed_pii_and_text`. |
| Newline case from evidence 3 on the new pattern | `[-. ]?` -> `no match` | Still `no match`. Two numbers on separate lines are not joined. |
| `scrub('Call me at (555) 123-4567')` (evidence 5) | space only: `'Call me at ([REDACTED]'` | `'Call me at [REDACTED]'`. No stray `(`. |
| `scrub('Contact: +1 555 123 4567')` (evidence 5) | today: unchanged; space only: `'Contact: +[REDACTED]'` | `'Contact: [REDACTED]'`. No stray `+`. Newly fixed, not preserved. |
| `scrub('order 9995551234567')` | not run yet | Unchanged. No match inside a longer digit run. |

Regression checks:
- `test_us_phone_formats` covers `555-123-4567` and `555.123.4567`,
  which redact today and must still redact. Its other two formats,
  `(555) 123-4567` and `+1 555 123 4567`, don't redact today. They're
  what this change newly fixes.
- `test_text_with_no_pii` and `test_detect_no_false_positives` must
  still pass.
- `make check && make test-unit` must be green, matching CI's `lint`,
  `typecheck` and `test-unit` jobs.

## Risks and unknowns

- **The start-of-match change is less tested than the space.** Evidence
  5 shows the stray `(` and `+`. That `\b` causes it, and that
  `(?<!\w)` fixes it without new false positives, is my reading plus
  single-string tries, not a full suite run. The new test plan rows
  check it on the branch.
- **Allowing a space widens what matches.** Space-separated 3-3-4 digit
  runs in prose (IDs, counts) may now be redacted. My newline test is
  one input. I haven't surveyed false positives beyond the repo's suite.
- **Untested environment parts.** Python 3.14.4 instead of 3.11, and no
  Docker. CI's `test-integration` and `frontend` jobs aren't run
  locally. This change doesn't touch them, but CI is the first place
  they'll run.
- **Shared issue.** Many classmates have posted plans with the same
  diagnosis. This plan comes from my own repro, and I'm building on my
  own fork.

## Deviations

Built as commit `bf67b7d` on `fix/53-phone-paren-space`.

**The code change matches the plan.** One line in `phone_us`: a literal
space at the three separators, and the leading `\b` replaced with
`(?<!\w)`. Plus the four `xfail` markers removed. Every test-plan row
came out as expected:

```
scrub  'Call me at (555) 123-4567 or 555-123-4567' -> 'Call me at [REDACTED] or [REDACTED]'
scrub  'Contact: +1 555 123 4567'                  -> 'Contact: [REDACTED]'
scrub  '(555)123-4567'                             -> '[REDACTED]'
scrub  'order 9995551234567'                       -> 'order 9995551234567'
scrub  'Reviewer note 555\n123 4567 tickets closed this week.' -> unchanged
detect [('phone_us', '(555) 123-4567')]
```

`tests/unit/test_pii_scrubber.py`: `24 passed, 1 xfailed` (the xfail is
`test_mixed_pii_and_text`, out of scope). Full unit suite:
`379 passed, 49 xfailed`. ruff passes.

**Verification differed in one place.** `make typecheck` stopped on
`numpy/__init__.pyi:737: error: Type statement is only supported in
Python 3.12 and greater`. That error comes from my Python 3.14 venv's
numpy stubs against the repo's `python_version = "3.11"`, before mypy
reaches any repo code. `mypy safety/pii_scrubber.py` passes
(`Success: no issues found in 1 source file`). CI's `typecheck` job, on
the documented Python, is the full check. I also ran `black --check .`
(`110 files would be left unchanged`) instead of `make format`, so no
unrelated files were rewritten.

My posted plan is still accurate, so no correction comment is needed on
the issue.
