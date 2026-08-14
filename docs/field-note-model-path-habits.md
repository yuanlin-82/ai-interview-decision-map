# Field note: surface habits when the follow-up **path** changes

Objective observations for builders who swap **model config × prompt package** on the same interview follow-up contract—not a vendor ranking, not a benchmark paper.

## Stance / disclaimer

- Compare **deployable paths**, not “which model is smarter.”
- Names below are **illustrative observations**: where a habit showed up often enough in our work that the pack or gate had to react. They are **not** judgments that a brand is “bad,” unfit, or worse overall.
- **No version pins** (no “v3.2 / flash / dated snapshot”). Families drift; a version table ages into false precision. Re-validate on the path you ship.
- Do **not** read frequencies or anecdotes as leaderboard scores.
- Still private: production prompt bodies, internal rates, customer logs ([NOTICE.md](../NOTICE.md)).

Related: [followup-quality.md](./followup-quality.md) (path A vs B gates) · [failure-case-process-leak.md](./failure-case-process-leak.md) · [failure-case-language-locale-leak.md](./failure-case-language-locale-leak.md) · [principles.md](./principles.md) (silent ASR repair).

## Who this is for

You already have a stable routing contract, yet after a **model swap** or a **pack rewrite** the logs change shape:

> Same stem and answer; the next spoken turn fails for different *surface* reasons—or stays clean for reasons that do not generalize to the next vendor.

## Habit families (observable, not ranked)

These are **families of tendency**, not “Model X is worse than Model Y.”  
**Seen more on…** lines name families only as memory aids for pack design.

### 1. Process / worksheet leaking into TTS

**What you see:** asides, stage labels, checklist residue, “Follow-up:” tags, handbook voice—strategically on-target questions that are unspeakable.

**Often co-occurs with:** authoring packs that force the live generator to **narrate intermediate steps** (extract → design → output) instead of emitting only the interviewer utterance.

**Seen more on (illustrative):** step-sequenced authoring packs with **DeepSeek** and **Doubao** generators—process text reaching TTS more readily than on some other paths under the same stress. Read as pack×model interaction, not “these brands cannot interview.”

**Read as:** instruction-following under a narrate-the-worksheet load—not as “the model is too analytical.” Detail: [failure-case-process-leak.md](./failure-case-process-leak.md).

**Contract implication:** prefer generation shapes that do not require live recitation of a checklist; treat process leak as a **hard fail** for voice; validate on the **shipped** path.

### 2. Over-clarifying ASR noise and small slips

**What you see:** the probe stops digging for competency evidence and instead asks the candidate to **clarify wording**—as if a recognition glitch or a small slip were the interview topic. Typical shape: “What do you mean by ×××?”, “When you said ×××, what did you mean?”, “Could you explain what ××× refers to?”

**Often co-occurs with:** strong literal reading of messy ASR; weak separation between **language form** and **competency content**.

**Seen more on (illustrative):** **Qwen**-oriented live packs—enough that explicit “do not quiz recoverable ASR / slips” constraints became part of the path fit. Again: a habit to design for, not a brand verdict.

**Read as:** a surface habit that collapses dialogue and burns turns—not as “careful interviewing.”

**Contract implication:** silent repair when intent is recoverable; never turn recoverable noise into the interview topic ([principles.md](./principles.md)). Packs for paths that over-clarify need explicit bans **and** eval that scores this as fatal for experience.

### 3. Locale / habit-token leaks on an otherwise on-locale probe

**What you see:** mostly correct locale, with a habitual word or fragment from another pack language glued in (English-track example family: [failure-case-language-locale-leak.md](./failure-case-language-locale-leak.md)).

**Often co-occurs with:** parallel multi-locale packs and high-frequency “scaffold” words from the author’s dominant language.

**Seen more on (illustrative):** **GPT**-family English probes with occasional Chinese habit tokens glued into an English skeleton (small, local leaks—not whole-turn language failure).

**Read as:** surface purity failure—not “the model cannot speak the target language.”

**Contract implication:** gate speakable locale purity separately from strategy quality.

### 4. Length: vague brevity vs an explicit character suggestion

