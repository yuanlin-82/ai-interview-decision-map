# Stop Conditions: When to End Probing (and When to Force the Item)

Methodology only. No production stop prompts, no deploy configs, no customer transcripts.

## Where this sits

| Layer | Question | This repo |
|-------|----------|-----------|
| **Route** | Which action class should fire *this* turn? | [maps/](../maps/) |
| **Probe quality** | Is this utterance a good probe? | [followup-quality.md](./followup-quality.md) |
| **Stop** | Should another probe be generated — or must the item end? | **This page** |

Routing decides *what* to ask next. Stop decides *whether* asking next is still allowed.

## Two layers

Products typically separate **stopping follow-ups** from **force-closing the item**.

| Kind | Effect | Typical trigger |
|------|--------|-----------------|
| **Force-close item** | End the **current item** → next item / next stage | Wall-clock answer time exceeds **item force-close time** |
| **Stop follow-ups** | **Do not generate** any new follow-up on this item | Checked when the candidate advances (e.g. “next”); see conditions below |

Force-close is a **session timer** decision. Stopping follow-ups is a **probe budget + dialogue policy** decision. They stack; force-close always wins if it fires.

### Time anchors (relative to configured max)

Let \(T_{\max}\) = backend **maximum allowed answer time** for the item (also the limit typically **shown to the candidate**).

Offsets are **fixed** (±1 minute). They do **not** vary by job posting or candidate:

| Clock | Definition |
|-------|------------|
| Follow-up time ceiling | \(T_{\max} - 1\) minute |
| Item force-close time | \(T_{\max} + 1\) minute |

Example if \(T_{\max} = 4\) minutes: follow-up ceiling ≈ 3 min, force-close ≈ 5 min.  
A separate **average** answer time may be configured for **recruiter / admin estimates**; it is not part of these two clocks.

**ASR near the visible max (illustrative product rule):** in a short window just before \(T_{\max}\), if ASR already has text, the system may **extend** allowed speaking time toward the force-close time—so a mid-sentence cut is less likely. Exact window is an implementation detail.

**Re-entry:** leaving and re-entering the answer flow may restart the item clock from 0s (product rule).

## When to stop follow-ups

**Stop follow-ups** means: **no new follow-up generation**. Evaluate when the candidate tries to advance, if **any** of the following holds:

1. **Time:** answer elapsed time \(>\) follow-up time ceiling (\(T_{\max}-1\) min).  
2. **Rounds:** dialogue probe count \(>\) **max follow-up rounds** for this type (see table).  
3. **Stop decision:** at least **one** follow-up has already been asked on this item, **and** the stop classifier outputs `stop`.

Conditions are **OR**. Time/round caps can stop follow-ups even if the classifier would say `continue`.

### Default vs max rounds (illustrative)

Products often configure both a **default** probe budget and a **maximum** (the round cap used here). Figures below are an **illustrative** matrix aligned to this repo’s type keys—not a promise of any live tenant’s numbers.

| Type key | Default rounds (illustrative) | Max follow-up rounds (illustrative) |
|----------|-------------------------------|-------------------------------------|
| `behavioral` | 2 | 5 |
| `situational` | 1 | 4 |
| `issue-reasoning` | 2 | 5 |
| `domain-knowledge` | 2 | 6 |
| `career-choice` | 2 | 5 |
| `information-gathering` | 1 | 2 |
| `workplace-resilience` | 1 | 3 |
| `job-transition` | 2 | 4 |
| `culture-fit` | 2 | 4 |
| `opening-intro` | 1 | 3 |
| `fallback` | 1 | 3 |

**Compliance / illegal custom stems** (clearly unlawful hiring questions under equal-opportunity rules—primary direct asks on banned topics): **0 / 0** — do not probe. Classification contract: [type-classification.md](./type-classification.md). Grey-zone items are **not** automatically this bucket; handle under product legal review, not by inventing a public type key.

## Stop classifier (model branch)

A dedicated classifier decides `stop` vs `continue` for condition 3. It is **not** the live follow-up generator:

| Role | Job |
|------|-----|
| Follow-up **generator** | Fast, high concurrency; typically **non**-deep-thinking |
| **Stop classifier** | Binary policy over the transcript; may use a stronger / deep-thinking model |
| Quality **judge** (offline compare) | Separate again — see [followup-quality.md](./followup-quality.md) |

Environments may keep **different** stop-prompt packages (e.g. test/pre-prod vs production). Publish the **decision shape** only; keep wording private.

### Output contract

- Binary only: `stop` | `continue`.  
- No middle label.  
- Prefer **benefit of the doubt**: if a stop condition is not clearly met (including exclusions / consecutive-count rules), treat it as not met and keep evaluating; if none clearly fire → `continue`.

### Stop if and only if

Either **inappropriate to continue** (candidate state / behavior) **or** **unnecessary to continue** (no further valid yield).

#### Inappropriate to continue (experience / effectiveness)

Illustrative classes (private prompts name signals and exclusions in full):

| Class | Idea | Needs consecutive hits? |
|-------|------|-------------------------|
| **Disruption** | Deliberate derail / mock / meaningless spam | Often yes, within this item’s probe sequence |
| **Clear resistance** | Explicit refuse / “I already said that” / “are we done?” | Exclude connective “as I said” and clarify-then-answer |
| **Passive non-engagement** | Repeated ultra-short closed answers to open probes | Often **≥2 consecutive** minimal turns |
| **Voluntary withdrawal** | “Nothing more to add” / clear closure of thought | Fillers only count if they **end** the turn with no substance |

#### Unnecessary to continue (information yield)

| Class | Idea | Notes |
|-------|------|-------|
| **Repetition without added value** | Same core claim; no new verifiable detail, steps, data, or angle | Paraphrase ≠ new information |
| **Hollow at capability limit** | Vague, non-evaluable content, **not** strategic avoidance the map still wants to dig (wrong evidence type, low-intensity example, “we”-only contribution) | Often requires **≥2** hollow turns on this item after exclusions |

ASR caveat (contract-level): unclear speech that could be recognition error should be interpreted so the candidate can still show role-relevant ability; “sufficient” is judged against the **competency under assessment**, with light job context available to the classifier.

**Do not paste** production stop prompts into this repo. Implementations must keep criteria auditable without publishing the full instruction text.

## Tie-back to maps

- Stopping follow-ups does **not** replace typed strategy packs; it **caps** them.  
- Type maps that say “stop digging when no episode can be elicited” are **related map rules** (yield / withdrawal)—not a second timer.  
- Safety boundaries on maps (e.g. `job-transition` grievance deflection) remain **route** rules; the stop classifier is an additional **global** check after probing has begun.

## Out of scope here

- Exact UI after follow-ups are stopped (closing line vs silent advance).  
- Precise definition of how “rounds” are incremented in telemetry (products must freeze one definition in private runbooks).  
- Production stop-prompt text or vendor model IDs.  
- Oral proficiency scoring.

## One-line summary

> **Force-close** ends the item on \(T_{\max}+1\) min; **stop follow-ups** blocks new probes on time (\(T_{\max}-1\)), type **max rounds**, or a binary stop classifier (inappropriate **or** unnecessary)—with maps still owning *what* to ask while probing is allowed.
