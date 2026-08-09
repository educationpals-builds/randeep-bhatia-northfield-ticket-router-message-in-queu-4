# Trick-task Board (GOVERN)

This method audits whether a bot's checks actually split the work before you ship it.

---

## The Seven Board Rows

Each row is a trick task. Run all seven against the bot's real messages. Mark each row with one of three verdicts.

| Row | Trick Task | What It Tests |
|-----|------------|---------------|
| p1 | **Bundle** | Does the bot split a message with two problems into two tickets? |
| p2 | **Messy-harmless** | Does the bot handle messy but harmless input without breaking? |
| p3 | **Mind-reader** | Does the bot invent intent the message never stated? |
| p4 | **Small-quotable** | Does the bot preserve the customer's exact words when quoting? |
| p5 | **Hidden-library** | Does the bot rely on knowledge it can't cite from the message? |
| p6 | **Goldfish** | Does the bot lose context it should have kept? |
| p7 | **Your own trick** | It reviews each ticket for completeness before routing. |

---

## The Three Marks

Every row gets exactly one mark:

### Caught
The bot handled the trick correctly. The check worked. No action needed.

### Slips
The bot failed the trick. The check did not catch the problem. A defense must flip this row before ship.

### Hold
The trick could not be tested with current messages, or the result is ambiguous. Resolve before counting toward go-live.

---

## Defenses: Use and Skip

Each Slips row names a defense that would flip it to Caught.

### Use
Turn this defense on. The bot's prompt or logic must enforce it before ship.

### Skip
Leave this defense off. You accept the risk, or the defense doesn't apply to your domain.

**Available defenses:**

| Defense | What It Catches |
|---------|-----------------|
| Force a split when there are two jobs | Catches: Two problems, one ticket — sample #3 must open two tickets before this router ships. |
| Ban mind-reading verbs | Catches: Sense the real intent — no queue without five labels (or a queue id) from the message. |
| Require a quoted source line | Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank. |

---

## Go-Live Rule

The board produces a go-live rule with two parts:

1. **Slips-to-block threshold**: Ship stops when Slips hit your count. No soft warnings, no owners.
2. **Re-run trigger**: Re-run after prompt, model, or tool change — plus a monthly floor.

If Slips count meets or exceeds the threshold, ship is blocked until defenses flip enough rows to Caught.

---

## Running the Method

1. Gather real messages the bot will face (e.g., last week's live queue export).
2. Run each of the seven trick tasks against those messages.
3. Mark each row: Caught, Slips, or Hold.
4. For every Slips row, name the Use defense that would flip it.
5. Set your slips-to-block threshold.
6. Set your re-run trigger.
7. If Slips ≥ threshold, do not ship. Turn on defenses and re-run until Slips < threshold.
