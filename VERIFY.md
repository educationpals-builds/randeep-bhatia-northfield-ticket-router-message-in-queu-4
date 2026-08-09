# Verification Checklist — Northfield ticket router

Use this checklist to confirm the Trick-task board works correctly when a stranger runs `/play` against their own bot.

---

## 1. Seven marks returned

The kit must return exactly **7** Caught / Slips / Hold marks — one for each trick task:

| Row | Task | Expected mark |
|-----|------|---------------|
| p1 | Bundle | Slips |
| p2 | Messy-harmless | Caught |
| p3 | Mind-reader | Hold |
| p4 | Small-quotable | Slips |
| p5 | Hidden-library | Slips |
| p6 | Goldfish | Caught |
| p7 | It reviews each ticket for completeness before routing. | Hold |

**Pass condition:** All seven rows appear with one of the three marks. No extra rows. No missing rows.

---

## 2. Every Slips row names a Use defense

When a row is marked **Slips**, the result must name the defense setting that would flip it.

Active defense from this board:

- **Require a quoted source line** (name_source = on)

Slips rows (p1, p4, p5) must each reference a defense that catches the slip. The defense explanation for the active rule:

> Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank.

**Pass condition:** Each Slips row includes the defense name or ID that would address it.

---

## 3. Hostile ask p7 quotes the learner's pick verbatim

The seventh trick task must quote:

> It reviews each ticket for completeness before routing.

**Pass condition:** p7 uses this exact phrase. Do not substitute "churn sensing," "intent detection," or any other soft claim.

---

## 4. Go-live rule quotes the block number verbatim

The go-live rule must state:

> **Block at 2 Slips**

**Pass condition:** The hold number is exactly **2**. Do not invent a different threshold.

---

## 5. Refuses green ship while Slips ≥ 2

When the board shows 2 or more Slips rows, the kit must refuse to return a green "ship" status.

Current board state:
- p1 Bundle → Slips
- p4 Small-quotable → Slips
- p5 Hidden-library → Slips

Total Slips: **3**

**Pass condition:** With 3 Slips, the kit blocks ship. It must not approve launch until Slips < 2.

---

## 6. Domain matches the selected situation only

All examples, sample messages, and queue references must stay within:

- **Bot:** Northfield ticket router — message in, queue out
- **Situation:** This bot routes each customer message to a queue. It already ran on real tickets. You prove whether it can ship before Friday's rebuild.
- **Clear bar:** A two-problem message opens two tickets.
- **Source:** Last week's live queue export (10 messages).

Sample messages from this domain:

- Refund for wrong size — not a shipping question.
- It broke again after you fixed it yesterday.
- Where's my order? Also the promo code never applied.
- Cancel the subscription but keep the open return.
- Billing charged twice; chat said shipping had the tracking.
- Password reset loop — agent told me to email support@.
- Damaged box on delivery; I need a replacement and a pickup.
- Can someone escalate? I've been in Billing for three days.
- Store credit never showed; ticket said Refunds owns it.
- App crash on checkout — same as last week's incident thread.

**Pass condition:** No landlord messages, no HVAC tickets, no lease clauses, no Harbor examples. Only Northfield ticket routing.

---

## Re-run trigger

This board must re-run:

> Re-run after prompt, model, or tool change — plus a monthly floor.

**Pass condition:** The kit states this trigger in the go-live rule output.

---

## Summary

| Check | Criterion |
|-------|-----------|
| ✓ | Exactly 7 marks (p1–p6 + p7) |
| ✓ | Slips rows name a Use defense |
| ✓ | p7 = "It reviews each ticket for completeness before routing." |
| ✓ | Block at = 2 |
| ✓ | No green ship while Slips ≥ 2 |
| ✓ | Domain = Northfield ticket router only |
