# Type map: `behavioral`

Extends the [overview](./overview.md). Shows how the normal pack specializes — still without production copy.

```mermaid
flowchart TD
  Start([Enter behavioral turn]) --> Ov[Overview router]
  Ov -->|Need_Question_Reanchor| R[Restate_Last_Interviewer_Question]
  Ov -->|other abnormal| Ab[Other overview abnormal actions]
  Ov -->|normal| Pack{Strategy_Pack behavioral}

  Pack -->|Abstract_only| G[Ground_in_real_episode]
  Pack -->|Concrete_episode| D{What is missing most?}

  D -->|Clarity of story| D1[Probe_missing_clarity]
  D -->|Judgment / tradeoff| D2[Probe_judgment]
  D -->|Contribution| D3[Probe_contribution]
  D -->|Reflection / learning| D4[Probe_reflection]

  G --> One[Ask_Single_Open_Question]
  D1 --> One
  D2 --> One
  D3 --> One
  D4 --> One
  R --> Await([Await_Next_Turn])
  Ab --> Await
  One --> Await
```

## Pack rules

- Choose **one** branch per turn.  
- After one deep follow-up on a cell, prefer shifting aspect next turn.  
- No premise invention when grounding.  
- Ideal / hypothetical answers (“I would…”) are still **Abstract_only**: ground in a real episode; do not deepen the plan as if it were past behavior.  
- If no episode can be elicited, stop unlimited digging and defer to product **dialogue-termination** (out of scope for this map).

Field note: [failure-case-behavioral-evidence.md](../docs/failure-case-behavioral-evidence.md).
