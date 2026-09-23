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

It reads this way because of issue-01. My first version failed anything listing
sub-items to be split, and it rejected issue-01 — which gold accepts. issue-01 is a
conda docs task with five sub-headings under "Proposed changes", and my check saw that
list and called it an umbrella. But it's one docs PR; the gold note calls it a "docs
task with a stated home and scope".

So I was punishing an issue for being well specified, which is backwards — a
file-by-file plan is exactly what a newcomer wants. Now the issue has to call *itself* a
tracking or mega issue, and I said outright that a multi-step change list inside one
deliverable is a spec, not an umbrella. That still catches issue-10, which is literally
titled "Documentation request megaissue".

**Trade-offs**

It gives up **issue-15**, the one disagreement in my 19/20 run. The check can't see an
issue that's bounded on paper but dead in practice: five years old, 97 comments, two
abandoned PRs.

I left it alone. The fix would be something like "reject if open over two years with
closed-unmerged PRs", and that would probably flip **issue-09** too — also old, also one
closed PR (`conda/conda#11627`), but gold accepts it: "the 2022 claim is stale and the
maintainer invited takers". I'd be trading a confirmed pass for a guess.

My canary was the `--only issue-01` re-run: issue-01 flipped `reject` → `accept`,
`agreement: 1/1 scored items`, nothing else touched.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

**1. Fit and time.** I have about 5 hours. #53 is one regex in
`safety/pii_scrubber.py` with four failing tests to turn green, so the fix is small and
most of my time goes to setup and reproducing — the part I haven't done before. It's
Python, and it's in the `safety/` layer of an AI product, which is the kind of work I
want more of.

**2. What the verdict got right, and what I weighed.** It handled `unclaimed` better
than I would have by hand: rejected #60 for an open PR, but didn't block #53 over a
classmate's commit reference. Right call both ways.

What it can't see is how "done" gets defined. It passed #53 and #73 equally, but #53's
four tests tell me when I'm finished and #73's "make the two files agree" doesn't. With
5 hours in a repo I haven't set up, I'd rather the finish line be a test run.

**3. Claiming it.** Should be easy — no assignee, no PR, no comments, and the house rule
says classmates' claims don't block. `Aburke225` replies in about 6 days, so I'll get an
answer, just not fast. Real risk is `Evin009` opening a PR first, so I shouldn't sit on
it.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
