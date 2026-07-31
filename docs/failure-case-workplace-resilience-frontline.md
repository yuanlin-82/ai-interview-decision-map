# Failure case: mistaking blunt workplace answers for “no intent” on resilience items

A field case note from **workplace-resilience** follow-up design in a **frontline / home-service hiring** setting (not a prompt cookbook).  
Same type key as [`workplace-resilience`](../maps/workplace-resilience.md)—a population variant, not a new question type.

## Author lens (one line)

I’m trained in applied psychology (master’s), with a focus in **MHR** — psychometrics and human resource management — in Beijing Normal University’s MAP program.  
That background shapes how I read this failure: not as “the model was too harsh,” but as **losing scorable work-stress evidence by over-policing register**.

## Who this is for

Builders whose resilience agents already “stay gentle,” yet keep producing empty dialogues with frontline candidates:

> Stem sets a **work pressure / customer conflict / overload** scene.  
> Candidate answers in a **short, blunt, even coarse** way that still shows a stance or coping move.  
> The agent treats the turn as unintelligible or “not yet answering,” and asks for a soft “first thought.”  
> Or it invents feelings (“you sound angry…”) from ASR debris.  

Fluency and “care” look high. The assessment goal—**how they handle workplace stress**—quietly dies.

## Why this sits under `workplace-resilience`

The construct is still work-framed stress handling (emotion, action under load, learning)—not clinical mental health, not a separate “service worker” type.

What changes in this population is mostly **surface and lane judgment**:

| Dimension | Office-default risk | Frontline / home-service risk |
|-----------|---------------------|-------------------------------|
| Register | Over-formal is awkward | Over-soft recovery feels patronizing |
| Short blunt answers | Often read as thin | Often **are** the attitude / coping move |
| ASR / dialect / fillers | Mild noise | Higher rate of “ugly but recoverable” turns |
| Invented affect | Still wrong | Especially damaging when speech is clipped |

Same pack, tighter **hearable vs unhearable** gate.

## What we observed (composite pattern)

Anonymized, composite—no production wording, no client copy:

1. Stem: client / household pushes hard; candidate must respond under pressure.  
2. Candidate: one rough directive or refusal (“get lost”-class stance)—short, on-topic, attitude-clear.  
3. **Fail A — false abnormal:** agent asks to “share an initial thought,” as if nothing related was said.  
4. **Fail B — invented affect:** ASR is messy; agent narrates emotions the candidate never voiced.  
5. **Fail C — stem leakage:** agent talks as if the candidate already lived every detail written in the item stem.

Misread as success: the agent “protected rapport,” “didn’t escalate,” “sounded empathetic.”  
Useful diagnosis: the agent never entered the **normal resilience pack** (probe emotion-handling **or** action logic **or** work-context learning) on a turn that already carried a work-stress stance.

## Content contract that prevents the fail

Judge **related intent first**, register second:

| Candidate shape | Correct lane | Wrong move |
|-----------------|--------------|------------|
| Blunt but on-topic coping / stance | Normal → one resilience avenue | Soft “any first idea?” recovery |
| Empty / pure noise / off-topic with no recoverable work intent | Abnormal recovery | Digging “how did that feel?” |
| ASR recoverable | Silent repair → normal or abnormal as intent warrants | Asking “did you mean X?” out loud |
| No affect words | Probe action or learning; don’t invent feelings | “You sound frustrated…” |

Still non-clinical: no pathology labels, no advice-giving, no therapy frame—same as the parent type.

## Eval hint

Score **route agreement** before warmth:

- Gold: blunt on-topic stance → normal pack, one open probe.  
- Auto-fail: same turn labeled abnormal-only because of tone.  
- Auto-fail: affect invented from garbage ASR.  
- Auto-fail: stem premises treated as candidate-said.

A “kind” sentence on the wrong edge is still a failed turn.

## Tie-back

- Type map: [workplace-resilience.md](../maps/workplace-resilience.md)  
- Label gates: [label-schema.md](./label-schema.md)  
- Related: inventing premises / wrong evidence channels in [failure-case-behavioral-evidence.md](./failure-case-behavioral-evidence.md)
