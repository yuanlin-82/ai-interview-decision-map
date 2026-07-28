# Failure case: digging into complaints about a former employer on job-transition questions

A field note from job-transition / stability follow-up design (not a prompt cookbook).

## Author lens (one line)

I’m trained in applied psychology (master’s), with a focus in **MHR** — psychometrics and human resource management — in Beijing Normal University’s MAP program.  
That background shapes how I read this failure: not as “the model asked one more useful detail,” but as **curiosity defeating a safety boundary** that experienced interviewers treat as non-negotiable.

## Who this is for

Builders whose agents already “stay on topic” when candidates explain why they left a job — and therefore keep digging:

> Stem asks about a career move or reasons for leaving.  
> Candidate volunteers frustration with a **former employer**, manager, or team.  
> The agent follows the drama: what went wrong, how bad was it, what did they do…  
> The session may feel candid. It may also feel like an interrogation — and become a **complaint risk**.

Experienced HR often already know the soft skills here.  
What AI interview systems frequently lack is an explicit **type-specific boundary** that outranks generic “deepen the answer” logic.

## Why this is a boundary bug, not a depth bug

Job-transition questions exist because hiring teams care about **decision drivers** and longer-term fit — not because the product should prosecute past workplaces.

So the useful object is roughly:

| Object | Example shape | Job-transition value |
|--------|---------------|----------------------|
| Decision drivers / values | What they optimized for; how they weighed options | High — what the item can legally collect |
| Future-seeking / environment fit | What they want next; where they work best | High — and safer when negativity appears |
| Blow-by-blow of former-employer harm | Who was wrong; how toxic; more grievance detail | Low for assessment — high for friction / complaint |

A careful human interviewer usually **stops digging the grievance** and pivots: what kind of environment or management helps them do their best work; what they were looking for next.  
An unconstrained LLM tends to treat complaint narrative as rich context and **rewards it with more probes**.

That is not empathy. That is **letting curiosity beat the safety lane**.

## What we observed (composite pattern)

Anonymized, composite — no production wording:

1. Stem: why leave / how they decided to move.  
2. Candidate: constructive start, then names a former boss or culture problem.  
3. Agent: asks for more about that conflict or failure of the former workplace.  
4. Candidate becomes defensive — or keeps venting — while the thread never returns to **decision quality** or **future fit**.

Misread as success: the agent “explored honestly,” “got real motivation,” “didn’t change the subject.”  
Useful diagnosis: once former-employer negativity appears, **normal probing should lose priority** to a boundary action.

Related edges in the same family (same control problem, different trigger):

- Candidate hears the question as a loyalty accusation (“Are you saying I hop too much?”).  
- Candidate refuses to discuss a specific exit.  
- Candidate cites personal non-work reasons and signals discomfort.

Those are not “empty answers.” They are **safety / consent edges**. Fixed recovery beats clever follow-up.

## Contract move (implementation-agnostic)

On `job-transition` turns, when former-employer (or manager/colleague) complaints appear:

- **Judgment:** type safety trigger — not “great detail, deepen.”  
- **Action class intent:** redirect away from grievance detail toward **environment fit / future seeking** (see [job-transition map](../maps/job-transition.md) → `Type_Specific_Boundary`).  
- **Do not** ask for more negative specifics about the former workplace.  
- On the normal pack (when the answer stays constructive), pick **one** family per turn: decision process, values, or patterns across moves — still non-judgmental; prefer *how/what* over accusatory *why*.

Spoken copy stays private. The publishable decision is: **boundary actions outrank curiosity probes**.

## Why developers should care

- **Complaint / brand risk:** voice agents that “go deep” on former-employer blame can feel invasive even when the stem was legitimate.  
- **Eval honesty:** “on-topic deepening” is the wrong metric here. Score whether the **boundary fired**.  
- **Fat prompts:** “be respectful” prose will not reliably override a generic deepen-the-story policy. The router needs an explicit override edge.  
- **Seam with HR knowledge:** mature interviewers already carry this soft skill; engineers shipping multi-turn agents often rediscover it only after an ugly dialogue. Publishing the edge makes the soft skill **auditable**.

## What this perspective adds

Engineering often asks: *Did we get more information about why they left?*  
A measurement / interview-content lens asks: *Did we collect decision-driver evidence without prosecuting a former workplace?*

For `job-transition`, fluent digging into complaints about a former employer is still a failed turn — even when the candidate volunteered the drama and the model stayed polite.

If your logs show warm, on-topic deepening of employer grievance, check the safety boundary before you check the model.
