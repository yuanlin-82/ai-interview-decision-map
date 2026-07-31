# Label Schema (Conceptual): What “Good / Bad” Means

Methodology only. No production prompts, no customer copy, no PII.

## Why this note exists

Routing maps answer **which action class** to fire.  
Products also need a **label contract**: what evidence counts, what score dimensions mean, and what a bad turn looks like—so training samples and human review stay consistent.

This page is a **schema sketch** for interview-style multi-turn agents. The same shape can be reused for expert-dialogue agents (swap competency names; keep fields).

## Sample record shape

One labeled interviewer turn (or one candidate–interviewer pair) can look like:

| Field | Type | Meaning |
|-------|------|---------|
| `type` | enum | Question-type key (`behavioral`, `situational`, …) |
| `state` | enum / struct | Latest-turn judgment (e.g. empty, off-topic, substantive-with-anchor) |
| `action_class` | enum | What the interviewer should do (map node), not the sentence |
| `score_dimensions` | list | Rubric axes used for this type (see below) |
| `evidence_span` | text span ids | Which candidate words justified the state / score (or “none”) |
| `quality_flag` | enum | `pass` \| `fail_route` \| `fail_grounding` \| `fail_safety` \| `fail_surface` |
| `notes` | short text | Reviewer rationale (internal; not for model training as free prose if avoidable) |

**Rule:** labels name **decisions and evidence**, not “sounds warm.”

## Score dimensions (illustrative)

Use a small fixed set per type. Example for `behavioral`:

| Dimension | Pass look | Fail look |
|-----------|-----------|-----------|
| **Route** | Action class matches map for this state | Beautiful sentence on the wrong edge |
| **Grounding** | Probe anchored in candidate meaning | Invented premise / stem treated as said |
| **Evidence type** | Past episode when type requires it | Hypothetical “I would…” treated as past |
| **One-ask** | Single open question | Multi-part stack |
| **Safety** | Boundary pack respected | Digs into forbidden complaint / pathology |

Expert-companion products can replace “Evidence type” with “Professional boundary” or “Crisis handoff,” but still keep **Route** and **Grounding**.

## “Good reply” vs “Bad reply” (operational)

Do **not** start with adjectives (“empathetic,” “professional”). Start with gates:

1. **Lane:** abnormal vs normal correctly classified?  
2. **Action class:** matches the decision map?  
3. **Grounding:** no invented candidate facts?  
4. **Only then** surface: length, register, TTS readability.

A warm sentence that invents feelings from ASR garbage is a **fail**, not a style issue.

## Probe-utterance quality (paired judge)

When the question is not “which map edge?” but “is this spoken probe good enough?”, use a **comparative** contract: same candidate input, score path A and path B separately, then pick winner / tie—so an LLM judge does not drift strict/lenient across runs. Fatals first; then a small strategic / understanding / experience set. Humans typically only audit large disagreements and ultra-low scores.

Shape: [followup-quality.md](./followup-quality.md). This does **not** replace route labels above.

## What this enables

- Shared vocabulary for route vs surface fails  
- Spot-checks of model outputs against the same axes  
- Preference / release compares that are a **contract**, not essays  

Spoken realizations and production prompts stay private.
