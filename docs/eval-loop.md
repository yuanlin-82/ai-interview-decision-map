# Eval Loop: From Bad Turns Back to the Contract

Methodology only. No proprietary dashboards, no raw logs.

## Goal

Keep the decision contract honest:

> Sample live (or shadow) dialogues → judge **route then surface** → cluster failures → change **map / labels / prompts** in that order.

Prompt polish without contract change is optional. Wrong edges are not.

## Minimal loop

```text
1. Sample turns (stratify by type + lane: abnormal vs normal)
2. Dual label: state + action_class (+ quality_flag)
3. Compare: human vs model (or policy A vs B)
4. Cluster fails: route / grounding / safety / surface
5. Patch:
     - route fails     → map / principles
     - grounding fails → label rules + prompt constraints
     - safety fails    → boundary pack
     - surface-only    → wording layer (private)
6. Re-sample the same strata; don’t only celebrate overall “win rate”
```

## What to measure (lightweight)

| Check | Question | Typical use |
|-------|----------|-------------|
| **Route agreement** | Same action class as gold map? | Primary gate |
| **Grounding rate** | Invented premises / false “you said”? | Hard fail |
| **One-ask rate** | Single open question? | Product constraint |
| **Re-anchor target** | Restate last ask vs wrong stem? | Known failure class |
| **Human–model agreement** | Spot panel on score_dimensions | Release bar |

Exact thresholds are product-specific. Publish the **shape**, not internal KPIs.

## Severity order

1. Safety / boundary violation  
2. Wrong lane or wrong action class  
3. Ungrounded probe  
4. Surface awkwardness (TTS, register)

Do not average these into one “nice score.”

## go / no-go mindset (example)

Useful release questions (illustrative):

- **go:** Route agreement on abnormal lane holds on a held-out sample; no new safety miss class.  
- **no-go:** Re-anchor systematically returns to stem on late turns; or grounding fails spike after a prompt change.

Numbers belong in private runbooks; the public point is **gate order**.

## Tie-back to this repo

- Failure notes already in `docs/failure-case-*.md` are inputs to step 5.  
- Label fields: [label-schema.md](./label-schema.md)  
- Principles for what “wrong” means: [principles.md](./principles.md)

## Out of scope here

Production prompt diffs, customer transcripts, and vendor-specific judge prompts stay private.
