# Type map: `opening-intro`

Extends the [overview](./overview.md).

```mermaid
flowchart TD
  Start([Enter opening-intro turn]) --> Ov[Overview router]
  Ov -->|Need_Question_Reanchor| R[Restate_Last_Interviewer_Question]
  Ov -->|other abnormal| Ab[Other overview abnormal actions]
  Ov -->|normal| Pack{Strategy_Pack opening-intro}

  Pack -->|Claim_to_verify| V[Verify_one_claim]
  Pack -->|Motivation_thin| M[Probe_motivation]
  Pack -->|Fit_signal_unclear| F[Distill_role_fit_signal]

  V --> One[Ask_Single_Open_Question]
  M --> One
  F --> One
  R --> Await([Await_Next_Turn])
  Ab --> Await
  One --> Await
```

## Pack rules

- Opening turns set trust: keep asks concrete and answerable.  
- Thin intros still get **one** directed ask for role-relevant experience — not a lecture.  
- Re-anchor uses last interviewer ask (often the intro prompt itself early on).
