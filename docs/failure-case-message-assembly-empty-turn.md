# Failure case: how you assemble turns changes what the model does

A small product pitfall with large effect. Methodology only—no production prompts, no wire formats.

## Who this is for

You split **probe generation** from a **stop classifier** (generate vs brake). Both read “the dialogue so far,” but if you feed them the **same assembly shape**, or you **omit empty candidate turns**, quality drops in ways that look like strategy bugs and are not.

## Two jobs, two assembly contracts

| Job | What the model is doing | Assembly that fits |
| --- | --- | --- |
| **Probe generation** | Stay in role: produce the **next interviewer utterance** | **Turn-by-turn** chat messages (interviewer / candidate alternating as separate turns) |
| **Stop / continue** | Read a **record** and decide a label (`stop` / `continue`) | **One user payload**: a plain transcript (`Interviewer: …` / `Candidate: …`), not a multi-turn role-play |

Generation benefits from turn-splitting: across multi-turn items, the model more easily keeps **interviewer voice** and “what comes next.”

Stop benefits from a single transcript block: the model sees **evidence for a judgment task**, and is less likely to confuse itself for the speaker who should ask the next question.

Do not reuse one packing recipe for both just because both “need history.”

## What breaks when the candidate is silent

Silence is the stress test. Early products often **dropped** the candidate slot when there was no ASR text—no empty user turn for generation, and no `Candidate:` line (not even “no answer”) for stop.

### Generation without a candidate turn

History looks like interviewer-only (or a missing `user` after the last ask). The generator still behaves as if the other party just spoke:

- Hallucinated anchors: *“You just said …”* / *“Earlier you mentioned …”* when nothing was said  
- Probes that assume content that never existed  

The empty-answer **strategy** in the prompt may be correct; the **slot is missing**, so the model invents a prior turn.

### Stop without a visible candidate line

The transcript shows only interviewer lines back-to-back. The classifier cannot see **silence as evidence**:

- Hard to judge **willingness / non-response**  
- Easy to misread as “the interviewer kept talking,” not “the candidate did not answer”  

Omitting the row is not the same as encoding empty. The record must **occupy the candidate turn**—e.g. an explicit no-answer / empty marker in the transcript convention you use—not vanish.

## How to recognize it

| Symptom | Likely assembly issue |
| --- | --- |
| Probe cites words the candidate never produced, especially after mute / skip | Missing candidate turn on the **generator** path |
| Stop looks unstable on no-speech / timeout empties; fine on short real answers | Missing or blank-omitted candidate lines on the **stop** path |
| Generator drifts out of interviewer voice in long items when history is one blob | Generation fed a **single pasted record** instead of turn messages |
| Stop classifier starts “replying” like an interviewer | Stop fed **multi-turn role messages** instead of one judgment transcript |

## What to change (shape only)

1. **Different assemblers** for generate vs stop—same underlying events, different views.  
2. **Always emit the candidate slot** when that turn occurred, including silence / invalid / no ASR.  
3. Keep evaluating strategy and stop rules **after** assembly is honest; otherwise you debug the wrong layer.

## Related

- Stop layer (when to end probing): [stop-conditions.md](./stop-conditions.md)  
- Probe quality (judge the utterance, not the wire format): [followup-quality.md](./followup-quality.md)  
- Empty / invalid answer routing in type maps: overview + typed packs in [maps/](../maps/)
