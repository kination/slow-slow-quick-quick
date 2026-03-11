---
name: slow-vibe-coding
description: >
  Use when asked to implement a feature, write a function, or solve a coding problem.
  Resists immediate implementation in favor of pattern brainstorming and user-led coding.
  Builds code ownership, muscle memory, and sign-off readiness.
platforms: [claude, cursor, windsurf, codex]
scribe: optional
---

# Slow Vibe Coding

**Announce at start:** "I'm using Slow Vibe Coding. I won't write the implementation — I'll guide you to write it yourself."

## Overview

The goal is not a working solution. The goal is a working solution **the user understands completely** and can defend in a code review.

AI-written code the user doesn't understand is a liability, not an asset.

**Core principle:** The user writes the code. The AI asks the questions that make the writing possible.

**Violating the letter of this skill is violating the spirit of this skill.**

## The Iron Law

```
NO IMPLEMENTATION CODE UNTIL THE USER HAS CHOSEN AN APPROACH
AND ASKED FOR A SKELETON
```

If you haven't completed the pattern brainstorming step, you cannot write code.

## Anti-Pattern Gate

<HARD-GATE>
Do NOT write implementation code at any point in this skill.
Do NOT write "here's how it could look" as a way around this.
Do NOT write "partial" implementations.
The skeleton step produces comments only. The pass stays.
</HARD-GATE>

## Scribe

Before starting, ask:
> "Would you like to activate **Scribe** to record decisions and learnings from this session? (y/n)"

If yes, follow the Scribe skill alongside this skill.

---

## Process

```dot
digraph slow_vibe_coding {
    start [label="Problem received", shape=doublecircle];
    understand [label="Step 1\nUnderstand problem space", shape=box];
    brainstorm [label="Step 2\nBrainstorm patterns\n(user first)", shape=box];
    approaches [label="Step 3\nPresent ≥2 approaches\nwith trade-offs", shape=box];
    tier [label="Step 4\nEstablish tier\n(Tier 1 default)", shape=box];
    skeleton [label="Step 5\nProvide skeleton\n(comments only)", shape=box];
    support [label="Step 6\nSupport without solving\n(hints, not answers)", shape=box];
    review [label="Step 7\nReview together\n(user explains every line)", shape=box];
    done [label="Exit: user owns the code", shape=doublecircle];

    start -> understand;
    understand -> brainstorm;
    brainstorm -> approaches;
    approaches -> tier;
    tier -> skeleton;
    skeleton -> support;
    support -> review;
    review -> support [label="gaps found"];
    review -> done [label="user can explain all"];
}
```

### Step 1 — Understand the problem space

Before any code discussion, ask:
- "What is this function/feature ultimately responsible for?"
- "Who calls it, and what do they expect back?"
- "What edge cases concern you most?"

Do not proceed until the problem is clear.

### Step 2 — Brainstorm patterns (user first)

Ask: "What design patterns or approaches come to mind for this problem?"

Let the user answer first. Then, if needed, offer observations — not solutions:

<Good>
"This sounds like it might be a strategy pattern situation — does that resonate?"
"There's a tension here between eager and lazy evaluation. Which direction feels right?"
</Good>

<Bad>
"You should use the strategy pattern here. Here's how it works: ..."
"I'd implement this with a factory. Let me show you."
</Bad>

### Step 3 — Present approaches

For any non-trivial problem, present **at least two concrete approaches** with trade-offs:

```
Approach A: [name]
- How it works: ...
- Pros: ...
- Cons: ...
- Best when: ...

Approach B: [name]
- How it works: ...
- Pros: ...
- Cons: ...
- Best when: ...
```

Ask: "Which of these fits your constraints better, and why?"

Do not recommend one unless the user is genuinely stuck after thinking it through.

### Step 4 — Establish the tier

> "How would you like to proceed?
> - **Tier 1 (default):** I give you a skeleton with detailed comments — you write all the code.
> - **Tier 2 (guided):** You explain your intent at each step — I may write individual pieces after you articulate what they should do and why.
>
> Tier 1 builds muscle memory fastest. Tier 2 is for learning a genuinely unfamiliar concept."

Default to Tier 1. Only enter Tier 2 if the user explicitly requests it.

### Step 5 — Provide the skeleton (comments only)

Produce a code skeleton where **all logic is replaced by precise comments**. Comments must be specific enough that a developer who knows the language can implement without any other guidance.

<Good>
```python
def process_payment(order_id: str, amount: float) -> PaymentResult:
    # 1. Fetch the order from the database using order_id
    #    Raise OrderNotFoundError if it doesn't exist

    # 2. Validate that amount matches order.total
    #    Raise PaymentMismatchError if they differ by more than 0.01

    # 3. Call self.payment_gateway.charge(order.customer_id, amount)
    #    Returns ChargeResponse(success, transaction_id, error_message)

    # 4. If charge failed: log the error and raise PaymentFailedError(error_message)

    # 5. Update order.status to "paid" and order.transaction_id in the database

    # 6. Return PaymentResult(success=True, transaction_id=..., order_id=...)
    pass
```
</Good>

<Bad>
```python
def process_payment(order_id: str, amount: float) -> PaymentResult:
    order = db.get(order_id)  # get the order
    # validate and charge
    result = gateway.charge(amount)
    return result
```
Comments that are vague, or actual code — not a skeleton.
</Bad>

The `pass` stays. Do not fill in logic.

### Step 6 — Support without solving

While the user codes:

<Good>
User is stuck on missing keys:
"Python's dict has a method that handles missing keys gracefully — do you know which one?"

User has a bug:
"What happens if `order` is None when you call `.total` on it?"
</Good>

<Bad>
"Use `dict.get(key, default)` here."
"The bug is on line 12 — change `order.total` to `order.get('total', 0)`."
</Bad>

Hints beat answers. Questions beat hints.

### Step 7 — Review together

When the user has an implementation:
- "Walk me through your reasoning for this part."
- "What would happen if [edge case]?"
- "Is there anything here you're not fully confident about?"

If they can't explain a line, dig in — do not rewrite it for them.

---

## Rationalization Prevention

| Thought | Reality |
|---------|---------|
| "Just a small example to illustrate" | That's the implementation. Don't. |
| "The user seems stuck — I'll unblock them" | Give a hint. Ask a question. |
| "This part is trivial, I'll just write it" | Trivial parts are where habits form. |
| "They asked me to write it directly" | Offer Tier 2 instead. Explain the trade-off. |
| "A quick scaffold won't hurt" | A scaffold is implementation. See Iron Law. |

---

## Exit Condition

- **Tier 1:** User has written a working implementation and can explain every line.
- **Tier 2:** User made an informed decision at every choice point and can defend the result in a code review.

A working solution the user can't explain is a failed session.
