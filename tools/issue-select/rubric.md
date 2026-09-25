# Rubric: is this a good first issue?

All dates are measured against the bundle's capture date (eval mode) or
today (live mode). "Maintainer" means an OWNER, MEMBER, or COLLABORATOR
author_association; bots are never maintainers.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| not-archived | The `archived:` field on the repo line of Repo facts (live: the archived banner on the repo page). | `archived: no`. | required |
| maintainer-alive | The "last 5 default-branch commits" list (dates and authors) under Repo facts, plus maintainer comments in this issue's thread (live: the repo's commit history). | At least one sign of human upkeep dated within 180 days: a commit authored by a human, or a bot commit that merges a human's pull request, or a maintainer comment in this thread. Bot-only commits (dependency bumps, generated content) do not count. | required |
| unclaimed | The "this issue: assignees / linked PRs" line under Repo facts, plus every PR mention and claim comment ("I'll take this", "working on this", "@bot claim") in the Comments section. | All three hold: (1) no assignee; (2) no open PR aimed at this issue, whether formally linked or only mentioned in the thread, and no merged PR that already resolves it; (3) no claim comment dated within the last 60 days that is still standing. A claim older than 60 days with no open PR from that person is stale and does not block; closed, unmerged PRs are abandoned attempts, not claims. In live mode, apply scope.md's house rule on classmates' claim comments. | required |
| one-bounded-task | The issue title and body, and any maintainer comments about size or approach. | The issue asks for one change that a single PR would close: a bug fix, a docs change, or one small feature. Fail if it is an umbrella, tracking, or "megaissue" (it lists separate sub-issues, or invites an open-ended stream of PRs with no single end state, e.g. "PRs welcome big and small", "incrementally add more"), if a maintainer says the fix needs core or architectural rework, or if it is a usage/support question. A terse body, a missing reproduction, a multi-file checklist that one PR finishes, or optional "additional suggestions" on top of the core fix do not fail this check. | required |
| settled-spec | The issue body, the opener's author_association, the labels, and maintainer comments in the thread. | What "done" means is already decided. Bug reports pass when they describe the wrong behavior; docs tasks pass when they say what to change. A new feature or behavior change additionally needs maintainer endorsement (a maintainer opened it, a triage label such as good first issue, help wanted, or accepted is on it, or a maintainer commented approving the direction), and no maintainer-raised design question may be left open. Fail if the issue or a maintainer says the design still needs confirming and nobody confirmed it, if the thread holds competing proposals with no maintainer decision, or if it is a feature wish with no maintainer endorsement at all (e.g. opened by a non-maintainer or a bot, no labels, no maintainer comment). | required |
| no-abandoned-history | The linked PRs and their states under Repo facts, PRs mentioned in the thread, and the claim/unassign history in the Comments section. | Pass unless the issue has been open more than 1 year AND shows repeated failed attempts: 2 or more closed, unmerged PRs aimed at it, or 3 or more different people who claimed it and dropped it without a merged PR. One old abandoned attempt is normal and passes. | required |
| ai-policy-allows | The "contribution policy" line under Repo facts (live: CONTRIBUTING.md, AI_POLICY.md or similar, and PR templates, per references/evidence-guide.md). | My workflow is AI-assisted with human review. Pass if the policy is silent, welcomes AI, or allows AI under conditions (disclose it, understand and test every change, human review), even if it discourages AI or closes fully-AI or unreviewed PRs. Fail only on an outright ban with no allowed path for reviewed AI-assisted work, e.g. "we do not accept AI-generated code". | required |
| maintainer-vouched | The opener's author_association and the labels on the issue. | Opened by a maintainer, or labeled good first issue / help wanted / easy. | preferred |
| responsive-maintainers | The "maintainer first-response sample" under Repo facts, plus maintainer replies in this thread. | At least one sampled issue opened by a non-maintainer, or this thread, got a maintainer reply within 14 days. | preferred |
| recent-release | The "latest release" line under Repo facts (live: the Releases box). | A release published within the last 12 months. | preferred |

## Verdict rule

Accept if every required check passes; reject if any required check
fails. `unclear` on a required check counts as fail, with one exception:
a thread shown only in part ("first 40 shown") is graded on the comments
shown plus the Repo facts line, and truncation alone is not a reason to
mark a check unclear. Preferred checks never change the verdict; they
only rank accepted issues.
