# Design Principles

Short constraints that sit *above* wording. Implementations may differ; violating these usually breaks interview quality.

## Routing

1. **Split lanes early.** Classify the latest turn as flow/abnormal vs substantively normal before composing a probe. Shared recoveries and type overrides: [abnormal-responses.md](./abnormal-responses.md).  
2. **Recover with action classes, not creativity.** Empty, unintelligible, quit/skip, challenge, and re-anchor cases use constrained recovery moves.  
3. **One open question per turn.** Do not chain “and also…” probes.

## Re-anchoring

4. **Restate the last interviewer question** when the candidate forgot, misheard, or needs the ask re-placed in dialogue.  
5. **Do not bind re-anchor to the stem by default.** The original item is correct only when it *was* the last ask (typically early turns).  
6. **Off-topic and forgot share a re-anchor family on the overview map** (`Need_Question_Reanchor`); type maps may specialize tone.

## Grounding & safety

7. **Never invent candidate premises** (“when you led the project…”) unless the candidate supplied them.  
7a. **Prefer observables over mind-reading.** Abnormal recovery should key off what was said, missing, or clashing across turns—not a free inference of “what they really intended.” (Design preference for stable routing; not a claim about model internals.)  
8. **Silent ASR repair** when intent is recoverable; do not quiz the candidate about recognition noise.  
9. **Type-specific safety/boundary packs** override generic probing when triggered — shown as one overview node, detailed per type. Example: on `job-transition`, do not dig complaints about a former employer; redirect to fit / future seeking.

## Normal probing

10. **Probe from an anchor** in the candidate’s meaning; if there is no anchor, you are still on the abnormal lane.  
11. **Strategy packs are typed.** `behavioral` ≠ `job-transition` ≠ `fallback`; only the overview router is universal.  
12. **Prefer depth over completeness.** After one deep follow-up on a detail, shift aspect rather than drilling the same cell. Prefer the **high-gain aspect** (empty job-evidence cells), not more asks of the same cell.  
13. **For `behavioral`, enforce evidence type.** Ideal / hypothetical “I would…” is not a past episode — ground first; do not accompany the fiction. Digging has a ceiling (termination is a separate product policy).  
14. **For `situational`, do not overgeneralize one probe rule.** Past-tense digs, boundary/risk probes, and “prefer real behavior” each need branch gates; legal answer shapes are wide (plan / past / stance / mix).

## Surface form

15. **Same contract, replaceable surfaces.** Language (ja/en/zh) and model vendor may change copy; action classes should not.  
16. **Keep public docs free of production utterances.** Publish decisions; withhold scripts.

## Evaluation mindset

17. **Judge routes, then sentences.** A beautiful sentence on the wrong edge is still a failure.  
18. **Failure cases update edges.** Prompt polish without contract change is optional; wrong re-anchor targets are not.
