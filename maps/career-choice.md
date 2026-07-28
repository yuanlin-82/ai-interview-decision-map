# Type map: `career-choice`

Extends the [overview](./overview.md).

```mermaid
flowchart TD
  Start([Enter career-choice turn]) --> Ov[Overview router]
  Ov -->|Need_Question_Reanchor| R[Restate_Last_Interviewer_Question]
  Ov -->|other abnormal| Ab[Other overview abnormal actions]
  Ov -->|normal| Pack{Strategy_Pack career-choice}

  Pack -->|Motivation_gap| M[Probe_why_this_direction]
  Pack -->|Preparation_gap| P[Probe_how_they_prepared]

  M --> One[Ask_Single_Open_Question]
  P --> One
  R --> Await([Await_Next_Turn])
  Ab --> Await
  One --> Await
```

## Pack rules

- Assess **decision process**, not whether the path is “correct”.  
- Pick motivation **or** preparation in one turn — not both stacked.  
- Stay within the career topic boundary of the stem thread.
