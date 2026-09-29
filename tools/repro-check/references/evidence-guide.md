# Evidence guide: where proof lives in a reproduction package

## Environment

- Where it lives: eval, the "Environment:" line or table in the
  candidate repro report, and the issue body (plus maintainer comments)
  for what the issue targets. The "bug reports:" line in Repo facts
  says which environment details the repo cares about. Live, the
  environment block of the draft repro comment; the target comes from
  the issue body on GitHub and the repo's `docs/SETUP.md` / README.
- What good looks like: the report names the version it actually ran
  (release number, or branch plus commit) and the OS. If the issue
  names a platform, build type, driver, shell, or branch, the report
  either matches it or says "the issue is X, I ran Y". An older version
  than the issue's target with no note is a mismatch, not a detail.

## Steps

- Where it lives: eval, the steps / commands / code blocks of the
  candidate repro report, plus any issue content the report explicitly
  says it reused. Live, the draft repro comment only: a reader of the
  thread cannot see files in the author's working directory.
- What good looks like: start from nothing (clone, or an empty dir)
  and every input is shown or named exactly. Shell prompts (`$`) with
  the real commands beat descriptions. A step that says what happened
  ("set up the environment", "ran our hook") without the command, or
  that needs a private repo or internal config, is a dead end for a
  stranger.

## Behavior shown

- Where it lives: eval, the output blocks, logs, console excerpts, and
  the Expected/Actual lines in the candidate repro report; the issue
  body's error text, wrong output, or described symptom is what they
  are read against. Live, the same parts of the draft, against the
  issue body on GitHub.
- What good looks like: the captured output shows the same failure the
  issue describes (same exception type and message, same wrong number,
  same missing header), coming from the issue's input. Watch for
  near-misses: a different command or syntax than the issue's, a parse
  or validation error where the issue has a crash, a setup screenshot
  where the issue's symptom should be. "Something failed" is not "this
  failed".

## Honesty

- Where it lives: the sentences in both comments that make a claim:
  "reproduced", "confirmed", "root cause", "verified", "100%",
  "every time", "exactly as described". Hold each one against the
  artifact that is supposed to back it.
- What good looks like: every claim points at output the reader can
  see. A clear "I could not reproduce" with the attempt shown and what
  differed is honest and complete. Red flags: a diagnosis with no
  output behind it, counts of runs used as proof ("ran it ten times"),
  appeals to other people ("everyone has this"), and "exactly as
  described" next to output that does not match the issue.

## Comms

- Where it lives: eval, the candidate claim comment (against the issue
  title/body and thread), and the "contribution policy" line in Repo
  facts for AI-use and comment rules. Live, the draft claim comment,
  the issue on GitHub, and the repo's CONTRIBUTING / AI policy / issue
  and PR templates (for Path Review: `docs/CONTRIBUTING.md` and
  `.github/`).
- What good looks like: the claim names this issue's specifics (the
  error, the file, the scenario) and a next step the author controls,
  like reproducing or investigating, then reporting back. It does not
  assign the issue to anyone, ask for it to be reserved, or promise a
  fix or a date. If the policy requires AI disclosure in issues or
  comments, a disclosure sentence is in one of the comments; if it only
  asks for disclosure in PRs, none is needed yet.
