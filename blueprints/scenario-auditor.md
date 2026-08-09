# Northfield ticket router — Trick-task board blueprint

This blueprint runs the seven-row board against any bot that routes customer messages to queues. A stranger pastes their bot description, their stakes, and a few real messages. The board returns Caught / Slips / Hold for each task, names the defense that would flip each Slips row, and applies the go-live rule.

---

## Intake paste shape

The stranger provides:

1. **Bot name and job** — what the bot does (e.g., "routes each customer message to a queue")
2. **Stakes** — who gets hurt when it quietly gets things wrong
3. **Sample messages** — at least 5 real messages the bot will face
4. **Clear bar** — the standard the bot must meet

---

## The seven trick tasks

Run each task against the stranger's pasted messages. Mark each row:

| Mark | Meaning |
|------|---------|
| **Caught** | The bot's checks already handle this trap |
| **Slips** | The bot misses this trap — needs a defense |
| **Hold** | Cannot evaluate yet — blocked until more info |

### p1 — Bundle trap

**Test:** Does the bot split multi-problem messages into separate tickets?

**Worked example from Northfield ticket router:**
> "Where's my order? Also the promo code never applied."

This message contains two problems (order status + promo code). The clear bar says: "A two-problem message opens two tickets." If the bot routes this as one ticket, mark **Slips**.

**Defense if Slips:** Force a split when there are two jobs

---

### p2 — Messy-but-harmless trap

**Test:** Does the bot handle messy formatting without breaking?

**Worked example from Northfield ticket router:**
> "It broke again after you fixed it yesterday."

Informal phrasing, no explicit category. If the bot routes it correctly despite the mess, mark **Caught**.

---

### p3 — Mind-reader trap

**Test:** Does the bot infer intent without explicit signals?

**Worked example from Northfield ticket router:**
> "Can someone escalate? I've been in Billing for three days."

The bot must not guess the customer's "real" intent. If it routes based on five labels (or a queue id) from the message, mark **Caught**. If it cannot be evaluated, mark **Hold**.

**Defense if Slips:** Ban mind-reading verbs

---

### p4 — Small-quotable trap

**Test:** Does the bot preserve the customer's exact words when summarizing?

**Worked example from Northfield ticket router:**
> "Store credit never showed; ticket said Refunds owns it."

A one-liner summary must quote the customer line or stay blank. If the bot paraphrases without quoting, mark **Slips**.

**Defense if Slips:** Require a quoted source line

---

### p5 — Hidden-library trap

**Test:** Does the bot rely on knowledge not in the message?

**Worked example from Northfield ticket router:**
> "Password reset loop — agent told me to email support@."

If the bot routes based on internal knowledge (e.g., "support@ goes to Tech") without that mapping being explicit, mark **Slips**.

**Defense if Slips:** Require a quoted source line

---

### p6 — Goldfish trap

**Test:** Does the bot lose context across the message?

**Worked example from Northfield ticket router:**
> "Billing charged twice; chat said shipping had the tracking."

Two departments mentioned. If the bot correctly routes to both or picks the primary without losing the second, mark **Caught**.

---

### p7 — Your own trap

**Test:** It reviews each ticket for completeness before routing.

**Worked example from Northfield ticket router:**
> "Damaged box on delivery; I need a replacement and a pickup."

If the bot routes without checking that all required fields are present, mark **Hold** until the completeness check can be verified.

---

## Use defenses

When a task marks **Slips**, name the defense that would flip it:

| Defense | Status | Catches |
|---------|--------|---------|
| Force a split when there are two jobs | off | Two problems, one ticket — sample #3 must open two tickets before this router ships. |
| Ban mind-reading verbs | off | Sense the real intent — no queue without five labels (or a queue id) from the message. |
| Require a quoted source line | **on** | Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank. |

---

## Go-live gate

**Gate rule:** Ship stops when Slips hit your count. No soft warnings, no owners.

**Slips to block:** 2

**Re-run trigger:** Re-run after prompt, model, or tool change — plus a monthly floor.

---

## Board result shape

Return to the stranger:

1. **Seven marks** — Caught / Slips / Hold for each task (p1–p7)
2. **Defense calls** — for each Slips row, name the defense that would flip it
3. **Go-live rule** — if Slips count ≥ 2, ship stops; otherwise, ship proceeds
4. **Re-run schedule** — when the board must run again

---

## Worked example: Northfield ticket router

**Bot:** Northfield ticket router — message in, queue out

**Clear bar:** A two-problem message opens two tickets.

**Source:** Last week's live queue export (10 messages).

**Sample messages:**
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

**Board result:**

| Task | Mark | Defense if Slips |
|------|------|------------------|
| p1 — Bundle | Slips | Force a split when there are two jobs |
| p2 — Messy-but-harmless | Caught | — |
| p3 — Mind-reader | Hold | Ban mind-reading verbs |
| p4 — Small-quotable | Slips | Require a quoted source line |
| p5 — Hidden-library | Slips | Require a quoted source line |
| p6 — Goldfish | Caught | — |
| p7 — Your own | Hold | — |

**Slips count:** 3

**Go-live decision:** Ship stops. Slips count (3) ≥ slips_to_block (2).

**Re-run:** Re-run after prompt, model, or tool change — plus a monthly floor.
