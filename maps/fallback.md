# Type map: `fallback`

Extends the [overview](./overview.md).

**Role of this pack:** safe **continuation** when no specialized competency frame fits (`default` pack in some product matrices). Prioritize keeping the dialogue usable and neutral.

```mermaid
flowchart TD
  Start([Enter fallback turn]) --> Ov[Overview router]
  Ov -->|Need_Question_Reanchor| R[Restate_Last_Interviewer_Question]
  Ov -->|other abnormal| Ab[Other overview abnormal actions]
  Ov -->|normal| Pack{Strategy_Pack fallback}

  Pack -->|Missing_what_or_how| H[Pick_one_5W1H_cell]
  Pack -->|Missing_why_who_when| H2[Pick_one_5W1H_cell]
  Pack -->|No_clear_5W1H_gap| E[Request_one_example]

  H --> One[Ask_Single_Open_Question]
  H2 --> One
  E --> One
  R --> Await([Await_Next_Turn])
  Ab --> Await
  One --> Await
```

## Pack rules

- Goal: **continue safely** — clarify or concretize without forcing a wrong specialty lens.  
- Choose the **single** most missing 5W1H cell (what / how / why / who / when-where), or fall back to one example request.  
- Neutral-curious; do not score aloud.  
- Never stack multiple 5W1H asks in one turn.
