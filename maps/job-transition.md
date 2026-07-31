# Type map: `job-transition`

Extends the [overview](./overview.md). Demonstrates overview choice **B**: type boundary as a dashed override.

**Intent:** reasons and patterns behind **leaving a job / changing employers**—离职与求职动因.

```mermaid
flowchart TD
  Start([Enter job-transition turn]) --> Ov[Overview router]

  Ov -->|Type safety trigger| Bdry[Type_Specific_Boundary]
  Ov -->|Need_Question_Reanchor| R[Restate_Last_Interviewer_Question]
  Ov -->|other abnormal| Ab[Other overview abnormal actions]
  Ov -->|normal| Pack{Strategy_Pack job-transition}

  Bdry --> B1[Redirect_from_employer_bashing]
  Bdry --> B2[Reframe_to_environment_fit]
  B1 --> Await([Await_Next_Turn])
  B2 --> Await

  Pack -->|Decision_process| P1[Probe_how_they_decided]
  Pack -->|Values| P2[Probe_what_they_optimized_for]
  Pack -->|Patterns| P3[Probe_patterns_across_moves]

  P1 --> One[Ask_Single_Open_Question]
  P2 --> One
  P3 --> One
  R --> Await
  Ab --> Await
  One --> Await

  classDef dash stroke-dasharray: 5 5
  class Bdry dash
```

## Pack rules

- Non-judgmental: understand drivers of moves; don’t prosecute past choices.  
- When negativity toward former employers (or managers/colleagues) appears, **boundary actions outrank curiosity**—redirect to environment fit / future seeking; do not dig grievance detail.  
- One probe family per turn: decision process, values, or patterns.  
- Prefer *how/what* over accusatory *why* when discussing past moves.  
- Distinct from [`career-choice`](./career-choice.md) (future planning maturity).

Field note: [failure-case-job-transition-boundary.md](../docs/failure-case-job-transition-boundary.md).
