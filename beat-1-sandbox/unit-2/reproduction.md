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

MollyMoriJing

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69#issuecomment-5899287916

Hi, I'd like to look into this one for AI301. Per the issue, `rag/generator/output_parser.py` calls `.items()` on whatever `json.loads` returns, so a top-level array like `["a", "b"]` should die with `AttributeError: 'list' object has no attribute 'items'`.

My plan is to set up a fork per `docs/SETUP.md`, call `parse_review_output` directly with an array, and run `test_json_array_fallback` (the H-02 test in `tests/unit/test_output_parser.py`) with `--runxfail` so the real traceback shows up instead of just XFAIL. Reading the file, the ```` ```json ```` fenced branch hands its result to the same `_parse_json_output`, so I'll try a fenced array too and see if it fails the same way. Once I've run both, I'll paste what I got back here, fork setup and all, and hold off on a fix until then.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69#issuecomment-5899400287

Reproduced on current main. Both a raw top-level array and an array inside a ```` ```json ```` fence crash at the same `data.items()` line.

**Environment**
- macOS 15.7.9 (arm64), Python 3.11.11, pytest 9.1.1
- my fork at `2f4e82f` (same as upstream main right now), no local changes
- setup per `docs/SETUP.md`: `cp .env.example .env`, `docker compose up -d`, `make setup` (finished with "Setup complete."). Side note: the `vector-db` container didn't start because something else on my machine already has port 8001, but postgres and redis were healthy and nothing below touches the DB.

**Steps** (from the repo root; I trimmed my home-dir prefix, the `^^^` markers, and log timestamps out of the output, and cut the middle of the long pytest traceback where it says `...`)

1. Raw array, the case in the issue:

```
$ .venv/bin/python -c 'import json; from rag.generator.output_parser import parse_review_output; parse_review_output(json.dumps(["First feedback item", "Second feedback item"]))'
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "rag/generator/output_parser.py", line 48, in parse_review_output
    return _parse_json_output(data)
  File "rag/generator/output_parser.py", line 68, in _parse_json_output
    for key, value in data.items():
AttributeError: 'list' object has no attribute 'items'
```

2. Same array wrapped in a json fence, which goes through the fence branch (line 41) instead of the raw-JSON branch (line 48), and still ends up in the same place:

```
$ .venv/bin/python -c 'from rag.generator.output_parser import parse_review_output; parse_review_output("Here you go:\n```json\n[\"First feedback item\", \"Second feedback item\"]\n```")'
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "rag/generator/output_parser.py", line 41, in parse_review_output
    return _parse_json_output(data)
  File "rag/generator/output_parser.py", line 68, in _parse_json_output
    for key, value in data.items():
AttributeError: 'list' object has no attribute 'items'
```

3. Control, a JSON object instead of an array, parses fine:

```
$ .venv/bin/python -c 'import json; from rag.generator.output_parser import parse_review_output; print(parse_review_output(json.dumps({"projects": "Solid README", "skills": {"suggestions": ["add tests"]}})))'
[info     ] json_output_parsed             section_count=2
[FeedbackSection(section_name='projects', content='Solid README', confidence=0.85, suggestions=[]), FeedbackSection(section_name='skills', content='{"suggestions": ["add tests"]}', confidence=0.9, suggestions=['add tests'])]
```

4. The H-02 test. Normally it just shows XFAIL; with `--runxfail` it fails on the same line:

```
$ .venv/bin/pytest tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback -q -rxX
XFAIL tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback - issue #69 (manifest H-02): output parser calls .items() on a JSON array fallback
1 xfailed in 0.10s

$ .venv/bin/pytest tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback -q --runxfail
...
data = ['First feedback item', 'Second feedback item']
>       for key, value in data.items():
E       AttributeError: 'list' object has no attribute 'items'

