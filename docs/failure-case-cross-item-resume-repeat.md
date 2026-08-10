# Failure case: same resume episode probed across different items

A product / architecture note. Methodology only—no production prompts, no resume schemas.

## Who this is for

You run **per-item** follow-up (each stem has its own probe loop, stop, and closing). You also inject **resume** into the probe so thin oral answers can be invited to a concrete episode. Then candidates say:

> “Didn’t you already ask me about that?”

## What breaks

Under single-item design, each question sees roughly:

- this stem  
- this answer  
- **the full resume** (or a large slice)  
- job / competency context  

Each item independently picks the **JD-best** episode. When one experience strongly matches the posting, **two different stems** may both invite “looking at your resume, that internship…” and dig the same story.

From the **item** view: local optimum.  
From the **session** view: repeated evidence, poor experience, thinner coverage of the rest of the resume.

This is **not** the same bug as source-gating failure (claiming the candidate *just said* a company name that only appears on the resume). Source-gating can be correct while cross-item **re-use** still happens.

## Why prompt-only resume probing hits a wall

Resume-aware follow-up needs a **session ledger**: which episodes were already deepened, on which item, how far.

Current shipping architectures (typed packs / domestic all-type packs) optimize **within one item**. Stop classifiers and closing lines are also **item-scoped**. There is no shared “already dug this episode” memory between questions.

So an exploratory pack that correctly teaches *when* and *how* to say “看简历…” still cannot prevent double-ask without **product state across items**.

## Recognition

| Signal | Likely cause |
| --- | --- |
| Candidate objects that the same job / project was asked again on a later stem | Cross-item resume hotspot, no occupation flag |
| Two items’ probes both cite the same employer / project from resume | Independent JD-best selection |
| Oral answers were thin on both items, so both fell back to resume | Expected under per-item rules; still needs session de-dupe |

## Directions (shape only)

1. **Episode occupation** — after an episode is substantially probed, mark it; later items prefer another segment or only fill gaps.  
2. **Routing policy** — allow deep resume expansion only on designated item types / first matching stem.  
3. **Surface softening** — “from another angle for this question…” — reduces friction but does **not** replace occupation rules.

## Status of related exploration

A domestic **draft** resume-probe pack (campus med-device / pharma sales flavor) was archived as unshipped. Work **parked here**: without cross-item memory in the overall interview system, productizing that pack would re-create this failure mode at scale.

## Related

- Per-item stop / close: [stop-conditions.md](./stop-conditions.md)  
- Probe quality (utterance judge, not session ledger): [followup-quality.md](./followup-quality.md)  
- Turn assembly (generate vs stop): [failure-case-message-assembly-empty-turn.md](./failure-case-message-assembly-empty-turn.md)
