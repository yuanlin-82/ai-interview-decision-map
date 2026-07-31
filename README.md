# AI Interview Follow-up Decision Map

**For builders of multi-turn / voice interview agents** who need auditable turn routing (exception lanes vs typed probes)—not another prompt dump.

A decision map for AI interview follow-up turns, reverse-derived from real multi-turn dialogues: which route to take after hearing the candidate — not a dump of production prompts.

> **Scope:** methodology & decision contract  
> **Out of scope:** production prompts, spoken copy decks, raw transcripts  
> See [NOTICE.md](./NOTICE.md).

---

## Why this exists

Follow-up quality is mostly a **routing problem**, not a prose problem.  
Across languages and model vendors, the stable asset is the *decision contract*: given dialogue state, which action class should fire, under which constraints—especially when you are also trading off **fat prompts** against layered routing under **sub-second TTS** latency.

This repo turns that contract into something you can:

- read as a map,
- compare across question types,
- audit when a dialogue goes wrong,
- implement privately without copying proprietary wording.

---

## What you will find

| Path | Content |
|------|---------|
| [maps/overview.md](./maps/overview.md) | Global decision map (action-level) |
| [maps/](./maps/) | Illustrative type-level maps + index |
| [docs/question-types.md](./docs/question-types.md) | Normal probe packs by type (at-a-glance对照) |
| [docs/principles.md](./docs/principles.md) | Short design principles |
| [docs/methodology.md](./docs/methodology.md) | How the map was reverse-derived |
| [docs/label-schema.md](./docs/label-schema.md) | Conceptual label / “good vs bad turn” contract |
| [docs/competency-to-scenario.md](./docs/competency-to-scenario.md) | Upstream sketch: competency → scenario intent → type key |
| [docs/type-classification.md](./docs/type-classification.md) | Stem → compliance screen → single type key |
| [docs/abnormal-responses.md](./docs/abnormal-responses.md) | Shared abnormal recoveries + type-specific overrides |
| [docs/eval-loop.md](./docs/eval-loop.md) | Sample → judge route → cluster fails → patch map/labels |
| [docs/followup-quality.md](./docs/followup-quality.md) | How to judge the probe itself (paired compare, fatals first) |
| [docs/stop-conditions.md](./docs/stop-conditions.md) | Stop follow-ups vs force-close item; time, rounds, stop classifier |
| [docs/failure-case-reanchor.md](./docs/failure-case-reanchor.md) | Field note: stem vs last-ask re-anchor |
| [docs/failure-case-behavioral-evidence.md](./docs/failure-case-behavioral-evidence.md) | Field note: “I would” chat vs past-episode evidence |
| [docs/failure-case-job-transition-boundary.md](./docs/failure-case-job-transition-boundary.md) | Field note: former-employer complaints vs safety boundary |
| [docs/failure-case-situational-overgeneralize.md](./docs/failure-case-situational-overgeneralize.md) | Field note: overgeneralizing one situational follow-up rule |
| [docs/failure-case-workplace-resilience-frontline.md](./docs/failure-case-workplace-resilience-frontline.md) | Field note: blunt frontline answers misread as “no intent” |
| [docs/failure-case-process-leak.md](./docs/failure-case-process-leak.md) | Field note: analysis / process text leaking into TTS |
| [docs/failure-case-language-locale-leak.md](./docs/failure-case-language-locale-leak.md) | Field note: non-English tokens in English probes (English-track only) |
| [examples/walkthroughs.md](./examples/walkthroughs.md) | Anonymized path walkthroughs |

## What you will not find

- Full production prompts (GPT / Qwen / …)
- Multi-lingual fixed reply corpora
- Original candidate audio, ASR logs, or PII
- Internal variable registries or deployment configs

Production systems may implement the same contract with different wording. That implementation layer stays private.

---

## Suggested reading order

1. **[principles.md](./docs/principles.md)** — constraints above wording (one ask, last-ask re-anchor, observables over mind-reading).  
2. **[maps/overview.md](./maps/overview.md)** — shared abnormal vs normal router.  
3. **[abnormal-responses.md](./docs/abnormal-responses.md)** — process / content / cross-turn contradiction + type overrides.  
4. **[question-types.md](./docs/question-types.md)** — normal packs at a glance → open one [type map](./maps/) for depth.  
5. **[type-classification.md](./docs/type-classification.md)** + **[stop-conditions.md](./docs/stop-conditions.md)** — how a stem gets a type; when probing ends.  
6. **[followup-quality.md](./docs/followup-quality.md)** + **[eval-loop.md](./docs/eval-loop.md)** — how to judge probes and feed failures back.  
7. **[examples/walkthroughs.md](./examples/walkthroughs.md)** + **`docs/failure-case-*.md`** — concrete paths and field notes.

