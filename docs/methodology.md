# Methodology: From Dialogues to a Decision Map

## Goal

Recover an explicit **turn-level decision contract** for AI interview follow-ups:

> After the candidate’s latest utterance, which judgment fires, and which action class should the interviewer take?

The contract must be stable across languages and model vendors, while spoken realizations remain private.

## Why reverse derivation

Hand-written “best practice” checklists drift from what actually happens in production dialogues.  
We treat logged multi-turn sessions as the source of truth, then compress repeated patterns into a map that can be audited and re-implemented.

## Pipeline

```text
Dialogue corpus (internal)
        │
        ▼
Turn labeling  ──  candidate state + interviewer action class
        │
        ▼
Conflict merge ──  collapse near-duplicate routes; keep rare-but-critical edges
        │
        ▼
Contract freeze ──  overview router + typed strategy packs
        │
        ▼
Failure-driven edge fixes ──  e.g. restate target = last ask, not stem only
        │
        ▼
Public artifact = maps + principles
Private artifact = prompts / copy
```

### 1) Sampling

- Multi-turn follow-ups only (not one-shot scoring essays).  
- Cover all question types in the product matrix.  
- Include “ugly” turns: empty answers, off-topic, challenges, ASR debris, mid-dialogue “what was the question?”.

### 2) Labeling schema (conceptual)

For each interviewer turn, annotate at least:

| Field | Meaning |
|-------|---------|
| `type` | Question-type key (`behavioral`, `fallback`, …) |
| `lane` | `abnormal` \| `normal` |
| `judgment` | e.g. `Need_Question_Reanchor`, `Empty_Or_Fillers`, … |
| `action_class` | e.g. `Restate_Last_Interviewer_Question`, `Ask_Single_Open_Question` |
| `probe_family` (normal only) | Intent family inside the type’s strategy pack |

Labels name **actions**, not sentences. Wording is ignored at this stage.

### 3) Conflict merge

When two annotators (or two prompt versions) disagree:

1. Prefer routes that **preserve interview continuity** without inventing candidate facts.  
2. Prefer **one question per turn** over multi-part stacks.  
3. Prefer **type-agnostic recovery** on the overview map; push specialty to type packs / type maps.  
4. Drop edges that only encode stylistic preference.

### 4) Contract freeze

Publishable shape:

- **Overview map** — shared router (this repo).  
- **Strategy packs** — per-type normal probing intents (tables + sample maps).  
- **Principles** — cross-cutting constraints.

### 5) Failure-driven revisions

Example (observed class of failures):

- Symptom: on turn 3+, candidate asks to repeat “the question”; system repeats the **original stem**.  
- Diagnosis: re-anchor action was bound to a stem injection variable.  
- Contract fix: re-anchor uses **last interviewer question in dialogue** (stem only if it was last).  
- Implementation may use a model-filled slot; the *decision* is what we publish, not the slot syntax.

## What we deliberately do not publish

- Prompt text, few-shot banks, fixed utterance lists.  
- Raw transcripts or recoverable PII.  
- Vendor-specific wording optimizations (GPT vs Qwen surface form).

Those belong to implementation repos under restricted access.

## Validity checks (lightweight)

A contract revision should pass:

1. **Coverage** — sampled abnormal classes still route somewhere.  
2. **Non-leakage** — public docs contain no production copy paragraphs.  
3. **Walkthrough test** — at least one normal→reanchor→normal path remains coherent.  
4. **Type sanity** — each type still has a distinct normal pack intent.

## Relation to production

```text
Decision contract (public)  →  guides design review & onboarding
        │
        ▼
Private prompts / policies  →  bind variables, languages, model knobs
        │
        ▼
Live dialogues              →  feed the next labeling round
```

The map is not a chatbot. It is the **spec the chatbot is supposed to obey**.
