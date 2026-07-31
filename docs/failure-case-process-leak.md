# Failure case: process text leaking into the spoken probe

A field note for builders of **voice** interview agents—not a methods paper.

## Who this is for

You already route and probe reasonably well, yet logs show turns that would **fail if read aloud**:

> Analysis, stage labels, handbook-style judgments, or “why we ask this” appear in the string sent to TTS.

Strategy eval can still look green. The product fails on **form**.

## What actually breaks

In selection interviews, standardization is not only “same stem for everyone.” It is also **procedural equivalence of the interaction surface**: candidates should not randomly witness the interviewer’s internal worksheet.

Human interviewer bias is usually **who** gets different treatment.  
Process leak is often **instrument form instability**: the same pack occasionally emits non-role text, so dialogues are no longer comparable as a measurement procedure—even when the embedded open question is on-target.

Useful split:

| Still true | Still a fail |
|------------|--------------|
| Probe targets the right evidence gap | Candidate can hear the rubric / checklist |
| Wording could work as a human ask | Role breaks (aside, tag, third-person “the candidate…”) |

Do **not** score this as “bad follow-up strategy.” Score it as **speech-channel contamination**.

## How to recognize it (composites)

Patterns that are unspeakable in a live interview:

- Parenthetical or labeled **asides** (inner summary, STAR checklist residue) before the real ask  
- **Numbered internal stages** left in the utterance (“extract → design → output”)  
- **Format tags** (`Follow-up:`, “final output:”, similar) glued to an otherwise fine question  
- **Handbook voice**: classifying the answer with internal labels, then asking—as if reading an ops manual  
- **Instruction echo**: stock phrases that sound copied from authoring instructions, not from an interviewer  
- Trailing **author notes** the candidate was never meant to hear  

Opposite (usually **not** this failure): ordinary interviewer pushback in plain talk—“that stayed pretty high-level; can you give one real example?”—without checklist jargon or stage scaffolding.

Wrong-locale tokens inside an on-locale probe are a **different** `fail_surface` class. The documented field note covers **English** probes with non-English fragments—not a claim about other target languages: [failure-case-language-locale-leak.md](./failure-case-language-locale-leak.md).

## Easy misread vs useful read

| Easy misread | Better read |
|--------------|-------------|
| “Model is too verbose / too smart” | Non-speech stages reached the **visible channel** |
| “Add another don’t-think-aloud line” | Stacking bans helps less than changing what the live pack **forces the model to narrate** |
| “Post-filter it” | Real-time TTS often has **no** rewrite slot after decode; prevention must fit **one** generation call |
| “Fix with a stronger model only” | Same pack can behave differently across deploy paths—**validate on the model you actually ship** |
| “Strategy judge will catch it” | Strategy judges reward the question; they under-weight **speakability / role purity** |

## Contract (implementation-agnostic)

1. **Speech payload** = only what an interviewer would say to the candidate (one open question; optional short cushion if product allows).  
2. Rubrics, state notes, and “why this probe” stay off TTS (hidden fields, tools, offline judges).  
3. Prefer generation designs that do not require the **live** model to recite a checklist when instruction-following is imperfect; if you keep step-wise authoring aids, add **blocking checks** before play.  
4. In release compares, treat process leak as a **hard fail** for voice—not a minor wording flaw. See [followup-quality.md](./followup-quality.md).  
5. Keep live generators on the **fast** path; long CoT on the speakable turn is not the product fix.

## One-line summary

> A probe can be strategically right and still fail standardization if the candidate can hear the worksheet—gate **role-pure speech**, not only “good question content.”
