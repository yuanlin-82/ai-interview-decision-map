# Type map: `mentalhealth` (workplace resilience)

Extends the [overview](./overview.md). Demonstrates overview choice **B**: dashed type boundary.

```mermaid
flowchart TD
  Start([Enter resilience turn]) --> Ov[Overview router]

  Ov -->|Type safety trigger| Bdry[Type_Specific_Boundary]
  Ov -->|Need_Question_Reanchor| R[Restate_Last_Interviewer_Question]
  Ov -->|other abnormal| Ab[Other overview abnormal actions]
  Ov -->|normal work-framed anchor| Pack{Strategy_Pack mentalhealth}

  Bdry --> B1[Redirect_non_work_trauma]
  Bdry --> B2[Allow_soft_pass_or_smaller_work_frame]
  B1 --> Await([Await_Next_Turn])
  B2 --> Await

  Pack -->|Awareness_regulation| P1[Probe_work_emotion_handling]
  Pack -->|Coping_action| P2[Probe_coping_actions]
  Pack -->|Learning| P3[Probe_learning]

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

- Non-clinical: no diagnosis, no probing private trauma for its own sake.  
- Keep examples in workplace frames (deadlines, conflict, change, load).  
- Boundary actions outrank curiosity probes when triggered.
