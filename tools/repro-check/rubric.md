# Rubric: is this reproduction package ready to post?

"The issue's target" means the version, platform, build, or config the
issue says the bug shows up on (issue body plus maintainer comments).
"Artifact" means something captured from a run: terminal output, a log
excerpt, console output, a test result. Prose describing a run is not
an artifact.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| env-recorded | The environment line(s) of the repro report, read against the issue's target (see evidence guide, Environment). | The report names the version of the software it actually ran and the OS it ran on. If the issue is tied to a specific platform, build type, driver, shell, or branch (e.g. "Windows only", "release build", "on main"), the report either ran on that or says plainly how its setup differs. Fail if the version or OS is missing, or if the report ran an older version than the one the issue targets without saying so. | required |
| steps-rerunnable | The steps and commands in the repro report, plus any issue content the report explicitly says it reused (see evidence guide, Steps). | A stranger with the named environment could get from a clean start to the trigger using only what the posted comments and the issue contain: every command is shown, and every input file or config is shown, pointed to exactly ("the issue's 12-line file", "the issue's two `format` calls"), or described precisely enough that any faithful recreation hits the same trigger ("a minimal env.yml with a valid `dependencies:` list plus a `category:` section"). Fail if any step depends on something the reader cannot get (a private repo, an internal config, "our pre-commit hook"), or says what was done without the action ("set up the project"). | required |
| behavior-matches | The artifacts in the repro report, read against the symptom and trigger the issue describes (see evidence guide, Behavior shown). | An artifact shows the issue's own symptom (same kind of failure: the same panic, wrong value, missing header, leaked selector) produced by the issue's trigger or an input the report says is equivalent. For a stated cannot-reproduce, pass if the artifact shows the issue's trigger was run and what came out instead. Fail if there is no artifact, if the artifact only shows setup, or if it shows a different failure (a parse error where the issue has a panic, garbage text where the issue has a crash) or a different input. | required |
| honest-outcome | Every conclusion in the claim comment and repro report ("reproduced", "confirmed", "root cause", "verified", "every time") read against the artifacts shown (see evidence guide, Honesty). | The package's headline outcome (reproduced, could not reproduce, root cause found, verified) is backed by a shown artifact and stated as it happened. An evidenced cannot-reproduce that says what differed passes. A side observation described in prose next to a shown main run (e.g. "dropping the flag gives the right output") does not fail this check; it only loses the control-run credit. Fail if the headline outcome is a reproduction, root cause, or verification the artifacts do not show, or if it leans on things nobody can check ("everyone I know has it", "I ran it ten times") in place of shown output. | required |
| claim-specific | The claim comment, read against the issue title and body (see evidence guide, Comms). | The claim names something only this issue has (its symptom, scenario, file, or function) and says what the author will do next that is within their control (reproduce, investigate, test a patch, report back). Fail if it is generic enough to paste on any issue (+1, "any updates?", pure enthusiasm), if it assigns or reserves the issue for the author, or if it promises a fix or a date. | required |
| policy-respected | The contribution policy line in Repo facts (live: CONTRIBUTING, AI_POLICY, templates), read against both comments (see evidence guide, Comms). | If the repo's policy requires disclosing AI use in issues or comments (or "all AI usage in any form"), at least one of the two comments carries a disclosure. My workflow always involves AI, at minimum this checker, so I treat a required disclosure as always owed. A policy that only asks for disclosure in pull requests, or only asks that comments be in the author's own words, or is silent, passes without one. Also fail if the comments do something the policy explicitly forbids. | required |
| control-run | The repro report's artifacts. | Besides the failing run, the report shows a contrast run (the boundary case, the passing variant the issue mentions, or the flag that turns the bug off). | preferred |

## Verdict rule

Accept if every required check passes; reject if any required check
fails. `unclear` on a required check counts as fail. In live mode on a
claim-only draft, the checks whose evidence is the repro report
(env-recorded, steps-rerunnable, behavior-matches, control-run) are
`unclear` / not yet applicable and are left out; the verdict then rests
on honest-outcome (read against the claim alone), claim-specific, and
policy-respected. Preferred checks never change the verdict.
