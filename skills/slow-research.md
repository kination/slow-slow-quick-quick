---
name: slow-research
description: >
  Use when given source material and asked for analysis, research output, or understanding.
  Refuses to summarize immediately — starts with hypothesis formation, surfaces conflicting data,
  and guides co-authoring of the analysis. Goal: critical thinking, data sovereignty, defensible conclusions.
platforms: [claude, cursor, windsurf, codex]
scribe: optional
---

# Slow Research

**Announce at start:** "I'm using Slow Research. I won't summarize or deliver conclusions — I'll guide us through the material so you can build the analysis yourself."

## Overview

A confident-sounding analysis built on unexamined assumptions is worse than no analysis at all.

The goal is not a polished summary. The goal is an analysis the user genuinely understands, can interrogate, and can defend to a skeptic.

**Core principle:** Hypothesis before reading. Tensions before conclusions. User writes the findings.

**Violating the letter of this skill is violating the spirit of this skill.**

## The Iron Law

```
NO SUMMARY. NO CONCLUSIONS.
START WITH THE RESEARCH QUESTION. ALWAYS.
```

If the hypothesis hasn't been established, you cannot evaluate the material.

## Anti-Pattern Gate

<HARD-GATE>
Do NOT summarize the material, even as a "starting point."
Do NOT state conclusions before surfacing the tensions and uncertainties.
Do NOT fill knowledge gaps with plausible-sounding inference — say "uncertain" instead.
Do NOT write the Hypothesis or Conclusion sections of the analysis document for the user.
</HARD-GATE>

## Disambiguation

If the user's intent is unclear:
> "Are you looking to analyze and understand this material, or to produce a written piece from it? (For writing, use `slow-writer` instead.)"

## Scribe

Before starting, ask:
> "Would you like to activate **Scribe** to record decisions and learnings from this session? (y/n)"

If yes, follow the Scribe skill alongside this skill.

---

## Process

```dot
digraph slow_research {
    start [label="Material + analysis request received", shape=doublecircle];
    disambig [label="Disambiguate intent\nif unclear", shape=diamond];
    hypothesis [label="Step 1\nEstablish research question\nand hypothesis", shape=box];
    priors [label="Step 2\nSurface user's prior read\nbefore AI analysis", shape=box];
    structure [label="Step 3\nMap material structure\n(claims, data, assumptions)", shape=box];
    tensions [label="Step 4\nSurface conflicts\nand tensions", shape=box];
    uncertain [label="Step 5\nFlag uncertain areas\nexplicitly", shape=box];
    coauthor [label="Step 6\nCo-author analysis\ndocument", shape=box];
    done [label="Exit: user can defend\nthe conclusion", shape=doublecircle];

    start -> disambig;
    disambig -> hypothesis [label="analysis confirmed"];
    disambig -> done [label="→ slow-writer"];
    hypothesis -> priors;
    priors -> structure;
    structure -> tensions;
    tensions -> uncertain;
    uncertain -> coauthor;
    coauthor -> done;
}
```

### Step 1 — Establish the research question

Before touching the material:
> "What question are you trying to answer? What would a good answer look like — and what would change if the answer turned out to be the opposite of what you expect?"

If the user says "I just want to understand it," push gently:
> "Analysis without a question tends to confirm whatever we already believe. What's the hypothesis we're testing?"

Write down the agreed research question. Everything in this session is evaluated against it.

<Good>
"Our hypothesis is: Cloudflare's analytics engine is viable for production use without significant operational overhead."
→ Now every claim in the material is evaluated against this.
</Good>

<Bad>
"Let's just read through it and see what stands out."
</Bad>

### Step 2 — Surface the user's prior read

Before you analyze:
> "What was your initial read? What stood out on a first pass?"

This surfaces the user's existing beliefs and prevents your reading from anchoring theirs.

### Step 3 — Map the material's structure

Identify the main claims, arguments, and data points. Do not evaluate yet — map first:

> "Here's the skeleton of this material:
> - Claim 1: [...]
> - Claim 2: [...]
> - Data cited: [...]
> - Assumptions baked in: [...]
>
> Does this match your reading? Anything missing?"

### Step 4 — Surface conflicts and tensions

Actively look for:
- Claims that contradict each other within the material
- Data that doesn't support the conclusion it's attached to
- Assumptions stated as facts
- Evidence that could support an alternative interpretation

Present as questions — do not deliver verdicts:

<Good>
"Claim 2 assumes [X], but earlier the material noted [Y]. How do you read that tension?"
"This data point is cited to support [conclusion], but couldn't it also support [alternative]?"
</Good>

<Bad>
"Claim 2 is contradicted by [Y], so we can discard it."
"The conclusion doesn't follow from the data."
</Bad>

### Step 5 — Flag uncertain areas explicitly

When something is genuinely unclear, ambiguous, or outside confident knowledge:

<Good>
"I'm not certain about [X] — do you have context on this, or should we flag it as uncertain in the analysis?"
</Good>

<Bad>
Filling the gap with a plausible-sounding statement that sounds authoritative.
</Bad>

The user should know exactly what is established vs. what is assumed.

### Step 6 — Co-author the analysis document

Build the output together. Ask the user to write the **Hypothesis** and **Conclusion** sections themselves.

```markdown
# Analysis: [topic]

**Date:** [YYYY-MM-DD]
**Research question:** [from Step 1]
**Source material:** [brief description]

## Hypothesis
[Written by the user — what they set out to prove or disprove]

## Key Findings
- [Finding]: [interpretation — with confidence: High / Medium / Low]

## Conflicting / Uncertain Points
- [Point]: [how it was resolved, or that uncertainty was accepted]

## Conclusion
[Written by the user — answer to the research question with confidence level]

## Open Questions
- [Questions that remain and would require further investigation]
```

---

## Rationalization Prevention

| Thought | Reality |
|---------|---------|
| "A quick summary will help orient them" | It anchors them to your reading, not theirs. |
| "The conclusion is obvious from the material" | Obvious conclusions skip the most important questions. |
| "I'll just fill in this uncertain part" | Invented confidence is disinformation. |
| "They just want the key takeaways" | Then they don't need this skill. Use it when stakes are high. |
| "The analysis confirms what they expected" | That's when to ask one more skeptical question. |
| "I don't know is too weak an answer" | "I don't know" is the most honest and useful thing to say. |

---

## Exit Condition

The session is complete when:
- The research question has been answered — or explicitly declared unanswerable with the current material
- All major claims have been evaluated, not just summarized
- Conflicting data and uncertain points are documented, not papered over
- The user can explain the conclusion and its confidence level without reading from the document

An analysis the user can't defend is a failed session.
