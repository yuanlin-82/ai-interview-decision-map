# Failure case: restating the stem instead of the last ask

A short field note from dialogue design (not an engineering postmortem).

## Who this is for

Builders who already have a turn loop and prompts, but keep seeing the same brittle moment:

> Mid-interview, the candidate asks “What was the question again?”  
> The agent repeats the **original stem** — and the conversation quietly derails.

If you have ever shrugged this off as “the model being dumb,” this note argues it is usually a **decision-contract bug**, visible first in interaction patterns, then in variables and routing.

## The psychology angle (why content people catch this early)

In a live interview, “the question” is not a document ID. It is whatever the candidate is currently trying to answer in working memory.

After a follow-up, that object has usually changed:

| Moment | What “the question” means to the candidate |
|--------|--------------------------------------------|
| Right after the stem | The stem |
| After your first probe | **Your probe** |
| After two probes | **The latest probe** |

Candidates are not trying to recover your item bank. They are trying to re-enter the **current demand** you placed on them.  
When we restate the stem, we ask them to abandon the thread they were holding — often after they already invested an answer. That feels dismissive or confusing, even when the wording is polite.

This is ordinary turn-taking psychology. It becomes a product bug when the system hard-binds “restate” to a stem injection variable.

## What we observed in multi-turn follow-ups

Composite pattern (anonymized):

1. Stem: teamwork conflict.  
2. Agent asks a judgment probe (last ask).  
3. Candidate: “Sorry, what was the question?”  
4. Agent restates **the stem**.  
5. Candidate restarts the whole story — or answers the stem again and never returns to the judgment gap.

Eval notes that only score “did they restate something?” will mark step 4 as success.  
Dialogue quality still drops: the probe’s intent is lost; latency budget is spent on a reset; trust dips because the interviewer seems not to track the last beat.

## Misdiagnosis vs useful diagnosis

| Easy misread | Better read |
|--------------|-------------|
| “Model ignored instructions” | Re-anchor action was **defined** as stem |
| “Need a smarter LLM” | Need a clearer **referent** for “the question” |
| “Add more examples of restating” | Change the **decision**: last interviewer ask in dialogue |

Prompt polish without changing the referent often fails: the stem is still the most salient injected string in context, so models gladly use it.

## Contract fix (implementation-agnostic)

When the judgment is `Need_Question_Reanchor`:

- **Action:** restate the **last question the interviewer asked in this dialogue**.  
- **Stem** is correct only when it *was* that last ask (typical early turns).  
- Keep stem in task context for *what the thread is about*; do not use it as the default re-anchor payload.

How you fill that slot (history lookup, model-filled placeholder, cached last agent utterance) is an engineering choice.  
The **decision** is what must be shared across languages and vendors.

See also: [overview map](../maps/overview.md) → `Restate_Last_Interviewer_Question`.

## Why developers should care (even if they don’t write the copy)

- **Latency:** a wrong re-anchor often costs an extra full turn under TTS budgets.  
- **Eval honesty:** “restate accuracy” must mean *correct referent*, not *any question-shaped sentence*.  
- **Fat-prompt limits:** if recovery is wired to the wrong variable, more wording will not save you.  
- **Cross-skill handoff:** engineers own the slot; dialogue designers own the referent rule. The bug sits on the seam.

## What this perspective adds

A psychology / interview-content lens does not replace state machines. It names **which human object** the machine must track when language is ambiguous (“the question,” “just now,” “again”).

That object is usually **the last demand the interviewer made** — not the item that started the thread.

If your logs show polite restates that still reset the candidate, check the referent before you check the model.