**What this habit is about:** the same follow-up task under **fuzzy style language** (“be brief / keep it short”) versus an **explicit length suggestion in the prompt** (e.g. “preferably no more than 150 characters”). The point is not which brand is better—it is that **the two framings land differently on different generator families**.

**What you see:**

1. **Vague brevity only** (no number): default length still differs by path. In our compares with **both** paths unconstrained by a numeric ceiling, **GPT**-family probes tended to run **longer** than **Qwen**-family probes.  
2. **Explicit suggestion** (worded as guidance such as “preferably ≤150 characters,” not a hard system truncate): distributions tighten, but not equally. On a Japanese-track supplement, a **Qwen** path that was long and high-variance under descriptive “be brief” alone became shorter once the 150-character suggestion was added; under that same style of numeric suggestion, **GPT** outputs looked **more concentrated** (tighter spread) than **Qwen**. Exact means stay private.

**Often co-occurs with:** TTS turn-time budgets, and packs that only praise brevity without a measurable target.

**Seen more on (illustrative):** soft adjectives alone → family-level length gaps (**GPT** longer than **Qwen** when both lack a number); add a **numeric suggestion** → both move, with **GPT** often clustering more tightly than **Qwen** under the same wording. Do not treat “150” as a universal standard—ceilings are product-local.

**Read as:** underspecified style vs specified budget—different compliance shapes—not a model quality ranking.

**Contract implication:** if speak time matters, prefer an **explicit length target in the pack + eval**, not adjective-only “keep it short”; re-check concentration after each generator swap.

### 5. Same pack, different habit mix across deploy paths

**What you see:** Path A and Path B share wording goals; one path spills process text, another over-clarifies ASR, a third stays role-pure more often. Blocking checks that help one path barely move another.

**Seen more on (illustrative):** any **swap of the live generator family** while holding the decision contract fixed—the dominant surface habit can change even when strategy intent does not.

**Read as:** **path-dependent surface risk**—not a permanent trait of a brand.

**Contract implication:** release compares must run on the model+pack you will actually serve; do not borrow green lights from a different path ([followup-quality.md](./followup-quality.md)).

### 6. Generator vs judge are different jobs

**What you see:** a fast live generator that is good enough for TTS turns; a separate stronger reasoner that is better at offline probe compares—and a stop brake that must stay fast enough to intercept.

**Seen more on (illustrative):** product practice across vendors—not tied to one name. Live follow-up and stop stay on a **fast** path; offline quality compares may use a **stronger reasoning** config.

**Read as:** role split—not “one model should do everything.”

**Contract implication:** do not reuse deep-thinking configs as the live stop brake; do not reuse the live generator as the only quality judge ([followup-quality.md](./followup-quality.md), [stop-conditions.md](./stop-conditions.md)).

## Easy misread vs useful read

| Easy misread | Better read |
|--------------|-------------|
| “These names are a ranking” | They are **where we noticed the habit** while fitting packs |
| “Pick the smartest model and freeze the pack” | Surface habits are **path** effects; freeze the **contract**, re-fit the pack |
| “Publish a vendor scorecard from one bake-off” | One bake-off ages out; keep **habit families** and gate order |
| “More bans in the prompt always transfer” | Transfer the **fail classes**; ban wording is path-local |
| “Strategy green ⇒ ship” | Strategy can pass while speakability / ASR-clarification / locale purity / length budget fail |
| “Soft ‘be concise’ is enough” | Fuzzy brevity and an explicit character suggestion land differently by path; check length **and** concentration |

## What this note is not

- Not win rates, Elo, version bake-offs, or “recommended model for follow-ups”
- Not a claim that DeepSeek, Doubao, Qwen, or GPT are unfit for interview dialogue
- Not permission to paste production prompts or internal A/B tables
- Not a claim that any vendor is unfit for task-style instructions in general—only that **some pack shapes** stress instruction-following in ways that show up as the habit families above

## One-line summary

> When follow-up paths change, watch **which surface habit moves**—process leak, ASR over-clarification, locale fragments, **length under vague vs numeric guidance**—and treat named families as **observation anchors for pack design**, not as a ranking; keep the decision contract stable and re-validate speakability on the path you ship.