rag/generator/output_parser.py:68: AttributeError
FAILED tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback
1 failed in 0.59s
```

**Expected:** a top-level array (raw or fenced) comes back as a list of `FeedbackSection`s or falls through to the plaintext parser. It shouldn't raise.

**Actual:** `AttributeError: 'list' object has no attribute 'items'` from `_parse_json_output` line 68, on both paths. Objects are fine.

Note: since both branches call `_parse_json_output`, I think a fix has to cover the fenced branch too, not just the raw `json.loads` one, otherwise `test_json_array_fallback` would pass while fenced arrays still crash.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run, first draft of the rubric: `agreement: 19/20 scored items (bar: 18/20: PASS)`.
   The miss was pkg-05 (gold accept, mine reject, failed `steps-rerunnable`).
2. `--only pkg-05,pkg-06,pkg-18,pkg-20` after loosening `steps-rerunnable`: 4/4 (partial, no bar).
   pkg-06 and pkg-18 were the canaries for the unfollowable category, pkg-20 for disclosure.
3. Confirming full run with `--save-run`: `agreement: 19/20`, but a different miss: pkg-03
   (gold accept, mine reject, failed `honest-outcome`), which had passed in run 1.
4. `--only pkg-03,pkg-08,pkg-13,pkg-15,pkg-20` after narrowing `honest-outcome`: 5/5 (partial).
5. Confirming full run with `--save-run eval-run.txt`: `agreement: 20/20 scored items
   (bar: 18/20: PASS)`, categories `clear-accept 8/8  disclosure 1/1  no-evidence 4/4
   unfollowable-comms 3/3  wrong-target 4/4`. This is the committed `eval-run.txt`.

**Package analysis**

`pkg-03` (BurntSushi/ripgrep#2779, adjacent multiline matches with `--replace` get wrong line
numbers). Gold: accept. My rubric: accept in run 1, **reject** in run 3, accept again in run 5.

The report reruns the issue's exact 12-line file and `rg -nU ... -r '$1'` command on 15.2.0 and
pastes `1:fnord 2:boccob 3:d321fdddffff 4:clowns`, which is the issue's symptom, so the main
reproduction is solid. The problem was its last sentence: "Dropping `-r '$1'` from the same
command reports 1, 4, 7, 10 correctly", written in prose with no output block. In run 3 the
grader read that as a conclusion without an artifact and failed `honest-outcome`: "'Dropping -r
$1 ... reports 1, 4, 7, 10 correctly' is stated as fact with no output block shown for that
run". My old wording said "Every conclusion ... is backed by something the package shows", so
that read was allowed by my own text, and in run 1 the same grader went the other way. That
flip told me the check was ambiguous, not that the package was bad. The gold label is right:
the thing the report claims (reproduced) is fully shown, and the unshown bit is a side note
that also matches what the owner already said in the thread. After the fix, run 5 graded it
`honest-outcome: pass` ("the flag-dropped observation is prose beside a shown main run, which
per rubric only costs control-run credit") and `control-run: fail`, which is exactly where I
want that missing output to cost something: a preferred check, not the verdict.

**Check rationale**

From `tools/repro-check/rubric.md`, the `honest-outcome` pass condition:

> The package's headline outcome (reproduced, could not reproduce, root cause found, verified) is backed by a shown artifact and stated as it happened. An evidenced cannot-reproduce that says what differed passes. A side observation described in prose next to a shown main run (e.g. "dropping the flag gives the right output") does not fail this check; it only loses the control-run credit. Fail if the headline outcome is a reproduction, root cause, or verification the artifacts do not show, or if it leans on things nobody can check ("everyone I know has it", "I ran it ten times") in place of shown output.

My first version was "Each conclusion is backed by something the package shows ... Fail if the
package claims a reproduction, root cause, or verification its artifacts do not show". "Each
conclusion" was too wide: it let the grader fail pkg-03 over one prose sentence, and it did that
on one run and not the other. I rewrote it around the headline outcome, because that is what
actually gets bad packages posted (pkg-15 claims a root cause with zero output, pkg-13 says
"100% confirm" with nothing pasted, pkg-02 says "confirmed" next to the wrong error). I kept the
"ran it ten times" / "everyone I know" clause on purpose, since pkg-02 and calib-03 both use run
counts in place of the right output. I did not just delete the honesty check and lean on
`behavior-matches`, because pkg-15 has no artifact at all and I wanted a check that names the
over-claim itself, not just the missing output.

**Trade-offs**

Narrowing `honest-outcome` means a report can now make a side claim in prose that is wrong and
still pass, as long as the main reproduction is shown. I accept that miss; the side claim only
costs `control-run`, which never changes the verdict. Because this loosened a check, I re-ran
canaries with `--only` before the confirming run: pkg-15 and pkg-13 (no-evidence, the packages
honesty is supposed to catch), pkg-08 (wrong-target, "conclusively demonstrates" over a
different expression), and pkg-20 (the one disclosure package). All four stayed reject and
pkg-03 moved to accept, and the full run 5 showed no other flips (20/20).

One thing I noticed and left alone: on pkg-20 my `claim-specific` check failed "I'd like to
take this one as a first Ghostty contribution" as self-assigning. That is stricter than I meant,
since "take this" is not the same as "assign it to me". It did not change the verdict, because
`policy-respected` fails pkg-20 anyway (the repo requires disclosing "all AI usage in any form"
and neither comment does). I didn't loosen it because the same check is what rejects pkg-19
("Kindly assign it to me ... fix it within 2 days guaranteed") and pkg-13 ("Assigning myself to
this"), and I didn't want to trade a harmless over-strict read for a risk on those.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
