# Type map: `default`

Extends the [overview](./overview.md). Used when no specialized competency model fits.

```mermaid
flowchart TD
  Start([Enter default turn]) --> Ov[Overview router]
  Ov -->|Need_Question_Reanchor| R[Restate_Last_Interviewer_Question]
  Ov -->|other abnormal| Ab[Other overview abnormal actions]
  Ov -->|normal| Pack{Strategy_Pack default}

  Pack -->|Missing_depth_on_5W1H| H[Pick_one_5W1H_cell]
  Pack -->|Needs_concrete_scene| E[Request_one_example]

  H --> One[Ask_Single_Open_Question]
  E --> One
  R --> Await([Await_Next_Turn])
  Ab --> Await
  One --> Await
```

## Pack rules

- Neutral-curious: deepen understanding, don’t score aloud.  
- Exactly one 5W1H cell **or** one example request — never both stacked.
