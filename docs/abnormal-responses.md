# Abnormal Responses: Shared Recovery vs Type Overrides

Methodology only. Action classes and intents—**no** fixed spoken scripts.

## Where this sits

Normal probing assumes a usable meaning anchor. Candidate turns that are not ready for typed deepen hit **recovery** first.

| Layer | Owns |
|-------|------|
| Shared recovery | Process / content / cross-turn contradiction → [overview](../maps/overview.md) |
| Type overrides | Boundary packs that outrank shared recovery when triggered |
| Stop | Whether another probe is allowed at all → [stop-conditions.md](./stop-conditions.md) |

Abnormal handling is **not** “be warmer.” It is **constrained action classes** so recovery stays comparable across candidates.

## Why label in discrete buckets

For an LLM router, prefer **named families with clear triggers** over a soft “pick whichever action feels least harmful” continuum.

Discrete buckets are easier to match to **observable** cues (quit language, empty ASR, forgot-the-question, …). Open-ended “least harm” framing often collapses into **inferring the candidate’s hidden intent**, then acting on that story. In practice that invites invented premises and unstable lane jumps—especially when a strong reasoner is asked to narrate “what they really meant.”

**Design stance (not a proven law of models):** recover from what was **said / missing / clashing across turns**, not from a mind-read. Intent may inform private authoring; it should not become a fact asserted to the candidate (“you were actually trying to…”). Ambiguous cases still go into **one** named family with exclusions—not into free intent essays.

## Priority (conceptual)

```text
1. Type-specific boundary / safety pack (if this type defines one and it fires)
2. Shared abnormal family (process → content → contradiction), each mapped to a fixed action class
3. Only then: typed normal strategy pack
```

If (1) or (2) fires, do **not** invent a creative deepen on the same turn.

---

## Shared abnormal families (all types)

Three families. Classify into **one**; then fire the matching action class. Private products may use fixed copy; the **family → action** contract is what stays public.

### 1. Process-related (meta / flow)

About the **interview procedure or interviewer role**, not about answering the competency ask.

| Typical triggers | Action intent |
|------------------|---------------|
| Wants to quit | Acknowledge; soft invite to continue if willing—don’t argue |
| Asks to skip / change item | Hold the current item first |
| Needs think time | Allow time; don’t probe content yet |
| Challenges interviewer / item (**asking to clarify ≠ challenge**) | Acknowledge; return to the ask |
| Tests system / breaks role | Return to the ask / role |
| Asks for answer or hint | Refuse content hints; return ownership to the candidate |
| Deliberate wrong-locale answering (locale tracks; proper nouns exempt) | Reassert locale; invite on-locale answer |

Overview bucket: much of `Boundary_Or_Fixed_Recovery`.

### 2. Content-related (this turn’s answer quality / fit)

About **what they produced for the current ask** (single-turn observable).

| Typical triggers | Action class / intent | Do not |
|------------------|----------------------|--------|
| Forgot / misheard; needs the ask back; fully off-ask with no usable thread | `Restate_Last_Interviewer_Question` | Bind re-anchor to the **stem** by default |
| Empty; fillers only; bare “I don’t know”; echo question with no answer | `Soft_Reinvite` (or soft re-ask with last ask) | Invent “when you mentioned…” |
| Incomplete / interrupted mid-sentence | Invite continuation (safe fragment only) | Jump to deep typed dig |
| Unintelligible after silent ASR repair | `Ask_Repeat_Utterance` | Treat as forgot-the-**exam**-question unless they also forgot |
| Very short but has a concrete word | One **anchored** open question (light normal)—not bare “say more” | Fabricate an episode |

**Re-anchor referent:** last interviewer ask in this dialogue. Field note: [failure-case-reanchor.md](./failure-case-reanchor.md).  
Silent ASR repair when intent is recoverable → stay out of this table ([principles](./principles.md)).

Partial “answered beside the point” usually stays in **content-related**: either re-anchor (ask lost) or one constrained pull-back / deepen from what they did say—still a **named** path, not a free essay about “least harm.”

