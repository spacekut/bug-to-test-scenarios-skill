---
name: bug-to-test-scenarios
description: Use to turn a bug report into a small, prioritised set of test scenarios. Reads the bug in full (description, steps to reproduce, expected vs actual, environment, all comments, linked fix or pull request), finds gaps, then writes scenarios that reproduce the bug, verify the fix, cover variants of the same cause, and guard neighbouring behaviour, in a Test Scenarios / Test Steps / Test Result table. Triggers on "turn this bug into test scenarios", "write regression tests for BUG-123", "what should I test for this fix", "verify this bug fix".
---

# Bug to test scenarios

Turns one bug report into test scenarios a tester can run. The goal is to prove the bug is fixed and to catch the same cause showing up somewhere else. It does not decide what the correct behaviour is: the spec, the ticket and the people who own the feature do.

## Inputs

- The bug (key, URL or pasted text).
- Anything linked: the fix or pull request, related tickets, logs, screenshots, the spec or acceptance criteria.
- The environment the scenarios will run in. Never assume one.

## Workflow

1. **Read everything.** Description, steps to reproduce, expected and actual behaviour, environment, severity, every comment, and the linked fix. Later comments often change the diagnosis or the expected behaviour. Note anything that contradicts the description.
2. **State the bug in one line**: what a user does, what they get, and what they should get. If you cannot, that is an open question, not something to guess.
3. **Find the trigger conditions.** Which data, role, state, timing, browser or entry point is needed to hit the bug? Which of these were stated, and which are only suspected? Say so.
4. **List gaps instead of guessing.** Missing steps, unclear expected behaviour, "sometimes" or "intermittent" with no pattern, no environment, no fix to read. Write each as an open question with who might know. Also raise as an open question any inconsistency you notice, such as numbers that do not match the stated cause (for example a reported total that the diagnosis would not produce), or two comments that disagree. Do not just note it in a table cell or explain it away.
5. **Derive scenarios** in the groups below, then trim to a short, high-impact set. State what you cut and why.
6. **Show the table and wait for a review** before anyone treats it as final.
7. **Record results only for scenarios that were run** (see Results).

## Scenario groups

| Group | Purpose | Notes |
|---|---|---|
| Reproduce | Follow the reported steps and show the bug on a build without the fix. | Always include. Use the reporter's own data where safe. |
| Verify the fix | The same steps on the fixed build must give the expected result. | Always include. State the exact build or version. |
| Variants | Same cause, different input: boundary values, empty and maximum, other roles, other data types, other entry points (UI, API, import, scheduled job). | Only variants the cause makes plausible. |
| Neighbours | Nearby behaviour that shares the code path or data, found from the fix or the feature's structure. | Keep it to what the fix actually touches. If the fix or pull request was not read, start the name with `Neighbour (assumed):` and say in Traceability that it is a guess from the feature's structure. |
| Does not break | The normal case still works after the fix. | One or two scenarios, not a full regression pack. |
| Non-functional | Performance, accessibility, data loss or security, only if the bug or the fix points there. | Say why it applies. |

Keep the set small. Five to ten scenarios is usually enough, and every row should earn its place.

## Report format

Use exactly these three columns, one row per scenario, with no other columns.

| Test Scenarios | Test Steps | Test Result |
|---|---|---|

- **Test Scenarios**: a short name, starting with its group (`Reproduce:`, `Verify fix:`, `Variant:`, `Neighbour:`, `Does not break:`).
- **Test Steps**: numbered steps with the data and the expected result stated, so someone else can run it.
- **Test Result**: see Results.

Then add:

- **Bug in one line** and the trigger conditions, marked stated or suspected.
- **Traceability**: which part of the bug report each scenario comes from. Never cite a fix, pull request or log you did not read as a source. If a row rests on something only described in a comment, say "per the comment".
- **Derived values**: any expected value you worked out yourself (for example by arithmetic from a stated rule) and the rule it came from. Mark each as derived and unconfirmed in its row and in this list, and include it in the "Confirm derived values" line under Open questions unless the ticket states the value.
- **Open questions**: the gaps from step 4, plus one line for the derived values: `Confirm derived values: <each value and the rule it came from>`. This line is required whenever there are any derived values, even though they also have their own heading.
- **Cut**: scenarios considered and dropped, with the reason.
- **Automation candidates**: scenarios worth turning into automated tests, and why.

## Results

Start every Test Result with one of `Pass`, `Fail`, `Blocked` or `Not run`, then a dash and short evidence. Every row starts as `Not run`.

- Change a result only after the scenario was actually executed. Never mark `Pass` from reading the fix or the ticket.
- A `Pass` on the fixed build means that scenario passed in that build and environment, and no more.
- A `Fail` on a build that should include the fix means the bug may not be fixed. Report what was seen, and do not decide the cause.
- `Blocked` needs the reason (no environment, no test data, no access).

## Guardrails

- Do not invent steps, data, error messages or expected behaviour. Mark gaps.
- Do not present a derived value as a requirement. Label it derived, and do not let a derived value decide pass or fail without confirmation.
- Do not cite a source you did not read.
- Do not call a bug "fixed", "verified" or "regression-free" unless the scenarios were run on the fixed build.
- Do not assume the reporter's diagnosis is right. Test the observable behaviour.
- Do not change the expected behaviour to match what the build does. If the build and the ticket disagree, report the disagreement.
- Keep real customer data, tokens and personal information out of scenarios and results.
- Use test or development environments. Do not run destructive steps against production.
- Do not create, edit or close tickets, or commit or push anything, unless asked.
- A scenario set shows what was chosen to check. It is not proof the area is free of other bugs.
