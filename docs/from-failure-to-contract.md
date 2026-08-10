# From failure case to contract change (one path)

A **composite** story for visitors. Not a real ticket dump; not production prompts.  
Shows how field failures feed the **decision contract**—the loop in [eval-loop.md](./eval-loop.md).

---

## Role in this work

Content / assessment design on the dialogue side: own the **routing contract**, failure taxonomy, and probe-quality gates. Engineering owns models, serving, and wire assembly; this path is about **what should fire**, not which GPU ran it.

---

## The symptom (what ops / QA heard)

Late in an item, the candidate asks “what was the question again?”  
The agent **restates the original stem**—but the last interviewer turn was already a **judgment probe**. The candidate answers the stem again; the probe’s evidence gap is lost. Dialogue looks “polite” and still **mis-routes**.

Related field note: [failure-case-reanchor.md](./failure-case-reanchor.md).

---

## Which layer failed

| Layer | Question | This case |
| --- | --- | --- |
| Route | Which action class? | Re-anchor fired (right family) but **target** was wrong |
| Probe wording | Is the sentence a good ask? | Secondary—stem restatement can be fluent and still wrong |
| Stop | Should we ask at all? | Not the issue |

Gate order: fix **route / re-anchor target** before polishing prose. See [followup-quality.md](./followup-quality.md) (routes before surface).

---

## Contract change (shape)

1. **Principle:** When re-anchoring to “the question,” restate the **last interviewer ask in this dialogue**, not necessarily the bank stem—unless the stem *was* the last ask. ([principles.md](./principles.md), [maps/overview.md](../maps/overview.md))  
2. **Label / audit:** Wrong re-anchor target → route fail (`fail_route` / re-anchor family), not “awkward wording.” ([label-schema.md](./label-schema.md))  
3. **Eval:** Sample “forgot the question” turns; score whether restatement matches **last ask**; patch map/prompt only after the target rule is explicit. ([eval-loop.md](./eval-loop.md))  
4. **Walkthrough check:** [walkthroughs.md](../examples/walkthroughs.md) Walkthrough 1 encodes the same path.

Private implementations change wording; the **public durable asset** is the target rule above.

---

## What this path does *not* claim

- Exact before/after win rates or KPI tables  
- That one field note fixed all products  
- Cross-item resume memory (different gap—[failure-case-cross-item-resume-repeat.md](./failure-case-cross-item-resume-repeat.md))

---

## One-line takeaway

> A good-sounding restatement of the **wrong** question is still a routing failure—and the fix lives in the **contract**, then the eval loop, not in prettier filler.
