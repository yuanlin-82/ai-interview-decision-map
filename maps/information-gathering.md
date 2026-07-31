# Type map: `information-gathering`

Extends the [overview](./overview.md). Normal pack optimizes for **collecting and confirming slots**, then stop—not open-ended exploration.

```mermaid
flowchart TD
  Start([Enter information-gathering turn]) --> Ov[Overview router]
  Ov -->|Need_Question_Reanchor| R[Restate_Last_Interviewer_Question]
  Ov -->|other abnormal| Ab[Other overview abnormal actions]
  Ov -->|normal| Pack{Strategy_Pack information-gathering}

  Pack -->|Admin_or_demographic| A[Confirm_fixed_format_fields]
  Pack -->|Work_experience_slots| W[Confirm_key_experience_details]
  Pack -->|Preference_or_requirement| P[Confirm_stated_intent]
  Pack -->|Complete_and_clear| S[Stop_or_light_confirm]

  A --> One[Ask_Single_Confirm_or_Clarify]
  W --> One
  P --> One
  S --> Await([Await_Next_Turn])
  R --> Await
  Ab --> Await
  One --> Await
```

## Pack rules

- Goal is **accurate capture**, not competency digging.  
- Do not re-ask slots already clearly answered.  
- Prefer short confirmations once facts are complete.  
- Re-anchor still uses **last interviewer ask** (often the slot question itself).
