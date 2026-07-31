# Type map: `issue-reasoning`

Extends the [overview](./overview.md).

**Intent:** open-ended issue / debate items that test **critical thinking and argumentation**.

```mermaid
flowchart TD
  Start([Enter issue-reasoning turn]) --> Ov[Overview router]
  Ov -->|Need_Question_Reanchor| R[Restate_Last_Interviewer_Question]
  Ov -->|other abnormal| Ab[Other overview abnormal actions]
  Ov -->|normal| Pack{Strategy_Pack issue-reasoning}

  Pack -->|Basic_stance_only| C[Probe_reason_for_stance]
  Pack -->|Reasoned_but_thin| D[Deepen_one_supporting_point]
  Pack -->|In_depth_with_tradeoffs| T[Stress_test_with_counterfactual]

  C --> One[Ask_Single_Open_Question]
  D --> One
  T --> One
  R --> Await([Await_Next_Turn])
  Ab --> Await
  One --> Await
```

## Pack rules

- Target is **reasoning process**, not whether the conclusion is “correct.” Stay neutral.  
- Grade sophistication, then one mode: build reasons → deepen one support → resilience under a counterfactual / opposing view.  
- Do not turn the item into a domain quiz or a values checklist.  
- Echo the candidate’s stance only to ground one clear point; do not invent positions.  
- One open question per turn.
