# bug-to-test-scenarios-skill

A Claude Code skill that turns a bug report into a small, prioritised set of test scenarios: reproduce the bug, verify the fix, cover variants of the same cause, and guard neighbouring behaviour. Output is a `Test Scenarios | Test Steps | Test Result` table, with traceability, open questions and automation candidates.

This is a generic skill written from scratch from general testing practice. It is not derived from any employer's skill or tooling, and it contains no company-specific URLs, systems or field names.

## Install

Copy `.claude/skills/bug-to-test-scenarios/` into a project's `.claude/skills/` (or `~/.claude/skills/` for all projects), then start a new Claude Code session.

## Quick start

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

Written but not yet tried. Treat the first run as a trial.
