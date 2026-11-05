# Demo script

A short demo of the skill on a made-up bug. It needs no browser and no real system, only Claude Code. It takes a few minutes, mostly waiting for the answer.

The bug below is invented, with some traps built in on purpose. Nothing here is a real product.

## 1. Set up

1. Make a scratch folder and install the skill into it:

   ```bash
   mkdir demo && cd demo
   mkdir -p .claude/skills
   cp -r /path/to/bug-to-test-scenarios-skill/.claude/skills/bug-to-test-scenarios .claude/skills/
   ```

2. Save the bug below as `BUG-1.md` in that folder.
3. Start a **new** Claude Code session in the folder.

### BUG-1.md

```markdown
# BUG-1: Order total wrong when a discount code and a free-shipping code are both applied

## Description
Shop checkout, staging, Chrome.

Steps to reproduce:
1. Add one item priced 20.00 to the cart.
2. Apply discount code SAVE10 (10% off).
3. Apply code FREESHIP (free shipping).
4. Look at the order total.

Expected: total is 18.00 (20.00 less 10%, shipping free).
Actual: total is 16.00.

## Comments
- QA, day 1: Only happens when FREESHIP is applied second. Applying FREESHIP
  first gives 18.00.
- Support, day 1: A few customers say they expect discounts to stack, so
  maybe 16.00 is the right answer?
- Dev, day 2: Cause found. When the shipping rule runs it recalculates the
  discount on the already-discounted subtotal, so the 10% is applied twice.
  Fix is in PR-7: the recalculation now uses the original subtotal.
- PM, day 2: I think I've also seen this with the 5% loyalty discount but I'm
  not sure.
- QA, day 3: Seems to happen less often for guest checkout, but I haven't
  tried enough times to say.
```

## 2. Ask

```
Turn the bug in BUG-1.md into test scenarios.
```

In a normal interactive session the skill shows the table and waits for your review. Say "looks good" to continue.

## 3. What to point out

- **The table** has exactly three columns, `Test Scenarios | Test Steps | Test Result`, and every result starts as `Not run`. Nothing has been executed.
- **Scenario groups**: Reproduce, Verify fix, Variants, Neighbour, Does not break.
- **The traps** in the bug, and how a good run treats them:

  | Trap | A good run |
  |---|---|
  | Support suggests 16.00 might be correct | Does not pick a side. Asks the product owner to confirm the expected total. |
  | PM "thinks" the loyalty discount is affected | Marks it suspected, not stated. |
  | QA "isn't sure" about guest checkout | Marks it suspected and asks about the pattern. |
  | The dev says the fix is in PR-7 and the skill cannot see it | Says the fix was not read. Names fix-dependent rows `Neighbour (assumed):`. Cites the dev's cause as "per the comment". |
  | 10% applied twice gives 16.20, not 16.00 | Raises the mismatch as an open question for the dev. (This came from the made-up numbers, not a planted trap, but it is a real check.) |

- **Derived values**: any expected number the skill worked out itself (for example 19.00 for a 5% discount on 20.00) is labelled derived and unconfirmed, and appears in a `Confirm derived values:` line under open questions.
- **The closing line**: nothing shown proves the bug is fixed, because nothing was run.

## 4. Be honest about the limits

- It has been run only on this made-up bug, a handful of times. Results vary from run to run: the scenario set changes a little and not every run raises every question. See the trial notes in the README.
- It writes scenarios. It does not run them, and it does not decide what the correct behaviour is.
- It has not been tried on a real bug, a real tracker, or a real fix.

## 5. Clean up

Delete the scratch folder. The conversation itself stays in Claude Code's session history, so don't use real tickets or data in a demo.
