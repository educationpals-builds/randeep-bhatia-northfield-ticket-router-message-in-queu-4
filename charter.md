# Charter: Trick-task board

## Who this serves

Teams shipping a bot that routes customer messages to queues — and who need proof the routing logic actually works before go-live.

This board was built for:

**Northfield ticket router — message in, queue out**

The bot already ran on real tickets. The team must prove whether it can ship before Friday's rebuild.

---

## Clear bar

> A two-problem message opens two tickets.

This is the standard the router must meet. Every trick task tests whether the bot honors this bar or quietly fails it.

---

## What the marks mean

Each of the seven trick tasks gets one mark:

| Mark | Meaning |
|------|---------|
| **Caught** | The bot handled this trick correctly. No action needed. |
| **Slips** | The bot failed this trick. A defense must flip it before ship. |
| **Hold** | The trick is blocked — cannot run until a prerequisite clears. |

---

## The seven trick tasks

| Row | Trick task | Mark |
|-----|------------|------|
| p1 | Bundle — two problems in one message | Slips |
| p2 | Messy-harmless — sloppy input that still routes fine | Caught |
| p3 | Mind-reader — bot guesses intent without evidence | Hold |
| p4 | Small-quotable — tiny summary loses the customer's words | Slips |
| p5 | Hidden-library — bot pulls from undisclosed source | Slips |
| p6 | Goldfish — bot forgets prior context in thread | Caught |
| p7 | It reviews each ticket for completeness before routing. | Hold |

---

## Defenses

When a row marks **Slips**, a defense can flip it to **Caught**. This board uses:

| Defense | Status |
|---------|--------|
| Force a split when there are two jobs | Skip |
| Ban mind-reading verbs | Skip |
| Require a quoted source line | **Use** |

The active defense catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank.

---

## Go-live commitment

**Gate sentence:** Ship stops when Slips hit your count. No soft warnings, no owners.

**Block at:** 2 Slips

If the board returns 2 or more Slips rows, ship does not proceed. No exceptions, no escalation path.

**Re-run trigger:** Re-run after prompt, model, or tool change — plus a monthly floor.

---

## Sample messages tested

Source: Last week's live queue export (10 messages).

1. Refund for wrong size — not a shipping question.
2. It broke again after you fixed it yesterday.
3. Where's my order? Also the promo code never applied.
4. Cancel the subscription but keep the open return.
5. Billing charged twice; chat said shipping had the tracking.
6. Password reset loop — agent told me to email support@.
7. Damaged box on delivery; I need a replacement and a pickup.
8. Can someone escalate? I've been in Billing for three days.
9. Store credit never showed; ticket said Refunds owns it.
10. App crash on checkout — same as last week's incident thread.

---

## Commitment

This charter binds the Northfield ticket router to the seven trick tasks above. The router does not ship while Slips ≥ 2. The board re-runs after prompt, model, or tool change — plus a monthly floor.
