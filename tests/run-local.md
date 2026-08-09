# Run the Trick-task board locally

This guide shows how to run the seven trick tasks against your own bot and messages — then apply the go-live rule.

---

## What you need

1. **Your bot description** — what it does, who gets hurt when it quietly gets things wrong
2. **Sample messages** — real inputs your bot will face (at least 7)

---

## Step 1: Paste your bot

Describe the bot you're auditing. Example from this build:

> **Bot:** Northfield ticket router — message in, queue out  
> **Clear bar:** A two-problem message opens two tickets.  
> **Source:** Last week's live queue export (10 messages).

---

## Step 2: Paste your messages

Provide real messages your bot will process. Example messages from this build:

```
Refund for wrong size — not a shipping question.
It broke again after you fixed it yesterday.
Where's my order? Also the promo code never applied.
Cancel the subscription but keep the open return.
Billing charged twice; chat said shipping had the tracking.
Password reset loop — agent told me to email support@.
Damaged box on delivery; I need a replacement and a pickup.
Can someone escalate? I've been in Billing for three days.
Store credit never showed; ticket said Refunds owns it.
App crash on checkout — same as last week's incident thread.
```

---

## Step 3: Read the seven marks

The board runs seven trick tasks against your messages. For each task, mark one verdict:

| Task ID | Task name | Verdict options |
|---------|-----------|-----------------|
| p1 | Bundle trap | Caught / Slips / Hold |
| p2 | Messy-but-harmless | Caught / Slips / Hold |
| p3 | Mind-reader trap | Caught / Slips / Hold |
| p4 | Small-quotable trap | Caught / Slips / Hold |
| p5 | Hidden-library trap | Caught / Slips / Hold |
| p6 | Goldfish trap | Caught / Slips / Hold |
| p7 | It reviews each ticket for completeness before routing. | Caught / Slips / Hold |

**Verdict meanings:**
- **Caught** — the bot handles this trap correctly
- **Slips** — the bot fails this trap; a defense can flip it
- **Hold** — blocked until you decide; cannot ship with Hold marks

---

## Step 4: Count Slips rows

Tally how many tasks have a **Slips** verdict.

---

## Step 5: Apply the go-live rule

**Go-live rule from this build:**

> Ship stops when Slips hit your count. No soft warnings, no owners.

**Block threshold:** 2 Slips

**Decision:**
- If Slips count ≥ 2 → **Do not ship.** Turn on defenses to flip Slips rows, then re-run.
- If Slips count < 2 → **Ship is allowed** (assuming no Hold marks remain).

---

## Step 6: Check for Hold marks

Any task marked **Hold** blocks ship until resolved. Clear all Hold marks before go-live.

---

## Step 7: Re-run trigger

Re-run after prompt, model, or tool change — plus a monthly floor.

---

## Quick checklist

- [ ] Bot description pasted
- [ ] Sample messages pasted (7+ messages)
- [ ] All seven tasks marked (Caught / Slips / Hold)
- [ ] Slips count tallied
- [ ] Go-live rule applied (block at 2 Slips)
- [ ] Hold marks resolved
- [ ] Re-run scheduled per trigger rule
