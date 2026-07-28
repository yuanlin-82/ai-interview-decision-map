# Type map: `information`

Extends the [overview](./overview.md). Normal pack optimizes for **slot completeness and confirmation**, then stop.

```mermaid
flowchart TD
  Start([Enter information turn]) --> Ov[Overview router]
  Ov -->|Need_Question_Reanchor| R[Restate_Last_Interviewer_Question]
  Ov -->|other abnormal| Ab[Other overview abnormal actions]
  Ov -->|normal| Pack{Strategy_Pack information}

  Pack -->|Missing_slot| F[Fill_missing_slot]
  Pack -->|Ambiguous_value| A[Disambiguate_value]
  Pack -->|Suspected_ASR| C[Confirm_suspected_value]
  Pack -->|Complete_and_clear| S[Stop_or_light_confirm]

  F --> One[Ask_Single_Open_Question_or_Confirm]
  A --> One
  C --> One
  S --> Await([Await_Next_Turn])
  R --> Await
  Ab --> Await
  One --> Await
```

## Pack rules

- Do not re-ask slots already clearly answered.  
- Prefer confirmations over exploratory chatting once facts are complete.  
- Re-anchor still uses **last interviewer ask** (often the slot question itself).
