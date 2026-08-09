# Trick-Task Board — Northfield ticket router

**Bot under test:** Northfield ticket router — message in, queue out  
**Standard:** A two-problem message opens two tickets.  
**Source:** Last week's live queue export (10 messages).

---

## Task Board (7 tasks)

| ID | Task Name | Test Message | What the Bot Did | Verdict | Defense to Flip Slips |
|----|-----------|--------------|------------------|---------|----------------------|
| p1 | Bundle trap | "Where's my order? Also the promo code never applied." | Routed to one queue instead of opening two tickets for two problems | **Slips** | Force a split when there are two jobs |
| p2 | Messy-but-harmless | "It broke again after you fixed it yesterday." | Routed correctly to support queue despite informal phrasing | **Caught** | — |
| p3 | Mind-reader trap | "Can someone escalate? I've been in Billing for three days." | Blocked — cannot route without explicit queue labels from the message | **Hold** | Ban mind-reading verbs |
| p4 | Small-quotable trap | "Store credit never showed; ticket said Refunds owns it." | Summarized without quoting the customer's actual line | **Slips** | Require a quoted source line |
| p5 | Hidden-library trap | "Password reset loop — agent told me to email support@." | Referenced prior agent instruction without surfacing the source | **Slips** | Require a quoted source line |
| p6 | Goldfish trap | "App crash on checkout — same as last week's incident thread." | Correctly flagged as repeat incident, linked to prior thread | **Caught** | — |
| p7 | Your trick task | "Damaged box on delivery; I need a replacement and a pickup." | It reviews each ticket for completeness before routing. | **Hold** | — |

---

## Summary

| Verdict | Count | Tasks |
|---------|-------|-------|
| Caught | 2 | p2, p6 |
| Slips | 3 | p1, p4, p5 |
| Hold | 2 | p3, p7 |

---

## Active Defense

| Defense ID | Defense Rule | Status |
|------------|--------------|--------|
| split_bundles | Force a split when there are two jobs | off |
| rewrite_mind_read | Ban mind-reading verbs | off |
| name_source | Require a quoted source line | **Use** |

The defense **Require a quoted source line** is active and would flip p4 and p5 from Slips to Caught once enforced in the prompt.

---

## p7 — Your Trick Task

**Ask:** It reviews each ticket for completeness before routing.

**Test message:** "Damaged box on delivery; I need a replacement and a pickup."

**Observation:** The router must check whether the ticket contains enough information (damage description, order number, pickup address) before assigning a queue. This message has two requests (replacement + pickup) but may lack required fields for either.

**Verdict:** Hold — requires manual review to determine if completeness check is functioning.
