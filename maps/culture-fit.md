# Type map: `culture-fit`

Extends the [overview](./overview.md). Normal pack deepens **preferences and contribution**—infer fit without scoring culture aloud.

```mermaid
flowchart TD
  Start([Enter culture-fit turn]) --> Ov[Overview router]
  Ov -->|Need_Question_Reanchor| R[Restate_Last_Interviewer_Question]
  Ov -->|other abnormal| Ab[Other overview abnormal actions]
  Ov -->|normal| Pack{Strategy_Pack culture-fit}

  Pack -->|Thin_example| E[Deepen_one_concrete_experience]
  Pack -->|Preferences_unclear_priority| T[Probe_value_tradeoff]
  Pack -->|Wants_without_gives| C[Probe_contribution_actions]

  E --> One[Ask_Single_Open_Question]
  T --> One
  C --> One
  R --> Await([Await_Next_Turn])
  Ab --> Await
  One --> Await
```

## Pack rules

- Infer from **behavior and trade-offs**, not from “do you like our values” checklists.  
- Trade-off probes need two preferences the candidate already voiced—do not invent A vs B.  
- Balance “what they seek” with “what they contributed” when the narrative is wants-only.  
- One open question per turn; stay non-evaluative in wording.
