# Type map: `fallback`

Extends the [overview](./overview.md).

**Role of this pack:** safe **continuation** of the interview when no specialized competency frame fits. Prioritize keeping the dialogue usable and neutral — not forcing a clever but wrong lens.

```mermaid
flowchart TD
  Start([Enter fallback turn]) --> Ov[Overview router]
  Ov -->|Need_Question_Reanchor| R[Restate_Last_Interviewer_Question]
  Ov -->|other abnormal| Ab[Other overview abnormal actions]
  Ov -->|normal| Pack{Strategy_Pack fallback}

  Pack -->|Needs_clarity| H[Pick_one_clarifying_cell]
  Pack -->|Needs_concrete_scene| E[Request_one_example]

  H --> One[Ask_Single_Open_Question]
  E --> One
  R --> Await([Await_Next_Turn])
  Ab --> Await
  One --> Await
```

## Pack rules

- Goal: **continue safely** — clarify or concretize without over-fitting a specialty model.  
- Neutral-curious; do not score aloud.  
- Exactly one clarifying deepen **or** one example request — never both stacked.
