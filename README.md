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
| [docs/question-types.md](./docs/question-types.md) | Question-type matrix (intent only) |
| [docs/principles.md](./docs/principles.md) | Short design principles |
| [docs/methodology.md](./docs/methodology.md) | How the map was reverse-derived |
| [examples/walkthroughs.md](./examples/walkthroughs.md) | Anonymized path walkthroughs |

## What you will not find

- Full production prompts (GPT / Qwen / …)
- Multi-lingual fixed reply corpora
- Original candidate audio, ASR logs, or PII
- Internal variable registries or deployment configs

Production systems may implement the same contract with different wording. That implementation layer stays private.

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
Below is an **illustrative subset** (not an exhaustive product catalog):

| Type key | Assessment intent (short) |
|----------|---------------------------|
| `behavioral` | Past behavior as evidence of future performance |
| `situational` | Capability under scenario tension |
| `career-choice` | Decision process behind career direction |
| `job-transition` | Drivers of job moves (non-judgmental) |
| `opening-intro` | Opening narrative: claims, motivation, clarity |
| `fallback` | Safe continuation when no specialized pack fits |

Details: [docs/question-types.md](./docs/question-types.md).

---

## Worked example (one path)

Candidate answers a behavioral probe → interviewer deepens one STAR gap → candidate says they forgot the question → interviewer **restates the last follow-up** (not the original stem) → candidate answers → interviewing continues.

Full narrative: [examples/walkthroughs.md](./examples/walkthroughs.md).

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

## License / sharing stance

Methodology text and maps in this folder are intended for open discussion and GitHub presentation.  
**Do not treat this repo as a substitute for production prompts.** Implementations must be authored under your own IP and safety review.
