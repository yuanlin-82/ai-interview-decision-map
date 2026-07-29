# Failure case: when one situational follow-up rule overgeneralizes

A field note from situational follow-up design (not a prompt cookbook).

## Author lens (one line)

I’m trained in applied psychology (master’s), with a focus in **MHR** — psychometrics and human resource management — in Beijing Normal University’s MAP program.  
That background shapes how I read this failure: not as “the model asked a clever stress question,” but as **one useful rule applied past the point where the dialogue frame still holds**.

## Who this is for

Builders whose situational agents already “probe deeply,” yet keep producing turns that feel slightly wrong to candidates:

> Stem sets a **shared hypothetical** (same scenario for everyone, job-relevant).  
> Candidate answers in a **legal** way — a plan, a stance, a resonant past episode, or a mix.  
> The agent applies a rule that is right *somewhere* (dig past behavior / probe risk / use “what did you do then”) **to the wrong branch**.  
> Fluency stays high. Trust dips: the interviewer seems not to share the same spacetime.

If your eval only checks “did we deepen the answer?”, overgeneralization often scores green.

## Why situational follow-ups have no single absolute

Situational items ask how the candidate would analyze and act in an **HR-specified** situation — roughly the same stimulus for everyone — to judge a competency.

Unlike behavioral items, **many answer shapes are normal**, not violations:

| Shape | Example vibe | Status |
|-------|--------------|--------|
| Hypothetical plan for this scenario | “I’d adjust my mindset, then learn the new team’s process…” | Legal |
| Past episode that resonated with the scenario | A real transfer / onboarding story sparked by the stem | Legal (usually on-topic) |
| Stance / attitude only | “Stay positive and open” | Legal (thin, but not “illegal”) |
| Mixture in one turn | Plan + feeling + a flash of past | Legal |

So the hard problem is not “reject illegal forms.”  
It is **routing inside a wide legal space** — and keeping the **shared fictional frame** when the candidate is still inside the hypothesis.

Humans do this with common ground / pretence: we treat the scenario as live for the conversation even when tense cues are messy.  
Languages with weaker morphological tense (and everyday speech / ASR / non-native English) make frame slips easier; **English is not immune** either (`I went to my manager…` on a hypo stem can still be hypothetical). Do not assume the problem away because the surface language “has tense.”
## What overgeneralization looks like (two slides off the line)

### 1) A past-probe rule applied to a hypothetical plan

Composite pattern (anonymized):

1. Stem: you are moved to an unfamiliar team — **how would you** adapt?  
2. Candidate: a full **plan** (mindset, read docs, observe how colleagues work…). No past-event anchors.  
3. Agent: “You mentioned learning by observing colleagues — **at that time**, how exactly did you observe?”  
4. Candidate hears: the AI thinks this already happened.

The rule “dig concrete behavior / ask what you did then” is often right **when a real past episode was offered**.  
Overgeneralized to a situational plan, it breaks the hypothetical frame. Candidates describe this as the interviewer “not understanding human speech.”

**Caution:** this is an **observed product failure class**, amplified when prompts globally encourage past-tense probes or “prefer real behavior” without a hypo gate. It is **not** claimed here as an inevitable unconstrained LLM law (unlike the stronger behavioral “I would” chat-along tendency).

### 2) A boundary-probe rule applied without a brake

The goal “reveal capability boundaries” is right for situational depth.  
Overgeneralized into repeated “what if that fails?” on the **same** risk line, it becomes **gotcha probing** — stacking hypotheticals to corner the candidate, not to explore their reasoning. That can **anger** candidates (they feel deliberately tested or mocked), not only confuse them. Trust drops even when the content is “on topic.”

Same family of bug: **a good intent, missing exceptions.**

## Prompt pitfall: you need examples — and examples pull

Situational packs must cover more than one legal branch.  
If candidates may answer with past experience, your examples will include past-framed openings (“When you were…”, “what did you do then”). Those examples are **legitimate** for the past branch.

They also **steer**. One global sample of “ask what you did then,” or one slogan of “prefer real behavior,” can drag hypothetical turns into past tense.  
One slogan of “probe the boundary” can drag structured plans into gotcha probing.

Failure-case writing here is for three things:

1. Name the **difficulty** (wide legal space + shared hypo frame).  
2. Name **trust damage** (wrong spacetime; gotcha probing that can anger candidates).  
3. Warn that **prompts must spell exceptions** — slightly careless absolute rules slip.

## Contract move (implementation-agnostic)

Before probing:

- **Classify** whether this turn is mainly past episode, hypothetical plan/reasoning, thin stance, or a mix — knowing all can be legal.  
- **If hypothetical (or unmarked and still in-scenario):** do not use past-episode probes (“what did you do then”, “at that time, how did you…”). Stay in the shared hypo frame; deepen reasoning or first concrete action.
- **If a clear resonant past episode:** dig the experience (unless truly off-topic). Do not yank back to the stem just to “stay situational.”  
- **If probing risk/constraints:** limited use, then **shift dimension** — no chained failure-stacking on one line.  
- **One open question per turn.**

Spoken copy stays private. The publishable decision is: **every simple situational rule needs branch gates; overgeneralization is the failure mode.**

See also: [situational map](../maps/situational.md).

## Why developers should care

- **Trust / brand:** spacetime slips feel like competence failure of the interviewer; gotcha probing can **anger** candidates and read as deliberate cornering — both hurt brand even when wording is polite.  
- **Eval honesty:** “deeper follow-up” ≠ correct frame. Score branch fit and non-adversarial depth.  
- **Fat prompts:** more “be a great interviewer” prose will not fix a missing exception. Examples that teach the past branch must be fenced so they do not colonize the hypo branch.  
- **Content ↔ eng:** this is where assessment intent (same scenario, capability signal) meets dialogue-state maintenance — a seam, not a turf war.

## What this perspective adds

Engineering often asks: *Did we apply our situational probe policy?*  
A content lens asks: *Did we apply the policy that matches this turn’s legal shape — without breaking the shared hypothetical world?*

For `situational`, a fluent probe that overgeneralizes one good rule is still a failed turn.

If your logs show polite “what did you do then?” on pure plans, or what-if ladders under “boundary probing,” check **exceptions and branch gates** before you check the model.
