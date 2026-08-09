# Ship Gate — Northfield ticket router

Go-live rule for the Northfield ticket router — message in, queue out.

---

## Hold style

> Ship stops when Slips hit your count. No soft warnings, no owners.

---

## Block threshold

**Slips to block:** 2

When the board shows 2 or more Slips rows, ship stops. No exceptions.

---

## Re-run trigger

> Re-run after prompt, model, or tool change — plus a monthly floor.

---

## Current board summary (7 tasks)

| Task | Verdict |
|------|---------|
| p1 — Bundle split | Slips |
| p2 — Messy harmless | Caught |
| p3 — Mind reader | Hold |
| p4 — Small quotable | Slips |
| p5 — Hidden library | Slips |
| p6 — Goldfish | Caught |
| p7 — It reviews each ticket for completeness before routing. | Hold |

**Slips count this run:** 3 (p1, p4, p5)

---

## Gate decision

The board shows 3 Slips rows. The block threshold is 2.

**Result: Ship stops.**

The Northfield ticket router does not ship until Slips count drops below 2.

---

## Defense that flips Slips

The defense currently set to **Use**:

- **Require a quoted source line** — Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank.

Defenses currently **off** that would flip remaining Slips:

- **Force a split when there are two jobs** — would flip p1 (Bundle split)
- **Ban mind-reading verbs** — would flip p4 (Small quotable), p5 (Hidden library)

---

## What must change before ship

1. Turn on "Force a split when there are two jobs" to flip p1
2. Turn on "Ban mind-reading verbs" to flip p4 and p5
3. Re-run the board after each change
4. Ship only when Slips count is below 2
