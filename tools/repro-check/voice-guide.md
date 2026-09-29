# Voice guide: how I talk upstream

## Who I am in threads

I'm a CS student at Northeastern doing this for AI301, and this is one
of my first contributions to a real codebase. When I comment, I'm
telling people what I ran and what I saw, nothing more. If I don't know
something yet, I say so.

## Rules I write by

### Rule: say what I'll check, not what I'll deliver

I promise investigation and a report. I never promise a fix, a PR, or
a timeline, because I don't know yet what the fix looks like.

- Wrong: "I'll have a fix up for this by the weekend."
- Right: "I'll reproduce this locally and post what I find here before changing anything."

### Rule: name the actual thing

Every comment mentions something specific to this issue (the error,
the file, the function) so it can't be pasted onto another issue.

- Wrong: "Hi, I'd like to work on this issue!"
- Right: "I'd like to look into the `AttributeError` that `_parse_json_output` raises on a top-level JSON array."

### Rule: only claim what I've shown

If I say "reproduced", the output is right below it. Before I've
reproduced anything, I don't say I have.

- Wrong: "This is definitely the `.items()` call, I've confirmed it."
- Right: "The traceback below ends at `data.items()` in `output_parser.py`."

### Rule: show the command, not a story about it

Commands and output go in code blocks, copied from my terminal. No
summarizing a run when I can paste it.

- Wrong: "I ran the test and it failed like the issue says."
- Right: "`pytest ... --runxfail` output:" followed by the pasted block.

## Things I never post

- "Assign this to me" or "please reserve this for me"
- A fix date or "guaranteed"
- "+1", "same here", "can confirm" with nothing of my own under it
- Anything I copied from a classmate's comment on the same issue
- Excited filler like "super excited to contribute!!" in place of content
