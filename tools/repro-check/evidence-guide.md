# Evidence guide: where proof lives in a reproduction package

The map for `rubric.md`. For each proof family: where to look, and
what good looks like when you get there.

## Environment

**Where it lives.** In an eval bundle: the repro report's environment
record, usually its opening block or a section headed environment,
setup, or version. The target it is judged against lives elsewhere —
the issue context's title and body excerpt, plus the repo-facts block,
which is where a supported-version or required-configuration statement
appears. In live mode: the environment section of the student's draft
repro report, read against the issue body on GitHub and the repo's
README, CONTRIBUTING, and bug-report template.

**What good looks like.** The operating system, the project version or
commit, and every dependency or setting the issue's own text turns on
are each named with a specific value. "macOS 15.6, yq v4.44.3 (arm64,
installed via Homebrew), zsh" is good; "recent macOS, latest yq" is
not. Where the environment differs from what the issue targets, the
difference is stated out loud rather than left for the reader to
notice — a called-out difference is a finding, a silent one is a hole.

## Steps

**Where it lives.** In an eval bundle: the repro report's
reproduction steps, and anything the claim comment adds about how the
author got there. In live mode: the steps section of the draft, plus
the repo's own setup docs, which tell you whether a step the draft
skips is actually obtainable.

**What good looks like.** A stranger starting from a clean checkout
can walk from the stated starting state to the trigger without
inventing a value. Every command is given as run, every input file is
either pasted or pointed at in the repo, and every configuration
change is spelled out. The test is walkability, never presentation:
one prose sentence containing the exact command is followable, and a
tidy numbered list that says "configure your credentials" and moves on
is not.

## Behavior shown

**Where it lives.** In an eval bundle: the artifacts pasted into the
repro report — output excerpts, log lines, stack traces, screenshot
descriptions, test results. The thing to read them against is the
issue context's description of the failure, including any error text
the issue itself quotes, and the thread highlights where a maintainer
has narrowed the symptom. In live mode: the fenced blocks and attached
images in the draft, read against the issue body and thread on GitHub.

**What good looks like.** The artifact shows the *same failure mode*
the issue reports, matched at the level of the specific symptom: the
same exception type and message, the same wrong value, the same
visible misbehavior. Proximity is not a match. An artifact showing a
parse or syntax error where the issue reports a runtime panic is a
different bug in the same neighbourhood, and reading it as the issue's
bug is the most common way a confident report goes wrong. When the
issue quotes an error string, the artifact should contain that string
or explain why the wording moved.

A report that concludes it could **not** reproduce is read differently
here, and it is a case worth getting right rather than treating as a
failure: its artifact is *supposed* to show the absence of the bug, so
the thing to judge is the aim of the attempt, not its result. Look at
whether the steps went at the mechanism the issue actually names — the
same feature, the same trigger condition — and whether the report shows
what happened instead. A negative result from a faithful attempt, with
the conditions that differed named out loud, is good evidence. A
negative result from an attempt at some other scenario is a
wrong-target report wearing humbler clothes, and says nothing about the
issue.

## Honesty

**Where it lives.** At the seam between the repro report's concluding
statement — "reproduced", "confirmed", "could not reproduce" — and the
artifacts sitting above it. Also in the claim comment, where a
promise ("I will investigate") is checked against a prediction ("I
will have a fix by Friday"). In live mode, the same two places in the
draft.

**What good looks like.** The conclusion is no stronger than the
artifacts underwrite. A report that says "on this environment, with
these steps, the command completed successfully; I could not
reproduce the reported panic" and shows that clean output is a
complete, honest result and reads as ready — a negative finding is
still a finding, and a maintainer can act on it. What fails here is
the gap in either direction: a confident "reproduced!" over an
artifact that shows something else, and a hedge ("might be related",
"seems like") laid over an artifact that plainly shows the bug.

## Comms

**Where it lives.** The repo-facts block is the authority: it carries
the repo's contribution policy and any AI-use disclosure requirement,
frozen as of the capture date. Read it against the claim comment and
the repro report as written. The block also lists the bug-report
template's content asks (OS, version, logs, and so on), but those are
not read here — they are the Environment and Behavior-shown families'
evidence, and reading them twice would score one gap as two.
The issue context's thread highlights show what the thread already
established. In live mode the same facts live in the repo's
CONTRIBUTING.md, its issue templates under `.github/`, its README, and
any AI or automation policy the repo publishes; the thread is the
issue page itself.

**What good looks like.** Every condition the repo's *policy* states is
met in the text of the comments. The case to check first, because it is
easy to miss and impossible to infer: if the repo requires contributors
to disclose AI assistance **in issue comments**, the comments say so
plainly, in the comment itself rather than in a link, a footnote, or a
later edit.

Read the disclosure ask for its *scope*, because that is where these
policies differ and where a careless read goes wrong in both
directions. A blanket rule — "all AI usage in any form must be
disclosed", or one that names issues and comments — reaches the
comments, and a comment that says nothing has not complied: assistance
is assumed, so silence is a failure to disclose, not an absence of
anything to disclose. A rule scoped to the pull request — "state the
tool and the extent of its use in the pull request" — imposes nothing
on an issue comment, and neither does a policy that is simply silent
about comments. And a policy that asks for human understanding, human
voice, or responsibility for AI output is asking for something other
than disclosure; do not read it as a disclosure ask.

Where the repo's facts record no AI policy at all, there is nothing
here to fail — this family judges stated policy, never inferred
etiquette, and never a comment's thinness. Separately, on the claim
comment: specific-and-honest beats boilerplate. A claim that names this
issue's symptom and says what the author will do next is doing work,
and "I'd like to work on this" is a sentence that fits every issue on
GitHub and therefore says nothing about this one.
