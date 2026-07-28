# Overview Decision Map

Shared router for all question types. **Action-level** only (no utterance copy).

## Design choices (locked)

| ID | Choice |
|----|--------|
| A | `Off_Topic` and `Forgot_Or_Misheard` merge into judgment **`Need_Question_Reanchor`**; action **`Restate_Last_Interviewer_Question`**. Type maps may still tune tone. |
| B | Type-specific safety/boundary appears once as **`Type_Specific_Boundary`** (dashed). Details live in type maps / matrix. |
| C | Normal path ends at **`Strategy_Pack(type)`** on this overview. Common probe families are documented in principles / type docs, not exploded here. |

## Diagram

```mermaid
flowchart TD
  Start([Turn_Start]) --> J1{Substantive_anchor_present?}

  J1 -->|No / flow-meta / broken| Abnormal[Abnormal_Router]
  J1 -->|Yes| Normal[Normal_Router]

  Abnormal --> J2{Judgment}

  J2 -->|Need_Question_Reanchor<br/>forgot / misheard / off-topic family| A_Restate[Restate_Last_Interviewer_Question]
  J2 -->|Empty_Or_Fillers| A_Soft[Soft_Reinvite]
  J2 -->|Unintelligible| A_Repeat[Ask_Repeat_Utterance]
  J2 -->|Quit_Skip_Hint_Challenge_NonLang_Contradiction_…| A_Fixed[Boundary_Or_Fixed_Recovery]
  J2 -->|Type safety trigger| A_Type[Type_Specific_Boundary]

  A_Restate --> Await([Await_Next_Turn])
  A_Soft --> Await
  A_Repeat --> Await
  A_Fixed --> Await
  A_Type --> Await

  Normal --> Pack[Strategy_Pack type]
  Pack --> OneQ[Ask_Single_Open_Question]
  OneQ --> Await

  classDef dash stroke-dasharray: 5 5
  class A_Type dash
```

## Node catalog

### Judgments

| Node | Fires when (conceptual) |
|------|-------------------------|
| `Substantive_anchor_present?` | Latest turn yields at least one usable meaning anchor for probing; not pure meta/empty/gibberish/re-anchor demand |
| `Need_Question_Reanchor` | Candidate forgot/misheard the ask, or the turn requires putting “the question” back on the table (includes many off-topic recoveries) |
| `Empty_Or_Fillers` | No content beyond silence/fillers/“I don’t know” without elaboration |
| `Unintelligible` | No recoverable intent even after silent ASR interpretation |
| `Boundary_Or_Fixed_Recovery` bucket | Quit, skip, hint-seeking, interviewer challenge, language violation, hard contradiction, etc. (may be expanded in implementation) |
| `Type_Specific_Boundary` | Type pack declares a safety/ethics override (e.g. non-work trauma on resilience items) |

### Actions

| Node | Meaning |
|------|---------|
| `Restate_Last_Interviewer_Question` | Re-place the **last question asked by the interviewer in this dialogue** (stem only if it was last), then invite answer |
| `Soft_Reinvite` | Invite contribution without inventing premises |
| `Ask_Repeat_Utterance` | Ask the candidate to say their answer again — **not** the same as restating the exam question |
| `Boundary_Or_Fixed_Recovery` | Constrained recovery / boundary move (implementation-private wording) |
| `Type_Specific_Boundary` | Type override (see matrix) |
| `Strategy_Pack(type)` | Select exactly one probe family allowed for this type |
| `Ask_Single_Open_Question` | Emit one open question; end turn |
| `Await_Next_Turn` | Wait for next candidate utterance |

## Critical edge note

`Restate_Last_Interviewer_Question` **must not** be hard-wired to the original stem variable.  
Stem injection remains valid context for *what the thread is about*; re-anchor content comes from dialogue history of interviewer asks.

## Related

- Type matrix: [../docs/question-types.md](../docs/question-types.md)  
- Type maps: [behavioral](./behavioral.md), [situational](./situational.md), [career-choice](./career-choice.md), [job-transition](./job-transition.md), [opening-intro](./opening-intro.md), [fallback](./fallback.md)  
- Principles: [../docs/principles.md](../docs/principles.md)
