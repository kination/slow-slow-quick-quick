---
name: scribe
description: >
  Activates a session recording mode that logs decisions, user intent, and learnings as a running timeline.
  Can be triggered at the start of any slow-series skill (optional prompt) or invoked independently at any time.
  Produces a structured Markdown chronicle at session end for code reviews, handovers, and retrospectives.
platforms: [claude, cursor, windsurf, codex]
scribe: disabled
---

# Scribe

**Announce at start:** "Scribe is active. I'll log decisions, intent signals, and learnings as we go. At the end, I'll produce a chronicle you can save."

## Overview

Code and documents answer "what." Scribe answers "why."

Why did we choose this over that? What were we worried about? What did we learn halfway through that changed our direction? These questions come up in code reviews, post-mortems, onboarding, and handovers — and they usually go unanswered because no one recorded the context while it was live.

**Core principle:** Observe without interrupting. Record faithfully. Surface at the end.

**Violating the letter of this skill is violating the spirit of this skill.**

## The Iron Law

```
RECORD ONLY WHAT ACTUALLY HAPPENED.
DO NOT EDITORIALIZE. DO NOT EVALUATE.
DO NOT MANUFACTURE ENTRIES.
```

## Anti-Pattern Gate

<HARD-GATE>
Do NOT interrupt the primary skill flow to record entries.
Do NOT evaluate whether decisions were good or bad — record what was decided and why.
Do NOT add recommendations or opinions inside Scribe entries.
Do NOT manufacture entries when nothing notable happened.
Do NOT write entries that are comprehensive — they must be scannable in under 5 seconds each.
</HARD-GATE>

---

## Activation

**From a slow-series skill:** Each slow skill asks at the start — "Would you like to activate Scribe? (y/n)" Answer yes.

**Independently:** Type `scribe` or `/scribe` at any point in a conversation.

---

## How It Works

Scribe is not a separate agent. It is a recording mode layered into the same conversation.

When active, **at the end of each AI response**, append a `[Scribe]` block if the turn contained a recordable event. Most turns will have zero entries — do not force it.

### Recordable events

| Type | What to capture |
|------|----------------|
| **Decision** | A choice was made between options — what was chosen and why |
| **User intent** | The user emphasized, pushed back on, or expressed concern — the signal |
| **Learning** | The user realized or articulated something they didn't know before |

### Entry format

```
---
[Scribe] Decision: Chose [X] over [Y] — [brief rationale from user's own words]
[Scribe] User intent: User emphasized [Z] as non-negotiable
[Scribe] Learning: User realized [insight]
---
```

One line per entry. Use the user's own words where possible. No evaluation, no opinion.

<Good>
[Scribe] Decision: Chose Postgres over DynamoDB — user: "we don't have the operational budget for distributed state right now"
[Scribe] User intent: User pushed back on eventual consistency — wants users to see consistent data even at latency cost
</Good>

<Bad>
[Scribe] Decision: Chose Postgres — this was a good call given the constraints
[Scribe] Summary: The session covered several important architectural topics and the user made progress
</Bad>

---

## Late Activation

If Scribe is activated mid-session:

1. Acknowledge:
   > "Scribe activated. I'll reconstruct the key decisions and intent signals from our conversation so far, then continue logging forward."

2. Produce a retrospective block from conversation history:
   ```
   [Scribe — retrospective]
   Decision: [most significant prior choice] — [reason as visible in conversation]
   User intent: [most significant prior emphasis or concern]
   ```
   Mark all retrospective entries `(r)`. Only record what was clearly visible — do not invent.

3. Continue with live entries.

---

## Producing the Chronicle

When the session ends or the user asks (e.g., "scribe output", "show chronicle", "wrap up"):

Compile all `[Scribe]` entries into the following format. Present as a fenced Markdown block for the user to copy and save.

```markdown
# Session Chronicle — [topic] — [YYYY-MM-DD]

[*Entries marked (r) were reconstructed retrospectively after Scribe was activated mid-session.*]

## Timeline

1. [Decision] Chose X over Y — reason: ...
2. [User intent] User emphasized Z as non-negotiable (r)
3. [Learning] User realized ...

## Summary

[2–3 sentences: what was accomplished, the key decision, what changed in understanding]

## Open Questions

- [Unresolved questions that surfaced during the session]
```

**Suggested filename:** `scribe-[topic]-[YYYY-MM-DD].md`

There is no automatic file write. The user saves it.

---

## Standalone Use

Scribe can be used without any slow-series skill:

> "I want to record this conversation. Scribe on."

Useful for any session where a decision trail matters: architecture discussions, requirements reviews, debugging marathons, feedback sessions, planning conversations.

---

## Rationalization Prevention

| Thought | Reality |
|---------|---------|
| "Nothing happened this turn worth recording" | Correct — skip the entry. |
| "I should note that this was a good decision" | No opinions. Record what, not whether it was good. |
| "I'll write a comprehensive entry to be thorough" | Entries must be one-line scannable. Cut it down. |
| "I'll reconstruct this from memory" | Only record what's visible in the conversation. |
| "The chronicle should tell the full story" | The chronicle is a timeline, not a narrative. |

---

## What Scribe Does Not Do

- Does not interrupt or redirect the conversation
- Does not evaluate whether decisions were good
- Does not add recommendations or opinions
- Does not write entries when nothing notable happened
- Does not persist across separate sessions — each chronicle is a standalone artifact
- Does not write files automatically — output is a copy-pasteable block
