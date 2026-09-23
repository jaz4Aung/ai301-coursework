# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/53

**Verdict output**

Live-mode run over three candidates; #53 is the top-ranked accept. Output as emitted,
ending with the fenced JSON block (the chosen issue is the first object, `"verdict":
"accept"`).

````
All three candidates are in the scoped repo (codepath/pathreview-ai301-fa26-s1), so all three are
gradable. Repo-level facts gathered once and applied to each: archived: false, newest default-branch
commit 2026-09-16 (6 days before today), no releases published, docs/CONTRIBUTING.md sets workflow
conditions (conventional commits, green CI, remove xfail markers) but says nothing about AI-assisted
contributions.

Ranked read-out

Accepted — 1st: #53 — PII scrubber fails to redact parenthesized US phone numbers
- not-archived — pass: repo API archived: false, no archive banner
- commits-recent — pass: newest of last 5 commits 2026-09-16T21:42Z, 6 days old
- maintainer-responds (preferred) — pass: #52 and #43 both got a COLLABORATOR (Aburke225) first reply
  in 6d; 6 of 8 sampled threads had no maintainer comment
- repo-in-use (preferred) — pass: releases list empty, and commits-recent passes (2 stars)
- scope-bounded — pass: one regex in safety/pii_scrubber.py, with a repro and four named failing
  tests; no umbrella framing, no design debate (0 comments), no core-internals warning
- unclaimed — pass: assignees: [], no linked PRs in this repo, 0 comments. A referenced event
  (2026-09-21) comes from a commit in a classmate's own repo (Evin009/ai301-coursework), which is
  neither a linked PR nor a claim comment — and under the Path Review house rule classmates' claims
  don't block anyway
- policy-allows-ai — pass: no AI ban in docs/CONTRIBUTING.md, SETUP.md, README, or the PR template

Fit: Python, which you've written; it sits in the safety/ layer of an AI product, so it's
AI-adjacent; and the finish line is the sharpest of the three — four named unit tests define "done,"
plus the @pytest.mark.xfail removal CONTRIBUTING describes. For a first contribution here, that
verifiable finish line is what ranks it above #73.

Accepted — 2nd: #73 — README and .env.example disagree about which LLM API key to set
- Same four repo-level grades as #53. scope-bounded passes (two named files, "make the two files
  agree," 1–2 hours estimated); unclaimed passes (assignees: [], no linked PRs, no comments, no
  timeline references at all).
- Fit: exactly the kind of docs issue your profile wants — getting it right means reading
  core/config.py to see which keys actually exist. Ranked second only because "make them agree" has
  no test defining done, so the finish line is a judgment call rather than a green check.

