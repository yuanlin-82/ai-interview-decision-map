# Type map: `opening-intro`

Extends the [overview](./overview.md). Normal pack picks **one** of three strategies from the intro content.

```mermaid
flowchart TD
  Start([Enter opening-intro turn]) --> Ov[Overview router]
  Ov -->|Need_Question_Reanchor| R[Restate_Last_Interviewer_Question]
  Ov -->|other abnormal| Ab[Other overview abnormal actions]
  Ov -->|normal| Pack{Strategy_Pack opening-intro}

  Pack -->|Claim_to_verify| V[Verify_one_claim_with_actions]
  Pack -->|Motivation_thin| M[Probe_motivation_for_this_role]
  Pack -->|Rich_but_scattered| F[Distill_one_role_fit_signal]

  V --> One[Ask_Single_Open_Question]
  M --> One
  F --> One
  R --> Await([Await_Next_Turn])
  Ab --> Await
  One --> Await
```

## Pack rules

- Opening turns set trust: keep asks concrete and answerable.  
- Thin intros still get **one** directed ask — not a lecture.  
- Scattered intros → distill (e.g. one strength / fit signal), not more open browsing.  
- Re-anchor uses last interviewer ask (often the intro prompt itself early on).
