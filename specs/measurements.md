# Northfield ticket router — Observable Measurements

What counts as observable evidence for each of the seven trick tasks when auditing the Northfield ticket router — message in, queue out.

---

## Clear bar

> A two-problem message opens two tickets.

---

## Task Observables

### p1_bundle — Two problems, one ticket

**Observable:** Count of tickets created from a single message containing multiple issues.

**What to measure:**
- Input message contains two or more distinct problems
- Output ticket count equals the number of distinct problems

**Sample message to test:**
> Where's my order? Also the promo code never applied.

**Pass condition:** This message creates exactly 2 tickets (one for order status, one for promo code).

**Fail condition:** This message creates 1 ticket bundling both issues.

---

### p2_messy_harmless — Messy but harmless

**Observable:** Queue assignment accuracy when message contains noise, typos, or informal language.

**What to measure:**
- Message contains informal phrasing or unclear structure
- Correct queue is still assigned

**Sample message to test:**
> It broke again after you fixed it yesterday.

**Pass condition:** Routes to the correct queue (likely Support or Returns) despite vague phrasing.

**Fail condition:** Routes to wrong queue or fails to route.

---

### p3_mind_reader — Sense the real intent

**Observable:** Presence of explicit labels or queue identifiers in the routing decision.

**What to measure:**
- The router cites at least five labels or a queue id from the message itself
- No inferred intent without textual evidence

**Sample message to test:**
> Password reset loop — agent told me to email support@.

**Pass condition:** Router cites "password reset" and "support@" as explicit signals before assigning queue.

**Hold condition:** Router assigns queue based on inferred intent without quoting message content.

---

### p4_small_quotable — Tiny summary, big quote risk

**Observable:** Presence of a quoted source line in the ticket summary.

**What to measure:**
- Ticket summary includes a direct quote from the customer message
- Summary does not paraphrase without attribution

**Sample message to test:**
> Store credit never showed; ticket said Refunds owns it.

**Pass condition:** Ticket summary quotes "Store credit never showed" or "ticket said Refunds owns it" verbatim.

**Fail condition:** Ticket summary paraphrases as "Customer inquiring about store credit" without quoting.

---

### p5_hidden_library — Hidden library dependency

**Observable:** Consistency of routing when the same message is processed multiple times or across model versions.

**What to measure:**
- Same input message routes to the same queue on repeated runs
- Routing logic does not depend on undocumented external lookups

**Sample message to test:**
> Billing charged twice; chat said shipping had the tracking.

**Pass condition:** Routes to the same queue (Billing) on three consecutive runs.

**Fail condition:** Routes to Billing, then Shipping, then Billing on three runs.

---

### p6_goldfish — Forgets prior context

**Observable:** Routing decision accounts for message history when relevant.

**What to measure:**
- Router handles messages that reference prior interactions
- Context from "yesterday" or "last week" is acknowledged

**Sample message to test:**
> App crash on checkout — same as last week's incident thread.

**Pass condition:** Router notes the reference to prior incident and routes appropriately (likely Tech Support or Escalation).

**Fail condition:** Router treats as new issue, ignoring "same as last week's incident thread."

---

### p7_your_own — It reviews each ticket for completeness before routing.

**Observable:** Presence of a completeness check before queue assignment.

**What to measure:**
- Router flags incomplete tickets before routing
- Incomplete tickets are held or returned for more information

**Sample message to test:**
> Can someone escalate? I've been in Billing for three days.

**Pass condition:** Router identifies missing information (what issue needs escalation?) and flags for completeness review before routing.

**Hold condition:** Router assigns to Escalation queue without checking whether the original issue is documented.

---

## Measurement Summary Table

| Task | Observable | Sample Message | Pass Signal |
|------|-----------|----------------|-------------|
| p1_bundle | Ticket count vs. problem count | Where's my order? Also the promo code never applied. | 2 tickets created |
| p2_messy_harmless | Correct queue despite noise | It broke again after you fixed it yesterday. | Correct queue assigned |
| p3_mind_reader | Explicit labels cited | Password reset loop — agent told me to email support@. | Labels quoted |
| p4_small_quotable | Source line quoted | Store credit never showed; ticket said Refunds owns it. | Verbatim quote in summary |
| p5_hidden_library | Same route on repeat | Billing charged twice; chat said shipping had the tracking. | Consistent queue |
| p6_goldfish | Prior context noted | App crash on checkout — same as last week's incident thread. | Reference acknowledged |
| p7_your_own | Completeness check | Can someone escalate? I've been in Billing for three days. | Flagged for review |

---

## Source

Last week's live queue export (10 messages).
