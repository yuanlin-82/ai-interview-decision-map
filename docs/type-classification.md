# Type Classification: From Stem to Follow-up Pack

Methodology only. No classifier prompts, no item banks, no jurisdiction-specific legal advice.

## Where this sits

| Layer | Question | This repo |
|-------|----------|-----------|
| **Upstream design** | Which competencies → scenario intents → type keys? | [competency-to-scenario.md](./competency-to-scenario.md) |
| **Stem classification** | Given *this* interview question text, which pack owns follow-ups? | **This page** |
| **Turn routing** | After the candidate speaks, which action class? | [maps/](../maps/) |
| **Stop** | Probe budget / force-close | [stop-conditions.md](./stop-conditions.md) |

Maps assume a type key. Classification answers **how a stem gets that key** before probing starts—including a **compliance gate** that can refuse to type at all.

## Two-step contract

```text
Stem text
  → (1) Compliance screen
        → non-compliant → stop (do not categorize; do not probe)
        → compliant → (2) Single primary-intent category → type key → typed map
```

Always **one** category when compliant. Primary intent wins; do not multi-label.

### Step 1 — Compliance screen

Purpose: catch stems whose **primary, direct** ask is a hard-prohibited topic (equal-opportunity / hiring-law style bans). Illustrative buckets (products must freeze their own list with counsel):

- Marital / relationship / family plans  
- Personal beliefs (religion, party support, …)  
- Background & heritage used as a direct ask (e.g. household registration / origin as the question)  
- Health unrelated to job function  
- Socioeconomic status via parents’ occupation, etc.

**Contract nuance:** flag only when the stem **directly and unambiguously** targets a listed ban. **Legal grey areas** are treated as **compliant for this screen**—they are not auto-`illegal`. Grey handling is a separate legal/product review, not a second type key.

Non-compliant outcome: a dedicated **non-type** outcome (implementation may label it `illegal` or equivalent). **Do not** run Step 2. Follow-up rounds = **0 / 0** — see [stop-conditions.md](./stop-conditions.md).

### Step 2 — Primary-intent category → public type key

Classify by **what the stem is mainly trying to learn**, not by job title fluff.

| Public type key | Primary intent (short) |
|-----------------|------------------------|
| `behavioral` | Past experiences / behaviors as evidence of future performance |
| `situational` | Hypothetical scenario; likely actions and thinking under that frame |
| `issue-reasoning` | Open topic; thought process, stance, argumentation |
| `domain-knowledge` | Mastery of a domain or skill set |
| `career-choice` | Aspirations, plans, self-awareness about career path |
| `information-gathering` | Factual / administrative / logistical slots |
| `workplace-resilience` | Work-framed stress handling, resilience, professional well-being (non-clinical) |
| `job-transition` | Leave / job-change patterns and motivations |
| `culture-fit` | Fit with values, work style, environment |
| `opening-intro` | Describe background, skills, experience (opening narrative) |
| `fallback` | Compliant but fits none of the above |

Internal engine short names (if any) should **map into these keys** for public docs and map ownership—not the reverse.

## Output shape (conceptual)

Classifier returns a **single** field, e.g. `questionType` ∈ { non-compliant marker } ∪ { type keys above }.  
No free-text essay in the hot path. Explanations stay offline if needed for audit.

## Why this matters for follow-up

- Wrong type → wrong strategy pack (e.g. situational stem forced into past-episode dig).  
- Skipping the compliance gate → probing an unlawful stem.  
- Multi-label temptation → unstable pack choice; keep **primary intent**.

Eval of classifiers is separate from [followup-quality.md](./followup-quality.md) (probe wording). Useful checks: agreement with gold type on a stem panel; zero probes when non-compliant.

## Out of scope here

- Full legal catalogs by country  
- Production classifier prompts or few-shot banks  
- How HR authors stems (see competency sketch)  
- Per-turn abnormal routing after the candidate answers  

## One-line summary

> Screen the stem for hard compliance first; if it passes, assign **exactly one** primary-intent type key so the matching follow-up map owns the dialogue.