Rejected: #60 — Faithfulness checker crashes when a context chunk has text: None
- Sunk by unclaimed: PR #74 ("fix(rag): handle None text in faithfulness checker (#60)", by
  nianiiier) is open and cross-referenced on the issue as of 2026-09-21. The rubric fails any issue
  with an open linked PR, and the house rule's exemption covers classmates' claim comments, not an
  open PR. Everything else about it passes — the scope is as tidy as #53's.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/53",
    "checks": [
      {"name": "not-archived", "grade": "pass", "evidence": "Repo API reports archived: false; no archive banner on the repo front page."},
      {"name": "commits-recent", "grade": "pass", "evidence": "Newest of last 5 default-branch commits is 2026-09-16T21:42:18Z, 6 days before today (2026-09-22)."},
      {"name": "maintainer-responds", "grade": "pass", "evidence": "Issues #52 and #43 each got a first reply from COLLABORATOR Aburke225 in 6 days; 6 of 8 sampled threads had no maintainer comment."},
      {"name": "repo-in-use", "grade": "pass", "evidence": "Releases API returns an empty list (none published) while commits-recent passes; 2 stars."},
      {"name": "scope-bounded", "grade": "pass", "evidence": "One phone-number regex in safety/pii_scrubber.py with a repro and four named failing tests; not an umbrella issue, 0 comments so no unsettled design debate."},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: [], no linked PRs in this repo, 0 comments; the only timeline reference is a commit in a classmate's own repo (Evin009/ai301-coursework)."},
      {"name": "policy-allows-ai", "grade": "pass", "evidence": "docs/CONTRIBUTING.md, docs/SETUP.md, README.md and the PR template state no restriction on AI-assisted contributions."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73",
    "checks": [
      {"name": "not-archived", "grade": "pass", "evidence": "Repo API reports archived: false; no archive banner on the repo front page."},
      {"name": "commits-recent", "grade": "pass", "evidence": "Newest of last 5 default-branch commits is 2026-09-16T21:42:18Z, 6 days before today (2026-09-22)."},
      {"name": "maintainer-responds", "grade": "pass", "evidence": "Issues #52 and #43 each got a first reply from COLLABORATOR Aburke225 in 6 days; 6 of 8 sampled threads had no maintainer comment."},
      {"name": "repo-in-use", "grade": "pass", "evidence": "Releases API returns an empty list (none published) while commits-recent passes; 2 stars."},
      {"name": "scope-bounded", "grade": "pass", "evidence": "Two named files (README.md, .env.example), 'Make the two files agree', estimated effort 1-2 hours; no umbrella framing and no maintainer core-internals warning."},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: [], linked PRs none, 0 comments, and the timeline holds only the four label events."},
      {"name": "policy-allows-ai", "grade": "pass", "evidence": "docs/CONTRIBUTING.md, docs/SETUP.md, README.md and the PR template state no restriction on AI-assisted contributions."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60",
    "checks": [
      {"name": "not-archived", "grade": "pass", "evidence": "Repo API reports archived: false; no archive banner on the repo front page."},
      {"name": "commits-recent", "grade": "pass", "evidence": "Newest of last 5 default-branch commits is 2026-09-16T21:42:18Z, 6 days before today (2026-09-22)."},
      {"name": "maintainer-responds", "grade": "pass", "evidence": "Issues #52 and #43 each got a first reply from COLLABORATOR Aburke225 in 6 days; 6 of 8 sampled threads had no maintainer comment."},
      {"name": "repo-in-use", "grade": "pass", "evidence": "Releases API returns an empty list (none published) while commits-recent passes; 2 stars."},
      {"name": "scope-bounded", "grade": "pass", "evidence": "One .get(\"text\", \"\") None-handling fix in rag/evaluator/faithfulness_checker.py with a repro and one named failing test."},
      {"name": "unclaimed", "grade": "fail", "evidence": "Timeline cross-reference 2026-09-21T11:35Z to PR #74 'fix(rag): handle None text in faithfulness checker (#60)', state open."},
      {"name": "policy-allows-ai", "grade": "pass", "evidence": "docs/CONTRIBUTING.md, docs/SETUP.md, README.md and the PR template state no restriction on AI-assisted contributions."}
    ],
    "verdict": "reject"
  }
]
```
````

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

Three runs, in order:

1. `agreement: 2/3 scored items` — smoke run, `--limit 3` (issue-01, issue-02,
   issue-03). issue-01 disagreed.
2. `agreement: 1/1 scored items` — `--only issue-01`, re-graded after rewriting the
   `scope-bounded` check.
3. `agreement: 19/20 scored items  (bar: 18/20: PASS)` — full 20-issue run, saved with
   `--save-run eval-run.txt`. This is the run I committed, and this line is the
   agreement line in that file.

**Issue analysis**

**issue-15** (zulip/zulip#19589).

My rubric decided `accept`. The gold label is `reject`. The committed run records the
disagreement as:

```
issue-15  reject  accept   NO     graded accept
```

The gold note reads "years of design debate and two abandoned PRs behind a friendly
label".

My rubric read it the way it did because none of the failure conditions in my
`scope-bounded` check fire on this bundle. The issue does not declare itself a tracking
or umbrella issue, no maintainer states that the fix touches core internals, it is not a
usage question, and it is not an unspecced feature wish — it has a concrete request. The
signals that actually condemn it are its age (opened 2021-08-18, five years before the
2026-08-05 capture), its 97-comment thread, and its two linked PRs, both closed —
abandoned attempts. My check gathers the linked-PR evidence and then never tests it: the
Evidence column tells the check to look at "whether the `linked PRs:` list shows
closed-unmerged attempts", but no clause in my pass condition uses that observation. The
issue also carries a `good first issue` label, which is the friendly surface the gold
note is pointing at.

**Check rationale**

The `scope-bounded` row of `tools/issue-select/rubric.md`, quoted as it is currently
written:

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| scope-bounded | The issue body and the comment thread: whether the issue declares itself an umbrella, tracking, meta or "mega" issue, whether the design is still being debated with no maintainer decision, whether a maintainer says the fix touches core internals, and whether the `linked PRs:` list shows closed-unmerged attempts. | fails if the issue declares itself a tracking, umbrella, meta or "mega" issue, or its items are explicitly meant to be split across separate PRs or contributors; or the thread shows design still being debated with no maintainer decision; or a maintainer says the fix touches core internals; or it's a usage question; or it's a feature wish with no spec and an unmade product decision. A multi-step change list within one coherent deliverable is a spec, not an umbrella, and passes. A terse body, a missing repro, or no label is not a failure. | required |

The wording is in its current form because of what the first run did to issue-01. My
original condition failed anything that was "an umbrella/tracking/'mega' issue listing
sub-items to be split", and it rejected issue-01 — a gold `accept`. issue-01 is a conda
docs task whose "Proposed changes" section has five sub-headings (add a new task page,
update `manage-pkgs.rst`, update `pip-interoperability.rst`, update `new-features.md`,
consider a `troubleshooting.rst` entry), and my check read that list as an umbrella. The
gold note calls it "docs task with a stated home and scope".

The check was punishing an issue for being well specified, which is backwards: a
file-by-file plan is exactly what a newcomer wants. So I made the umbrella clause require
self-declaration — the issue has to call itself a tracking or mega issue, or say its
items are meant to be split across separate PRs — and added the explicit line "A
multi-step change list within one coherent deliverable is a spec, not an umbrella, and
passes." That still catches issue-10, whose title is literally "Documentation request
megaissue". I also wrote in the carve-out that a terse body, a missing repro, or no label
is not a failure, because the polish of the writeup is not the size of the work.

**Trade-offs**

This check gives up **issue-15**, and I know what it costs.

As written it cannot see an issue that is bounded on paper but dead in practice: old,
heavily debated, with abandoned attempts behind it. issue-15 is that case, and it is the
single disagreement in my committed 19/20 run.

I chose not to close the gap. The obvious fix is a clause like "fails if the issue has
been open more than two years and has closed-unmerged linked PRs" — and that would very
likely flip **issue-09**, a gold `accept`, to reject. issue-09 (conda/conda#7617) is also
old and also carries exactly one closed linked PR, `conda/conda#11627`; its gold note
reads "old but valid bounded feature; the 2022 claim is stale and the maintainer invited
takers". Age plus an abandoned PR is precisely the pattern that clause would punish, and
issue-09 is the case proving the pattern is not decisive on its own. I would be trading a
confirmed pass for an unconfirmed one.

The canary for the current wording is the `--only issue-01` run above: after the rewrite,
issue-01 flipped from `reject` to `accept` (`agreement: 1/1 scored items`) with no other
row touched, so the change is attributable to this check alone.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

**1. Fit to my interests and to the time available.** I have about 5 hours for this. #53
is one phone-number regex in `safety/pii_scrubber.py` with four named failing tests and
an `@pytest.mark.xfail` marker to remove, so the change itself is small and most of my
time can go to setting up the repo and reproducing the failure, which is the part I have
not done before. It is Python, which I have written, and it sits in the `safety/` layer
of an AI product — the AI-adjacent work I said I wanted more of. What I actually want
from this term is to understand what an open-source commitment involves: reading a
codebase I did not write and following its conventions. This repo's CONTRIBUTING asks
for conventional commits, green CI and the xfail marker removed, which is that practice
in a small, checkable form.

**2. What the verdict identified correctly, and what I weighed that it could not.** The
verdict got `unclaimed` right in a way I would probably have gotten wrong by hand: it
rejected #60 because PR #74 is open against it, while *not* blocking #53 over a
classmate's commit reference from their own coursework repo. That is the Path Review
house rule applied in both directions — a classmate's claim does not block, but a real
open PR does — and it is a distinction I would have had to think about.

What the rubric could not weigh is the difference in how "done" is defined. My rubric
passed both #53 and #73 on `scope-bounded` and ranked them by fit, but it has no check
for whether completion is *verifiable*. #53's four named tests mean I know I am finished
when they pass; #73's "make the two files agree" leaves that to my judgment. With 5
hours and a repo I have not set up yet, I would rather the finish line be a test run
than a judgment call. I also noticed the `Evin009` commit referencing #53 — not a claim,
but it tells me I am not the only person looking at this issue, which my rubric records
as evidence and then correctly declines to act on.

**3. Anticipated difficulty in claiming it.** Low, mechanically: no assignee, no linked
PR in this repo, zero comments, and the house rule means I do not need the issue to be
untouched to take it. The maintainer (`Aburke225`) replied within about 6 days on #52 and
#43, so I should expect acknowledgement rather than silence, but not immediately. The
real risk is the `Evin009` reference — someone may open a PR on #53 before I do. Under
the house rule that does not stop me, since credit attaches to the pull request I open
rather than to whether it merges, but it does mean I should not sit on it. I am not
commenting yet; the claim comment is written in Unit 2.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
