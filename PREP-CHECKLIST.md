# Prep checklist: first real run

One page. Tick these off before, during and after you ask for scenarios.

## 1. Before you start

**Permission**
- [ ] I'm allowed to use this skill with this bug. If it belongs to an employer or client, I've checked their rules, and I'm running it inside their tools and environment.
- [ ] I won't copy their ticket text, data, screenshots or results into a personal repo.

**The bug report**
- [ ] It has steps to reproduce, the expected result and the actual result.
- [ ] I've read **all** the comments. Later comments often change the diagnosis or the expected behaviour.
- [ ] It names the environment (and browser or device, if it matters).
- [ ] Claude can read it: pasted into the chat, saved in a file, or reachable through a tracker tool.

**The fix and the spec**
- [ ] I have the fix or pull request description or diff, if there is one. Without it, "neighbour" scenarios are guesses and will be marked `Neighbour (assumed):`.
- [ ] I have the spec or acceptance criteria, if there are any. Without them, expected behaviour may end up as an open question.

**The environment**
- [ ] I know which build contains the fix, and which build does not, so I can reproduce and then verify.
- [ ] I have test data and accounts I'm allowed to use. Not production data.

**The session**
- [ ] The skill is installed and I started a **new** Claude Code session.

## 2. While you review the table

- [ ] The table has exactly three columns: `Test Scenarios | Test Steps | Test Result`.
- [ ] Every row starts as `Not run`.
- [ ] The set is small (about 5 to 10 rows) and each row traces to the bug report.
- [ ] Rows that rest on a fix nobody read are named `Neighbour (assumed):`.
- [ ] Any expected value the skill worked out itself is labelled **derived** and unconfirmed.
- [ ] Open questions include the gaps, any inconsistency in the numbers or comments, and a `Confirm derived values:` line.
- [ ] The skill did not decide the correct behaviour when the ticket and a comment disagree. It asked.

## 3. After you run the scenarios

- [ ] I changed a result only after actually executing that scenario, on a build I can name.
- [ ] Each result starts with `Pass`, `Fail`, `Blocked` or `Not run`, with short evidence.
- [ ] A `Fail` on a build that should contain the fix is reported as what was seen. I did not guess the cause.
- [ ] I did not call the bug fixed because the pull request says so.
- [ ] I searched the scenarios and results for tokens, personal data and real customer data. There are none.
- [ ] Nothing was created, edited or closed in the tracker, and nothing was committed or pushed, unless I asked.

## 4. Red flags (stop and check)

- A `Pass` for a scenario nobody ran.
- A scenario that cites a fix, pull request or log that was not read.
- A derived number treated as a requirement.
- The skill picked an expected result when the ticket and a comment disagreed.
- Destructive steps aimed at production.
- Real customer data in a scenario or result.

## 5. Keep it honest

A scenario set shows what was chosen to check. It is not proof the area is free of other bugs, and the skill is a starting point for a tester, not a replacement for one.
