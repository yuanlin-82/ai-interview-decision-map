# Type map: `domain-knowledge`

Extends the [overview](./overview.md).

**Intent:** oral **domain / professional knowledge** checks—closest to a spoken technical interview item, not soft behavioral storytelling.

```mermaid
flowchart TD
  Start([Enter domain-knowledge turn]) --> Ov[Overview router]
  Ov -->|Need_Question_Reanchor| R[Restate_Last_Interviewer_Question]
  Ov -->|other abnormal| Ab[Other overview abnormal actions]
  Ov -->|normal| Pack{Strategy_Pack domain-knowledge}

  Pack -->|Factual_recall_only| A[Link_to_job_application]
  Pack -->|Simple_application| D[Deepen_method_or_tradeoff]
  Pack -->|Rich_analysis| S[Synthesize_under_complex_case]

  A --> One[Ask_Single_Open_Question]
  D --> One
  S --> One
  R --> Await([Await_Next_Turn])
  Ab --> Await
  One --> Await
```

## Pack rules

- Probe **what they know and how they use it** in the role’s domain (tools, concepts, procedures, judgment).  
- Move **one proficiency step** per turn: recall → application → analysis/synthesis.  
- Prefer job-relevant depth over trivia or gotcha quizzes.  
- Synthesis needs an anchor in what they already claimed.  
- One open question per turn; no multi-part oral exams stacked.
