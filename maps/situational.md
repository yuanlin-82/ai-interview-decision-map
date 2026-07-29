# Type map: `situational`

Extends the [overview](./overview.md).

```mermaid
flowchart TD
  Start([Enter situational turn]) --> Ov[Overview router]
  Ov -->|Need_Question_Reanchor| R[Restate_Last_Interviewer_Question]
  Ov -->|other abnormal| Ab[Other overview abnormal actions]
  Ov -->|normal| Pack{Strategy_Pack situational}

  Pack -->|Past_event_cited| P[Deepen_decision_under_constraints]
  Pack -->|Hypothetical_plan| H[Probe_structure_tradeoff_or_risk]
  Pack -->|Vague_principle_only| G[Ask_for_concrete_first_steps]

  P --> One[Ask_Single_Open_Question]
  H --> One
  G --> One
  R --> Await([Await_Next_Turn])
  Ab --> Await
  One --> Await
```

## Pack rules

- Classify past-event vs hypothetical before probing — both (and thin stance / mixtures) can be **legal** on situational items.  
- Do not overgeneralize one good rule: past-tense probes belong to real episodes; risk probes need a brake (no chained “what if that fails” on the same line).  
- After one risk probe, shift dimension.  
- One open question per turn.  
- Hold the shared hypothetical frame when the answer is still a plan; dig experience when a resonant past episode is clearly offered.

Field note: [failure-case-situational-overgeneralize.md](../docs/failure-case-situational-overgeneralize.md).
