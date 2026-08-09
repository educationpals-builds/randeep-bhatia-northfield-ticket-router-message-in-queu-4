# Northfield ticket router — Scenario Analyzer

How the analyzer reads a stranger's paste into the seven board rows and defenses for the Trick-task board.

---

## Input shape

A stranger pastes:

1. **Bot description** — what the bot does, who gets hurt when it quietly gets things wrong
2. **Sample messages** — real messages the bot will face
3. **Clear bar** — the standard the bot must meet

---

## Analyzer steps

### Step 1: Extract the paste

Parse the stranger's input into three fields:

| Field | What to look for |
|-------|------------------|
| Bot description | First paragraph describing the bot's job |
| Sample messages | Numbered or line-separated customer messages |
| Clear bar | A sentence stating the pass/fail standard |

### Step 2: Run each of the seven tasks

For each task, the analyzer reads the paste and marks **Caught**, **Slips**, or **Hold**.

| Task ID | Task name | What the analyzer checks |
|---------|-----------|--------------------------|
| p1_bundle | Two problems, one ticket | Does any message contain two distinct jobs? If the bot routes it to one queue, mark Slips. |
| p2_messy_harmless | Messy but harmless | Does the bot handle informal phrasing without breaking? If yes, mark Caught. |
| p3_mind_reader | Sense the real intent | Does the bot infer intent without explicit labels or queue IDs? If no evidence either way, mark Hold. |
| p4_small_quotable | Tiny summary, big quote risk | Does the bot summarize without quoting the customer line? If yes, mark Slips. |
| p5_hidden_library | Hidden library lookup | Does the bot reference external data not in the message? If yes, mark Slips. |
| p6_goldfish | Forgets prior context | Does the bot lose context from earlier in the thread? If no, mark Caught. |
| p7_your_own | It reviews each ticket for completeness before routing. | Does the bot check completeness before routing? If no evidence either way, mark Hold. |

### Step 3: Map Slips rows to defenses

For each row marked **Slips**, the analyzer names the defense that would flip it:

| Task ID | Defense ID | Defense label |
|---------|------------|---------------|
| p1_bundle | split_bundles | Force a split when there are two jobs |
| p3_mind_reader | rewrite_mind_read | Ban mind-reading verbs |
| p4_small_quotable | name_source | Require a quoted source line |
| p5_hidden_library | name_source | Require a quoted source line |

### Step 4: Check defense state

The analyzer reads which defenses are currently ON or OFF:

| Defense ID | Current state |
|------------|---------------|
| split_bundles | off |
| rewrite_mind_read | off |
| name_source | on |

If a defense is ON, the corresponding Slips row may flip to Caught on re-test.

If a defense is OFF (still off), the analyzer reports it as **Skip/unset** — no rewrite module is invented.

### Step 5: Apply the go-live rule

Count the Slips rows. Compare to the threshold:

- **slips_to_block**: 2

If Slips count ≥ 2, ship stops. No soft warnings, no owners.

### Step 6: Note the re-run trigger

Re-run after prompt, model, or tool change — plus a monthly floor.

---

## Worked example: Northfield ticket router

**Bot**: Northfield ticket router — message in, queue out

**Clear bar**: A two-problem message opens two tickets.

**Source**: Last week's live queue export (10 messages).

**Sample messages**:

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

### Analyzer output

| Task ID | Task | Verdict | Note |
|---------|------|---------|------|
| p1_bundle | Two problems, one ticket | Slips | Message #3 ("Where's my order? Also the promo code never applied.") contains two jobs. |
| p2_messy_harmless | Messy but harmless | Caught | |
| p3_mind_reader | Sense the real intent | Hold | |
| p4_small_quotable | Tiny summary, big quote risk | Slips | Message #9 ("Store credit never showed; ticket said Refunds owns it.") needs the customer line quoted. |
| p5_hidden_library | Hidden library lookup | Slips | |
| p6_goldfish | Forgets prior context | Caught | |
| p7_your_own | It reviews each ticket for completeness before routing. | Hold | |

**Slips count**: 3

**Go-live rule**: Ship stops when Slips hit your count. No soft warnings, no owners. Threshold = 2. Current Slips = 3. **Ship blocked.**

**Defenses to flip Slips**:

| Slips task | Defense | Current state |
|------------|---------|---------------|
| p1_bundle | split_bundles | off |
| p4_small_quotable | name_source | on |
| p5_hidden_library | name_source | on |

**Re-run trigger**: Re-run after prompt, model, or tool change — plus a monthly floor.

---

## Output shape

The analyzer returns:

```
{
  "board": [
    { "task_id": "p1_bundle", "verdict": "Slips", "defense": "split_bundles" },
    { "task_id": "p2_messy_harmless", "verdict": "Caught", "defense": null },
    { "task_id": "p3_mind_reader", "verdict": "Hold", "defense": "rewrite_mind_read" },
    { "task_id": "p4_small_quotable", "verdict": "Slips", "defense": "name_source" },
    { "task_id": "p5_hidden_library", "verdict": "Slips", "defense": "name_source" },
    { "task_id": "p6_goldfish", "verdict": "Caught", "defense": null },
    { "task_id": "p7_your_own", "verdict": "Hold", "defense": null }
  ],
  "slips_count": 3,
  "slips_to_block": 2,
  "go_live": false,
  "rerun_trigger": "Re-run after prompt, model, or tool change — plus a monthly floor."
}
```
