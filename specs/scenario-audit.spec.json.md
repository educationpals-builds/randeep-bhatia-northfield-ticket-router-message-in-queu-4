{
  "$schema": "https://educationpals.ai/schemas/trick-task-board/v1",
  "spec_name": "Northfield ticket router — message in, queue out",
  "spec_version": "1.0.0",
  "description": "Machine spec for the seven-row Trick-task board auditing the Northfield ticket router",
  "specimen": {
    "name": "Northfield ticket router — message in, queue out",
    "standard_line": "A two-problem message opens two tickets.",
    "source": "Last week's live queue export (10 messages)."
  },
  "vocabulary": {
    "marks": ["Caught", "Slips", "Hold"],
    "mark_definitions": {
      "Caught": "The bot handles this task correctly; no intervention needed.",
      "Slips": "The bot fails this task; a defense could flip it.",
      "Hold": "Cannot determine pass/fail from current evidence; blocked pending more data."
    }
  },
  "tasks": [
    {
      "id": "p1_bundle",
      "label": "Two problems, one ticket",
      "description": "Does the router split a message with two distinct problems into two tickets?",
      "verdict": "Slips",
      "sample_trigger": "Where's my order? Also the promo code never applied.",
      "defense_that_flips": "split_bundles"
    },
    {
      "id": "p2_messy_harmless",
      "label": "Messy but harmless",
      "description": "Does the router handle messy formatting without breaking routing?",
      "verdict": "Caught",
      "sample_trigger": "It broke again after you fixed it yesterday.",
      "defense_that_flips": null
    },
    {
      "id": "p3_mind_reader",
      "label": "Sense the real intent",
      "description": "Does the router infer intent without explicit labels or queue IDs?",
      "verdict": "Hold",
      "sample_trigger": "Can someone escalate? I've been in Billing for three days.",
      "defense_that_flips": "rewrite_mind_read"
    },
    {
      "id": "p4_small_quotable",
      "label": "Tiny summary, big quote risk",
      "description": "Does the router quote the customer line or leave the summary blank when the message is too short?",
      "verdict": "Slips",
      "sample_trigger": "Store credit never showed; ticket said Refunds owns it.",
      "defense_that_flips": "name_source"
    },
    {
      "id": "p5_hidden_library",
      "label": "Hidden library dependency",
      "description": "Does the router rely on unstated knowledge to route correctly?",
      "verdict": "Slips",
      "sample_trigger": "Password reset loop — agent told me to email support@.",
      "defense_that_flips": "name_source"
    },
    {
      "id": "p6_goldfish",
      "label": "Goldfish memory",
      "description": "Does the router lose context from earlier in the same message?",
      "verdict": "Caught",
      "sample_trigger": "Billing charged twice; chat said shipping had the tracking.",
      "defense_that_flips": null
    },
    {
      "id": "p7_your_own",
      "label": "It reviews each ticket for completeness before routing.",
      "description": "Custom task: It reviews each ticket for completeness before routing.",
      "verdict": "Hold",
      "sample_trigger": "Damaged box on delivery; I need a replacement and a pickup.",
      "defense_that_flips": null
    }
  ],
  "defenses": {
    "available": [
      {
        "id": "split_bundles",
        "label": "Force a split when there are two jobs",
        "explain": "Catches: Two problems, one ticket — sample #3 must open two tickets before this router ships.",
        "status": "off"
      },
      {
        "id": "rewrite_mind_read",
        "label": "Ban mind-reading verbs",
        "explain": "Catches: Sense the real intent — no queue without five labels (or a queue id) from the message.",
        "status": "off"
      },
      {
        "id": "name_source",
        "label": "Require a quoted source line",
        "explain": "Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank.",
        "status": "on"
      }
    ],
    "active": ["name_source"]
  },
  "go_live_controls": {
    "gate_sentence": "Ship stops when Slips hit your count. No soft warnings, no owners.",
    "slips_to_block": 2,
    "rerun_trigger": "Re-run after prompt, model, or tool change — plus a monthly floor."
  },
  "sample_messages": [
    "Refund for wrong size — not a shipping question.",
    "It broke again after you fixed it yesterday.",
    "Where's my order? Also the promo code never applied.",
    "Cancel the subscription but keep the open return.",
    "Billing charged twice; chat said shipping had the tracking.",
    "Password reset loop — agent told me to email support@.",
    "Damaged box on delivery; I need a replacement and a pickup.",
    "Can someone escalate? I've been in Billing for three days.",
    "Store credit never showed; ticket said Refunds owns it.",
    "App crash on checkout — same as last week's incident thread."
  ]
}
