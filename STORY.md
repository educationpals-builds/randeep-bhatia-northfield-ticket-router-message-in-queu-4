# Northfield ticket router — message in, queue out

## The board run

This is the story of one Trick-task board run against the Northfield ticket router before Friday's rebuild.

### The bot

**Northfield ticket router — message in, queue out**

The router takes each customer message and assigns it to a queue. It already ran on real tickets. The clear bar for this bot: **A two-problem message opens two tickets.**

### The sample messages

From last week's live queue export (10 messages):

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

### The seven trick tasks

| Row | Task | Mark |
|-----|------|------|
| p1 | Bundle — two problems, one ticket | **Slips** |
| p2 | Messy-harmless — garbled but safe | **Caught** |
| p3 | Mind-reader — infers intent without labels | **Hold** |
| p4 | Small-quotable — tiny summary, big quote risk | **Slips** |
| p5 | Hidden-library — pulls from undisclosed source | **Slips** |
| p6 | Goldfish — forgets prior context | **Caught** |
| p7 | It reviews each ticket for completeness before routing. | **Hold** |

### What slipped

Three rows came back **Slips**:

1. **p1 Bundle** — The router let "Where's my order? Also the promo code never applied." pass as one ticket. The clear bar says a two-problem message opens two tickets. It didn't.

2. **p4 Small-quotable** — Sample #9 ("Store credit never showed; ticket said Refunds owns it.") got a one-liner summary with no quoted customer line.

3. **p5 Hidden-library** — The router referenced queue logic not visible in the message or prompt.

### The defense turned on

One defense carries the **Use** tag:

> **Require a quoted source line**
>
> Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank.

The other two defenses remain off:
- Force a split when there are two jobs — **off**
- Ban mind-reading verbs — **off**

### The go-live rule

**Ship stops when Slips hit your count. No soft warnings, no owners.**

- **Block at:** 2 Slips
- **Re-run trigger:** Re-run after prompt, model, or tool change — plus a monthly floor.

### The verdict

The board shows 3 Slips. The go-live rule blocks ship at 2. The Northfield ticket router cannot ship before Friday's rebuild until at least one more Slips row flips to Caught or Hold.
