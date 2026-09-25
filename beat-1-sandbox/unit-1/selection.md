# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69

**Verdict output**

Live run of my `issue-select` skill on this issue (Path Review house rule applied:
classmates' claim comments do not block a candidate).

```
Live run — codepath/pathreview-ai301-fa26-s3#69
"Output parser crashes on a top-level JSON array fallback"

Required checks:
- not-archived — pass — repo line shows archived: no.
- maintainer-alive — pass — latest default-branch commit 2026-09-16 by Aburke225
  (human, repo maintainer), 9 days before today.
- unclaimed — pass — assignees: none; no PR aimed at #69 (the repo's only open PR,
  #74, targets #68); the 5 thread comments are all classmates' claims, which
  scope.md's Path Review house rule says do not block.
- one-bounded-task — pass — one fix in rag/generator/output_parser.py for a
  top-level JSON array, plus removing one xfail marker; a single PR closes it.
- settled-spec — pass — bug report stating the wrong behavior and the fix, opened by
  the maintainer (Aburke225), labeled bug + good first issue; no open design question.
- no-abandoned-history — pass — opened 2026-09-10 (<1 yr); zero closed/unmerged PRs
  against it.
- ai-policy-allows — pass — no CONTRIBUTING.md/AI policy in the repo; silence permits
  reviewed AI-assisted work.

Preferred (never change the verdict):
- maintainer-vouched — pass — maintainer-opened and good first issue.
- responsive-maintainers — unclear — maintainer commits actively but has not replied
  to the thread's student comments; no response-latency sample gathered.
- recent-release — fail — latest release: none published.

All seven required checks pass -> accept.

{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69",
  "checks": [
    {"name": "not-archived", "grade": "pass", "evidence": "repo line shows archived: no"},
    {"name": "maintainer-alive", "grade": "pass", "evidence": "latest default-branch commit 2026-09-16 by Aburke225 (human maintainer), 9 days before today"},
    {"name": "unclaimed", "grade": "pass", "evidence": "assignees: none; no PR targets #69 (repo's only open PR #74 targets #68); 5 thread comments are classmates' claims, ignored per Path Review house rule"},
    {"name": "one-bounded-task", "grade": "pass", "evidence": "single fix in rag/generator/output_parser.py for top-level JSON array + remove one xfail marker; one PR closes it"},
    {"name": "settled-spec", "grade": "pass", "evidence": "maintainer-opened bug (Aburke225) stating wrong behavior and fix, labeled bug + good first issue; no open design question"},
    {"name": "no-abandoned-history", "grade": "pass", "evidence": "opened 2026-09-10 (<1yr); zero closed unmerged PRs against it"},
    {"name": "ai-policy-allows", "grade": "pass", "evidence": "no CONTRIBUTING.md/AI policy in repo; silence permits reviewed AI-assisted work"},
    {"name": "maintainer-vouched", "grade": "pass", "evidence": "maintainer-opened and labeled good first issue"},
    {"name": "responsive-maintainers", "grade": "unclear", "evidence": "maintainer commits actively but has not replied to student comments in this thread; no latency sample gathered"},
    {"name": "recent-release", "grade": "fail", "evidence": "latest release: none published"}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

**Run history**

- Run 1, smoke run with `--limit 3` (issue-01, issue-02, issue-03): `agreement: 3/3
  scored items`. I only ran this to check the harness and my rubric gave a valid JSON
  verdict. A `--limit` run does not write `eval-run.txt`.
- Run 2, full 20-issue run: `agreement: 20/20 scored items (bar: 18/20: PASS)`, with
  `categories: claimed 4/4  clear-accept 8/8  dead-repo 3/3  policy 1/1  scope 4/4`.

The last score, 20/20, is the one in the committed `eval-run.txt`.

**Issue analysis**

`issue-15` (zulip/zulip#19589, category `scope`). My rubric's decision: reject. Gold
label: reject. What I found interesting is that the usual "someone's already on it"
check does not catch it. The repo-facts line reads `this issue: assignees: none;
linked PRs: zulip/zulip#20840 (closed); zulip/zulip#23123 (closed)`, so my `unclaimed`
check passes: no assignee, no open PR, and the claim comments are years old. The check
that actually rejects it is `no-abandoned-history`. The issue was `opened by esamson
(NONE) on 2021-08-18`, over a year before the 2026-08-05 capture, and it has two
closed, unmerged PRs against it, which is the "2 or more closed, unmerged PRs aimed at
it" case. So it gets rejected for a history of failed attempts, not for being claimed,
and that lines up with the gold note ("years of design debate and two abandoned PRs
behind a friendly label").

**Check rationale**

The check that decided issue-15, quoted from the `rubric.md` in `tools/issue-select/`
as it is currently written:

> | no-abandoned-history | The linked PRs and their states under Repo facts, PRs
> mentioned in the thread, and the claim/unassign history in the Comments section. |
> Pass unless the issue has been open more than 1 year AND shows repeated failed
> attempts: 2 or more closed, unmerged PRs aimed at it, or 3 or more different people
> who claimed it and dropped it without a merged PR. One old abandoned attempt is
> normal and passes. | required |

Why I wrote it this way: a "good first issue" label says an issue is friendly, not
that it is easy. The lecture's four families (maintainer alive, repo in use, scope
fits, nobody on it) miss one failure mode the eval set plants on purpose: an issue that
is unclaimed right now but has already beaten several people. I made the threshold a
conjunction (open more than 1 year AND either 2+ closed unmerged PRs or 3+ people who
dropped it) so normal churn does not trip it. One old abandoned attempt still passes,
because one person walking away is common and doesn't tell you much.

**Trade-offs**

What this check gives up, and why I'm confident it doesn't misfire:

- It lets `issue-09` through on purpose. That issue (conda/conda#7617) was
  `opened by jakirkham (MEMBER) on 2018-08-03`, well over a year old, but it only has
  one closed linked PR (`#11627 (closed)`) and one 2022 claimer. That's a single
  abandoned attempt, under both the 2-PR and 3-dropper thresholds, so it passes and
  gets accepted (gold: accept). If I dropped the threshold to "1 closed PR" it would
  flip issue-09 to reject and I'd lose a clear-accept.
- A case it will miss: a genuinely hard, long-argued issue that never drew any PRs
  (nobody got far enough) leaves no abandoned history for this check to see. My other
  checks cover that, since `settled-spec` fails an unsettled design and
  `one-bounded-task` fails an umbrella, but this check alone would not catch it.

I didn't need a `--only` re-run to tune this. The full run already had issue-15 as
reject and issue-09 as accept, both matching gold, so the conjunction is on the right
side of both.

---

## Selection rationale

**Selection rationale**

1. Fit. It's a Python bug in the RAG output parser, which is the kind of backend/RAG
   work I said I like and not the frontend stuff I wanted to avoid. It's estimated at
   2-4 hours and there's already a test I get to un-xfail, so I can show the fix works.
   That's a good size for one week.

2. What the verdict got right, and what I added. The skill was right that it's
   unclaimed even though four classmates commented to claim it, because our house rule
   ignores those and there's no assignee or real PR on #69. It also confirmed the repo
   is alive, the fix is one PR, and there's no AI policy blocking me. What it couldn't
   do is pick between several RAG bugs it all accepted, so I checked the PR list myself:
   the one open PR (#74) is already on #68, so I skipped #68 and took #69, which no one
   has a PR for yet and is the parsing kind of bug I'm most comfortable with.

3. Difficulty claiming it. The issue is crowded, not locked. It's a popular good first
   issue with four students already on it. Credit is for the PR I open, not a merge, so
   sharing is fine, but I'm basically racing them to a clean PR. The real work is
   reproducing the array-input crash and fixing it without breaking the normal
   object case.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
