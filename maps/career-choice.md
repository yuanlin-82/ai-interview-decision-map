# Type map: `career-choice`

Extends the [overview](./overview.md). Normal pack grades **planning maturity**, then probes one step deeper.

```mermaid
flowchart TD
  Start([Enter career-choice turn]) --> Ov[Overview router]
  Ov -->|Need_Question_Reanchor| R[Restate_Last_Interviewer_Question]
  Ov -->|other abnormal| Ab[Other overview abnormal actions]
  Ov -->|normal| Pack{Strategy_Pack career-choice}

  Pack -->|Surface_goals_only| P[Probe_near_term_actions]
  Pack -->|Structured_plan| F[Probe_feasibility_or_risk]
  Pack -->|Value_aligned_vision| T[Probe_tradeoff_under_pressure]

  P --> One[Ask_Single_Open_Question]
  F --> One
  T --> One
  R --> Await([Await_Next_Turn])
  Ab --> Await
  One --> Await
```

## Pack rules

- Assess **decision process and maturity**, not whether the path is “correct.”  
- One maturity mode per turn: build plan → stress feasibility → value trade-off.  
- Stay within the career topic boundary of the stem thread.  
- Do not stack motivation + preparation + trade-off in one ask.