Upstream design sketch (competency → scenario → type): [competency-to-scenario.md](./docs/competency-to-scenario.md).  
How the maps were reverse-derived: [methodology.md](./docs/methodology.md).

---

## How to read the maps

1. **Diamonds** = judgments on the candidate’s latest turn (and light dialogue context).  
2. **Rounded boxes** = action classes (what the interviewer should *do*), not scripts.  
3. **Solid arrows** = primary routing.  
4. Hard constraint on the normal path: **one probe per turn**.

Key global rule called out on the overview map:

> When re-anchoring to “the question”, restate the **last question the interviewer asked in this dialogue** — not necessarily the original stem item — unless that stem *was* the last ask.

---

## Question types at a glance

Follow-up is not one skill. The contract is typed.  
Maps cover a fuller matrix (still methodology-only):

| Type key | Assessment intent (short) |
|----------|---------------------------|
| `behavioral` | Past behavior as evidence of future performance |
| `situational` | Capability under scenario tension |
| `issue-reasoning` | Open-topic critical thinking & argumentation |
| `domain-knowledge` | Domain / professional knowledge (spoken technical-style) |
| `career-choice` | Career planning maturity |
| `information-gathering` | Collect & confirm information slots |
| `workplace-resilience` | Work-framed stress handling (non-clinical) |
| `job-transition` | Leave / job-change drivers |
| `culture-fit` | Preference, trade-offs, contribution |
| `opening-intro` | Opening narrative: claims, motivation, clarity |
| `fallback` | Safe continuation when no specialized pack fits |

Index: [maps/README.md](./maps/README.md) · At-a-glance packs: [docs/question-types.md](./docs/question-types.md).

---

## Worked example (one path)

Candidate answers a behavioral probe → interviewer deepens one STAR gap → candidate says they forgot the question → interviewer **restates the last follow-up** (not the original stem) → candidate answers → interviewing continues.

Full narratives: [examples/walkthroughs.md](./examples/walkthroughs.md) (re-anchor, type boundary, fallback, empty recovery, stop-brake miss).

---

## How the map was derived (summary)

1. Sample real multi-turn follow-up dialogues (internal corpora).  
2. Label each interviewer turn with **state** + **action class**.  
3. Merge conflicts into a minimal router (abnormal vs normal).  
4. Freeze as a decision contract; fix edges from failure cases (e.g. wrong restate target).  
5. Keep spoken realizations proprietary; publish only the contract.

Longer note: [docs/methodology.md](./docs/methodology.md).

---

## Design principles (preview)

- Separate **flow/abnormal handling** from **substantive probing**.  
- Prefer **fixed action classes** for recovery; generative probing only when there is an anchor.  
- **One open question per turn.**  
- Restate **last interviewer question** when re-anchoring.  
- Silent ASR repair when intent is recoverable; don’t interrogate recognition noise.  
- Same contract, replaceable surface forms (language / model).

Full list: [docs/principles.md](./docs/principles.md).

---

## Labels & eval (preview)

Routing alone is not enough for product work. This repo also sketches:

- **[label-schema.md](./docs/label-schema.md)** — fields for state / action_class / quality flags; judge route before surface.  
- **[followup-quality.md](./docs/followup-quality.md)** — paired LLM-judge comparison for probe wording (fatals → dimensions; humans spot-check extremes).  
- **[stop-conditions.md](./docs/stop-conditions.md)** — when to stop follow-ups vs force-close the item (time / rounds / stop classifier).  
- **[competency-to-scenario.md](./docs/competency-to-scenario.md)** — how competencies connect to typed follow-up packs.  
- **[type-classification.md](./docs/type-classification.md)** — compliance screen then one primary-intent type key per stem.  
- **[abnormal-responses.md](./docs/abnormal-responses.md)** — shared recovery actions vs type boundary overrides.  
- **[eval-loop.md](./docs/eval-loop.md)** — failure clusters feed map and label fixes, not only prompt polish.

Still methodology-only: no production prompts or customer stems.

---

## Honest scope gap: session-level flow

This repo is strong on **within-item** follow-up: type the stem, recover abnormalities, probe once per turn, stop the item.

**Whole-interview orchestration** (how items should sequence, how evidence should accumulate across stems, how opening/closing should frame the session) is **not** designed here in depth. Live products often run **item → item** with transition copy between them. That is a real product gap—not a hidden chapter of this map. Treat inter-item transitions as UX glue unless you separately design session policy.

---

## License / sharing stance

Methodology text and maps in this folder are intended for open discussion and GitHub presentation.  
**Do not treat this repo as a substitute for production prompts.** Implementations must be authored under your own IP and safety review.
