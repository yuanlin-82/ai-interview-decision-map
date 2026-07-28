# Failure case: chatting with “I would” instead of grounding in “I did”

A field note from behavioral follow-up design (not a prompt cookbook).

## Author lens (one line)

I’m trained in applied psychology (master’s), with a focus in **MHR** — psychometrics and human resource management — in Beijing Normal University’s MAP program.  
That background shapes how I read this failure: not as “the model wrote a bad sentence,” but as **the wrong kind of evidence entering the interview**.

## Who this is for

Builders whose behavioral agents already sound fluent, yet keep producing dialogues that feel like coaching chats:

> Stem asks for a **past** episode.  
> Candidate answers in **“I would…”** (ideal / hypothetical / impression-managed).  
> The agent politely deepens the hypothetical plan.  
> Everyone leaves with a coherent story — and almost no scorable past-behavior evidence.

If your eval only checks “did the agent ask something relevant?”, this path often scores green while the assessment goal quietly dies.

## Why this is a content bug, not a fluency bug

Behavioral interviewing rests on a simple claim:

> Past personal behavior is evidence of future performance.

So the legal evidence type is roughly:

| Evidence type | Example shape | Behavioral value |
|---------------|---------------|------------------|
| Past personal episode | “I did… / I chose… / I failed and then…” | High — what we came for |
| Hypothetical ideal self | “I would… / If that happened I would…” | Low — often social desirability |
| Collective vague “we” without self | “We always…” with no personal act | Weak / wrong subject |

A live human interviewer usually **refuses to continue the ideal-self chat**. They ask for a real instance (or a similar past episode).  
An unconstrained LLM tends to do the opposite: it treats “I would” as a cooperative narrative and **follows the candidate**.

That is not empathy. That is **losing interviewer control of the evidence channel**.

## What we observed (composite pattern)

Anonymized, composite — no production wording:

1. Stem: past conflict / setback / teamwork episode.  
2. Candidate: polished “I would first… then I would…” with no concrete past scene.  
3. Agent: asks how that plan would work, what they would say next, what if stakeholders resisted — still inside the hypothetical.  
4. Session ends with a tidy future script and **no grounded episode**.

Misread as success: the agent “stayed on topic,” “probed deeply,” “sounded professional.”  
Useful diagnosis: the agent never enforced the **evidence-type gate** that behavioral items exist for.

## Contract move (implementation-agnostic)

When the latest answer is abstract / ideal / hypothetical rather than a past personal episode:

- **Judgment:** still on the normal `behavioral` pack — specifically the **grounding** branch (see [behavioral map](../maps/behavioral.md) → `Ground_in_real_episode`).  
- **Action class intent:** request **one real (or similar) past episode**. Do not deepen the fictional plan.  
- **If none can be elicited:** do not dig forever. Hand off to your product’s **dialogue-termination** policy (separate topic; not expanded here).

What you must *not* do on this edge:

- Treat “I would” as a valid STAR story and keep polishing it.  
- Invent a past premise (“when you led that project…”) the candidate never supplied.  
- Stack multiple demands in one turn.

Spoken copy stays private. The publishable decision is: **no episode → ground; do not accompany the ideal self.**

## Same family, lighter note: interviewer control

“Don’t be led by the candidate” is not only about evidence type.

When candidates try to **change the item**, demand the answer key, or ask for a score mid-thread, a human interviewer typically holds the frame with a standard recovery — they do not negotiate the assessment design in-chat.

That is the same control problem at a coarser grain: **who sets the demand**.  
This note keeps the focus on behavioral evidence. Control / refusal / termination deserve their own field notes later.

## Why developers should care

- **Scoring integrity:** downstream raters cannot fairly score past-behavior dimensions on a pure “I would” chat. Your agent just burned turns on non-evidence.  
- **Eval honesty:** “on-topic follow-up” ≠ “correct evidence type.” Add a check for *episode present / still hypothetical*.  
- **Fat prompts:** more “be a great interviewer” prose will not fix a missing gate. The route must prefer grounding over chat continuation.  
- **Seam with product policy:** grounding has a ceiling; termination is a product decision, not an infinite probe loop.

## What this perspective adds

Engineering often asks: *Did we ask a follow-up?*  
A measurement / interview-content lens asks: *Did we collect the evidence this item is allowed to collect?*

For `behavioral`, fluency without a past episode is still a failed turn — even when the candidate is cooperative and the model is eloquent.

If your logs show warm, on-topic deepening of “I would,” check the evidence gate before you check the model.
