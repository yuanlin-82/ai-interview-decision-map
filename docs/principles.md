# Design Principles

Short constraints that sit *above* wording. Implementations may differ; violating these usually breaks interview quality.

## Routing

1. **Split lanes early.** Classify the latest turn as flow/abnormal vs substantively normal before composing a probe.  
2. **Recover with action classes, not creativity.** Empty, unintelligible, quit/skip, challenge, and re-anchor cases use constrained recovery moves.  
3. **One open question per turn.** Do not chain “and also…” probes.

## Re-anchoring

4. **Restate the last interviewer question** when the candidate forgot, misheard, or needs the ask re-placed in dialogue.  
5. **Do not bind re-anchor to the stem by default.** The original item is correct only when it *was* the last ask (typically early turns).  
6. **Off-topic and forgot share a re-anchor family on the overview map** (`Need_Question_Reanchor`); type maps may specialize tone.

## Grounding & safety

7. **Never invent candidate premises** (“when you led the project…”) unless the candidate supplied them.  
8. **Silent ASR repair** when intent is recoverable; do not quiz the candidate about recognition noise.  
9. **Type-specific safety/boundary packs** (e.g. non-clinical framing, employer-neutrality) override generic probing when triggered — shown as one overview node, detailed per type.

## Normal probing

10. **Probe from an anchor** in the candidate’s meaning; if there is no anchor, you are still on the abnormal lane.  
11. **Strategy packs are typed.** Behavior ≠ information ≠ culture-fit; only the overview router is universal.  
12. **Prefer depth over completeness.** After one deep follow-up on a detail, shift aspect rather than drilling the same cell.

## Surface form

13. **Same contract, replaceable surfaces.** Language (ja/en/zh) and model vendor may change copy; action classes should not.  
14. **Keep public docs free of production utterances.** Publish decisions; withhold scripts.

## Evaluation mindset

15. **Judge routes, then sentences.** A beautiful sentence on the wrong edge is still a failure.  
16. **Failure cases update edges.** Prompt polish without contract change is optional; wrong re-anchor targets are not.
