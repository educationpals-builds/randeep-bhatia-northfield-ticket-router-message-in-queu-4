# Review Cycle 01 — Northfield ticket router

**Bot under review:** Northfield ticket router — message in, queue out  
**Standard:** A two-problem message opens two tickets.  
**Source:** Last week's live queue export (10 messages).

---

## Board Walk: Seven Trick Tasks

### p1 — Bundle trap
**Test:** Does the router split multi-problem messages into separate tickets?

**Sample tested:**
> Where's my order? Also the promo code never applied.

**Observation:** Router created one ticket containing both problems.

**Mark:** Slips

**Use defense:** Force a split when there are two jobs — *currently OFF*

---

### p2 — Messy-harmless trap
**Test:** Does the router handle messy but single-issue messages correctly?

**Sample tested:**
> Refund for wrong size — not a shipping question.

**Observation:** Router assigned to Refunds queue despite informal phrasing.

**Mark:** Caught

---

### p3 — Mind-reader trap
**Test:** Does the router infer intent without explicit queue labels?

**Sample tested:**
> Can someone escalate? I've been in Billing for three days.

**Observation:** Cannot verify — router requires queue ID or five labels from message; test blocked pending label schema.

**Mark:** Hold

**Use defense:** Ban mind-reading verbs — *currently OFF*

---

### p4 — Small-quotable trap
**Test:** Does the router preserve the customer's original line when summarizing?

**Sample tested:**
> Store credit never showed; ticket said Refunds owns it.

**Observation:** Router summary dropped "ticket said Refunds owns it" — no quoted source line in output.

**Mark:** Slips

**Use defense:** Require a quoted source line — *currently ON*

---

### p5 — Hidden-library trap
**Test:** Does the router reference prior tickets or external context without disclosure?

**Sample tested:**
> It broke again after you fixed it yesterday.

**Observation:** Router pulled yesterday's ticket context but did not cite the source ticket ID in the routing note.

**Mark:** Slips

**Use defense:** Require a quoted source line — *currently ON*

---

### p6 — Goldfish trap
**Test:** Does the router lose context within a single message?

**Sample tested:**
> Damaged box on delivery; I need a replacement and a pickup.

**Observation:** Router retained both replacement and pickup requests in the same ticket assignment.

**Mark:** Caught

---

### p7 — Your own trap
**Test:** It reviews each ticket for completeness before routing.

**Sample tested:**
> Password reset loop — agent told me to email support@.

**Observation:** Cannot verify — no completeness check visible in router output; test blocked pending completeness module.

**Mark:** Hold

---

## Board Summary

| Task | Mark | Defense |
|------|------|---------|
| p1 — Bundle | Slips | Force a split when there are two jobs (OFF) |
| p2 — Messy-harmless | Caught | — |
| p3 — Mind-reader | Hold | Ban mind-reading verbs (OFF) |
| p4 — Small-quotable | Slips | Require a quoted source line (ON) |
| p5 — Hidden-library | Slips | Require a quoted source line (ON) |
| p6 — Goldfish | Caught | — |
| p7 — Your own | Hold | — |

**Slips count:** 3  
**Hold count:** 2  
**Caught count:** 2

---

## Defense State

| Defense | Status |
|---------|--------|
| Force a split when there are two jobs | OFF |
| Ban mind-reading verbs | OFF |
| Require a quoted source line | ON |

---

## Go-Live Rule

**Slips to block:** 2

**Gate sentence:** Ship stops when Slips hit your count. No soft warnings, no owners.

**Re-run trigger:** Re-run after prompt, model, or tool change — plus a monthly floor.

---

## Verdict

**Current Slips:** 3  
**Threshold:** 2

**Result:** BLOCKED — Slips count (3) exceeds slips_to_block (2).

The Northfield ticket router cannot ship until:
- p1 (Bundle) flips to Caught — turn ON "Force a split when there are two jobs"
- p4 (Small-quotable) flips to Caught — defense "Require a quoted source line" is ON but router still dropped the quote
- p5 (Hidden-library) flips to Caught — defense "Require a quoted source line" is ON but router still omitted source citation

With defense "name_source" already ON, p4 and p5 failures indicate the router is not honoring the quoted-source requirement. Fix the router's summary logic before re-running this board.