### 3. Cross-turn contradiction (special)

**Cannot be judged from a single isolated answer.** Needs dialogue history: a clear earlier claim vs a clear later claim that hard-conflict.

| Include | Exclude |
|---------|---------|
| Hard factual / stance clash across turns | Self-correction; comparing views; describing a change of mind over time |

**Action intent:** check understanding once; don’t litigate or trap.  
If history is insufficient to see a clash, **do not** invent contradiction—fall through to content-related or normal.

This family is why multi-turn context must be available to the recovery router even when generation is single-turn shaped.

---

## Type-linked abnormalities

Shared families still apply. These **extra** packs fire on specific types and **override** curiosity.

### `job-transition`

| Trigger | Action intent | Do not |
|---------|---------------|--------|
| Negativity / bashing toward former employer, manager, colleague | Redirect to environment fit / future seeking | Dig grievance detail |
| Treats the item as accusation (“are you saying I hop too much?”) | De-escalate; restate neutral purpose | Prosecute tenure |
| Refuses reasons for leaving | Soft pass; pivot to non-forbidden angle | Force disclosure |
| Uncomfortable personal non-work leave reasons | Acknowledge; offer work-framed alternative | Excavate private life |

Map: [job-transition.md](../maps/job-transition.md) · Field note: [failure-case-job-transition-boundary.md](./failure-case-job-transition-boundary.md).

### `workplace-resilience`

| Trigger | Action intent | Do not |
|---------|---------------|--------|
| Non-work trauma / clinical material | Soft close that thread; return to work frame or pass | Diagnose; probe pathology |
| Explicit “too personal” / refusal | Soft pass; smaller work frame | Push for feelings |
| Challenges “why ask about feelings?” | Neutral purpose; keep work-stress evidence lane | Debate clinical legitimacy |

Map: [workplace-resilience.md](../maps/workplace-resilience.md) · Field note: [failure-case-workplace-resilience-frontline.md](./failure-case-workplace-resilience-frontline.md).

### `behavioral` (evidence-type)

| Trigger | Action intent | Do not |
|---------|---------------|--------|
| Ideal / hypothetical “I would…” with no past episode | `Ground_in_real_episode` | Deepen the plan as if it were past behavior |

Map: [behavioral.md](../maps/behavioral.md) · Field note: [failure-case-behavioral-evidence.md](./failure-case-behavioral-evidence.md).

### `situational`

| Trigger | Action intent | Do not |
|---------|---------------|--------|
| Pure plan / stance under a scenario stem | Stay in scenario judgment / trade-off / risk lanes as allowed | Force a past-episode dig that **breaks** the frame |
| Empty hypothesis | Soft re-anchor to the scenario | Over-apply “always ask past behavior” |

Map: [situational.md](../maps/situational.md) · Field note: [failure-case-situational-overgeneralize.md](./failure-case-situational-overgeneralize.md).

### Other types (lighter extras)

| Type | Extra boundary flavor |
|------|------------------------|
| `information-gathering` | Slot confusion → confirm format; don’t open competency dig |
| `opening-intro` | Ultra-thin intro → one concrete claim check; don’t run full behavioral ladder |
| `domain-knowledge` | No knowledge move → return to proficiency step; don’t swap into life-story dig |
| `issue-reasoning` | Demands a “correct answer” from the interviewer → refuse; stay on their reasoning |
| `fallback` | Prefer continuity recoveries; don’t invent a specialty lens |

---

## Tie to stop and quality

- Repeated empty / withdrawal may later hit **stop follow-ups**—budget, not a substitute for first-turn recovery.  
- Recovery turns still obey **one open question** and **no invented premises**.  
- Agent-side surface fails (process leak, English off-locale tokens) are not candidate abnormalities: [failure-case-process-leak.md](./failure-case-process-leak.md), [failure-case-language-locale-leak.md](./failure-case-language-locale-leak.md).

## One-line summary

> Route abnormalities into **process / content / cross-turn contradiction** (then type boundaries)—named families matched to observables, not open intent-inference or “least harm” improvisation.
