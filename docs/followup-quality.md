# Follow-up Quality: How to Judge the Probe Itself

Methodology only. No judge prompts, no production wording, no customer transcripts.

## Where this sits

| Layer | Question | This repo |
|-------|----------|-----------|
| **Route** | Which action class should fire? | [maps/](../maps/), [principles.md](./principles.md) |
| **Probe quality** | Is *this* interviewer utterance a good probe? | **This page** |
| **Stop** | Should there be another turn at all? | [stop-conditions.md](./stop-conditions.md) |

Maps answer **which edge**. This page answers **whether the spoken probe is acceptable** once a generative probe is allowed.

Label fields that absorb failures: [label-schema.md](./label-schema.md) (`fail_grounding`, `fail_safety`, `fail_surface`, …).  
How findings feed fixes: [eval-loop.md](./eval-loop.md).

## Who uses this method

**Primary consumer: an LLM judge**, not a full-time human rater.

Daily product review does **not** need every turn scored on a long rubric. The comparative method exists so that:

- two outputs can be judged on a **shared input** without one side drifting “strict” or “lenient”;
- prompt or model changes can be decided with a **stable gate order**.

Humans usually only spot-check:

- **large disagreements** between sides (or between judge and expectation);
- **ultra-low** scores / fatal flags.

That is sampling for calibration—not “human scores every line.”

### Generator vs judge (different model jobs)

| Role | Constraint | Typical choice |
|------|------------|----------------|
| **Dialogue generator** (live follow-up) | Low latency, high concurrency; must stay under TTS turn budget | Fast path; **not** deep-thinking / long CoT modes |
| **Quality judge** (offline or shadow compare) | Reasoning over stem + answer + two probes; scale stability matters more than ms | Stronger **deep-thinking** / high-reasoning model |

Do not reuse the live generator’s “non-thinking” config as the judge. Speed and judgment are separate product knobs.

## What is compared

Typical unit (illustrative product practice):

- **Task:** first follow-up after the candidate’s answer to the stem (not full multi-turn strategy).
- **Unit of comparison:** a deployable **path** — model config × prompt package — not “raw model IQ.”
- **Same method for prompt iteration:** hold the model fixed; generate with **old vs new** prompt; compare the two probes on identical `(stem, answer, type)` inputs.

Languages / vendors may differ; the **gate shape** should not.

## Impact first: fatal vs minor flaw

Before fine scores, classify harm to **candidate experience** and **evidence goals**:

| Level | Meaning | Effect on judgment |
|-------|---------|-------------------|
| **Fatal** | Hallucination / severe misread, wrong language for TTS, hostile tone, severe off-type probe, unusable output | Side cannot win; maps to hard `fail_*` |
| **Clear flaw** | Wrong strategy for the state, stacked multi-asks, harsh pivot | Heavy penalty on the relevant dimension |
| **Minor flaw** | Slightly long, stock opener, mild wording roughness—but answerable | Light or no penalty; do **not** lose on “less pretty” |

Do not let style aesthetics overturn a probe that still gains the right next evidence.

## Gate order (shape)

### Hard fails (fatal)

Any fatal hit fails the turn. Illustrative classes (names may vary privately):

| Class | Rough meaning |
|-------|----------------|
| Hallucination / severe distortion | Invents facts or ignores clear candidate content |
| Language / unusable surface | Wrong language for the locale; empty, truncated, garbage TTS |
| Severe off-type | e.g. situational stem forced into “tell a past story” against the type contract |
| Hostile | Mockery, pressure, improper judgment |

**Release mindset (when comparing path A vs path B):** B’s fatal rate should not exceed A’s; B should not introduce a fatal **mode** that A almost never shows.

Exact rates and tag taxonomies stay in private runbooks.

### Quality scores (when no fatal)

When no fatal applies, score a small fixed set (illustrative 0–100 each):

1. **Strategic efficacy** — Does the probe pull the next useful layer of evidence for the competency / type?
2. **Understanding** — Does it respect what the candidate meant? Anchors, no false “you said.”
3. **Candidate experience** — Clear, professional, speakable; **prefer one open question per turn.**

Diagnose **candidate state** first (e.g. normal / hypothetical-evasive / misaligned / incomplete / empty), then apply type-aware strategic preferences (behavioral deepen STAR gaps; situational deepen judgment under the scenario; do not treat minor flaws as losses).

Private products may keep detailed state→strategy tables; public contract only needs: **state before strategy, strategy before style.**

### Style metrics (do not decide winners)

Length, praise rate, stock openings, multi-ask rate, etc. explain differences. They are **not** a substitute for fatal + dimensional judgment.

## Comparative protocol (why “compare,” not absolute scores alone)

Absolute scores from an LLM judge drift. Prefer:

```text
1. Same stem + answer + type context
2. Score side A alone (state → dimensions → notes)
3. Reset; score side B alone the same way
4. Calibrate; set winner ∈ {A, B, tie}
   - fatal side cannot win
   - tie: both usable next-layer probes, only angle differs
```

Use the same protocol for:

- **vendor / model swap** (path A vs path B);
- **prompt update** (old vs new pack, model held fixed).

Report **tie rate**, **win rate among non-ties**, and **fatal case list**—not a single blended “niceness” score.

## Mapping to `quality_flag`

| Judgment | Typical flag |
|----------|----------------|
| Wrong lane / wrong action class | `fail_route` (usually caught by map audit, not this probe score alone) |
| Invented premise / severe misread | `fail_grounding` |
| Boundary / hostile | `fail_safety` |
| Wrong language, unusable TTS, process leak into speech | `fail_surface` |

For process text in the speakable string, see [failure-case-process-leak.md](./failure-case-process-leak.md). For **English-track** off-locale token leaks (not generalized to other target languages), see [failure-case-language-locale-leak.md](./failure-case-language-locale-leak.md).
| Pass gates; only minor flaws | `pass` |

Route bugs still dominate: a beautiful probe on the wrong edge is a map failure first.

## Out of scope on this page

- When to **stop** probing (dialogue termination) — see [stop-conditions.md](./stop-conditions.md).
- Full multi-turn router evaluation.
- Production judge prompts, score anchors, or KPI thresholds.
- Oral English **proficiency** scoring (separate assessment construct).

## One-line summary

> Judge follow-up probes with **impact-first fatals**, then a **small strategic/understanding/experience** set, via **paired comparison** so the judge’s scale does not drift—and let humans only audit big disagreements and ultra-low cases.
