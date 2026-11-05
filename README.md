# bug-to-test-scenarios-skill

A Claude Code skill that turns a bug report into a small, prioritised set of test scenarios: reproduce the bug, verify the fix, cover variants of the same cause, and guard neighbouring behaviour. Output is a `Test Scenarios | Test Steps | Test Result` table, with traceability, open questions and automation candidates.

This is a generic skill written from scratch from general testing practice. It is not derived from any employer's skill or tooling, and it contains no company-specific URLs, systems or field names.

## Install

Copy `.claude/skills/bug-to-test-scenarios/` into a project's `.claude/skills/` (or `~/.claude/skills/` for all projects), then start a new Claude Code session.

## Quick start

Before a first real run, go through [`PREP-CHECKLIST.md`](PREP-CHECKLIST.md). To show someone the skill in a few minutes, follow [`DEMO.md`](DEMO.md).

1. **Have the bug ready.** Description, steps to reproduce, expected and actual behaviour, environment, and every comment. Add the fix or pull request, and any spec or acceptance criteria, if you have them. Paste the text, save it to a file, or use a tracker tool Claude can reach.
2. **Check permission.** If the bug belongs to an employer or client, make sure you're allowed to use it this way, run the skill inside their tools, and keep their ticket text and data out of personal repos.
3. **Ask**: "Turn BUG-123 into test scenarios" or "What should I test to verify this fix?"
4. **Review the table** before treating it as final. Check that each row maps to the bug report, that gaps are listed as open questions, and that the set is small.
5. **Run the scenarios yourself.** Every row starts as `Not run`. A result changes only after the scenario was executed, on the build and environment you name.
6. **Keep the evidence short**: what was seen, on which build.

## What it will not do

- Invent steps, data or expected behaviour.
- Call a bug fixed or regression-free without the scenarios having been run on the fixed build.
- Create, edit or close tickets, or commit or push, unless asked.

## Status

Not yet tried on a real bug. Treat the first run on one as a trial.

Trial run (2026-11-05): one non-interactive run on a made-up bug (a discount-code total that comes out wrong), with five comments that included a disagreement about the expected total, two unverified claims, and a claimed fix that the run could not see. The skill was invoked and produced nine scenarios in the required three-column table, all `Not run`. It raised the expected-total disagreement as an open question and did not pick a side. It marked the unverified claims as suspected, listed what it cut and which scenarios suit automation, and stated that nothing shown proves the bug is fixed. It wrote no files.

Weak spots seen: one row's traceability said it came from the fix, although the run had not read the fix, and some expected values in the variants were derived by arithmetic from the ticket's rule and not confirmed.

After the trial the skill was tightened for both: rows resting on an unread fix must be named `Neighbour (assumed):`, traceability may not cite a source that was not read, and derived expected values must be labelled derived and listed as open questions. The same bug was then run again against the tightened skill (2026-11-05). Result: the row resting on the unread fix was named `Neighbour (assumed):`, the run said it had not read the fix, and traceability said so too. Derived expected values (19.00, 45.00, 18.00 for a single code) were labelled derived and unconfirmed in their rows and collected under a "Derived values" heading. The run also noticed that 16.00 does not match "10% applied twice" (that would be 16.20) and asked the dev about it. That inconsistency came from the made-up ticket's own numbers, not from a planted trap. One small gap remains: the derived values were listed under their own heading but not repeated in the open questions, as the skill asks. It is still all `Not run`, it took no side on the expected total, and it wrote no files.

That gap was then closed in the skill: whenever there are derived values, the open questions must now include a "Confirm derived values" line. That change was then re-run once on the same bug (2026-11-05). Result: the open questions began with `Confirm derived values:` and listed all three derived values. The earlier fixes held (the `Neighbour (assumed):` row, "I did not read PR-7", the dev's diagnosis cited as "per the comment"), every row was `Not run`, it took no side on the expected total, and it wrote no files.

Small differences in that run, for honesty: it noticed that 16.00 does not match "10% applied twice" (16.20) and put it in the table and the derived-values list, but did not raise it as an open question for the dev, which the previous run did. It also labelled "SAVE10 alone = 18.00" as a stated rule, not a derived value, which is arguable. This is one run, and runs vary, so treat it as encouraging and not proven.

To address the missed question, the skill now says that any inconsistency noticed (numbers that do not match the stated cause, or two comments that disagree) must be raised as an open question, not only noted in a table cell. That change was then re-run once on the same bug (2026-11-05). Result: the 16.00 versus 16.20 mismatch was raised as the first open question for the dev ("Is 16.00 a different defect, a shipping charge, or a rounding or flat-amount effect?"). The earlier fixes held (a `Confirm derived values:` line, the `Neighbour (assumed):` row, "PR-7 was not read", the dev's cause cited as "per the Dev comment"), every row was `Not run`, it took no side on the expected total, and it wrote no files. The scenario set changed a little from the earlier runs, as expected. It is one run, so treat it as encouraging and not proven.

Not exercised by that trial: reading a real fix or pull request, running the scenarios, a real tracker, and a bug with screenshots or logs.
