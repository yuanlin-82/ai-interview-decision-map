# Type map: `workplace-resilience`

Extends the [overview](./overview.md). Demonstrates overview choice **B**: dashed type boundary.

**Intent:** work-framed stress handling (deadlines, conflict, change, load)—not clinical diagnosis.

```mermaid
flowchart TD
  Start([Enter workplace-resilience turn]) --> Ov[Overview router]

  Ov -->|Type safety trigger| Bdry[Type_Specific_Boundary]
  Ov -->|Need_Question_Reanchor| R[Restate_Last_Interviewer_Question]
  Ov -->|other abnormal| Ab[Other overview abnormal actions]
  Ov -->|normal work-framed anchor| Pack{Strategy_Pack workplace-resilience}

  Bdry --> B1[Redirect_non_work_trauma]
  Bdry --> B2[Allow_soft_pass_or_smaller_work_frame]
  B1 --> Await([Await_Next_Turn])
  B2 --> Await

  Pack -->|Emotion_handling| P1[Probe_how_emotion_was_handled]
  Pack -->|Action_logic| P2[Probe_first_actions_and_criteria]
  Pack -->|Reflection_growth| P3[Probe_work_context_learning]

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

- Non-clinical: no diagnosis, no pathology labels, no probing private trauma for its own sake.  
- Prefer **how** over judgmental **why** when exploring emotion.  
- Keep examples in workplace frames.  
- Pick **one** avenue per turn: emotion · action · work-context learning.  
- Boundary actions outrank curiosity probes when triggered.

Field note: [failure-case-workplace-resilience-frontline.md](../docs/failure-case-workplace-resilience-frontline.md) (frontline / home-service population variant—same type, tighter lane judgment).
