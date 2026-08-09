# Trick-task board

A stranger describes the bot they're about to trust — what it does, who gets hurt when it quietly gets things wrong, and a few real messages it will face. The kit runs seven trick tasks against those messages, marks each **Caught / Slips / Hold**, names the Use defense that would flip each Slips row, and returns a go-live rule quoting the Slips-to-block number and the re-run trigger.

---

## Worked example

**Bot:** Northfield ticket router — message in, queue out

**Clear bar:** A two-problem message opens two tickets.

**Source:** Last week's live queue export (10 messages).

**Sample messages:**

> Refund for wrong size — not a shipping question.  
> It broke again after you fixed it yesterday.  
> Where's my order? Also the promo code never applied.  
> Cancel the subscription but keep the open return.  
> Billing charged twice; chat said shipping had the tracking.  
> Password reset loop — agent told me to email support@.  
> Damaged box on delivery; I need a replacement and a pickup.  
> Can someone escalate? I've been in Billing for three days.  
> Store credit never showed; ticket said Refunds owns it.  
> App crash on checkout — same as last week's incident thread.

---

## The seven trick tasks

| Row | Task | Mark | Example from sample messages |
|-----|------|------|------------------------------|
| p1 | **Bundle** — Does the router split a two-problem message into two tickets? | **Slips** | "Where's my order? Also the promo code never applied." — two problems, one ticket risk. |
| p2 | **Messy-harmless** — Does the router handle messy but harmless phrasing without breaking? | **Caught** | "It broke again after you fixed it yesterday." — messy phrasing, routed correctly. |
| p3 | **Mind-reader** — Does the router guess intent without explicit labels? | **Hold** | "Can someone escalate? I've been in Billing for three days." — no queue id in message, router must not invent one. |
| p4 | **Small-quotable** — Does the router preserve the customer's exact words when summarizing? | **Slips** | "Store credit never showed; ticket said Refunds owns it." — one-liner must quote the customer line or stay blank. |
| p5 | **Hidden-library** — Does the router rely on knowledge not in the message? | **Slips** | "Password reset loop — agent told me to email support@." — router must not assume what "support@" means without explicit context. |
| p6 | **Goldfish** — Does the router forget context mid-thread? | **Caught** | "App crash on checkout — same as last week's incident thread." — router links to prior thread correctly. |
| p7 | **Your trick task:** It reviews each ticket for completeness before routing. | **Hold** | "Damaged box on delivery; I need a replacement and a pickup." — completeness check must not invent missing fields. |

---

## Defenses that catch Slips

The following defense is set to **Use**:

| Defense | Status | What it catches |
|---------|--------|-----------------|
| Require a quoted source line | **Use** | Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank. |

Defenses set to **Skip** (available but not active):

| Defense | Status | What it catches |
|---------|--------|-----------------|
| Force a split when there are two jobs | Skip | Catches: Two problems, one ticket — sample #3 must open two tickets before this router ships. |
| Ban mind-reading verbs | Skip | Catches: Sense the real intent — no queue without five labels (or a queue id) from the message. |

---

## Go-live rule

**Block at:** 2 Slips

Ship stops when Slips hit your count. No soft warnings, no owners.

**Re-run trigger:** Re-run after prompt, model, or tool change — plus a monthly floor.

---

## One-paste rebuild

```
Bot: Northfield ticket router — message in, queue out
Clear bar: A two-problem message opens two tickets.
Source: Last week's live queue export (10 messages).

Sample messages:
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

Defense (Use): Require a quoted source line
Block at: 2 Slips
Re-run: Re-run after prompt, model, or tool change — plus a monthly floor.
```

Paste this block to rebuild the board for a different bot or a fresh audit run.

<!-- educationpals-build-verified -->
