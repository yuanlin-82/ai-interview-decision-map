# Field note: Difficulty mismatch on the Normal lane

Methodology only. No production prompts, no customer transcripts, no PII.

## Symptom

The overview router classifies the turn as **Normal** (not quit / empty / forgot). The generator then runs a typed deepen (STAR gap, criteria, “why,” stacked specifics). The candidate hears:

- the same ask in new clothes (**synonym re-ask**),
- a jump to premises / standards before the what is clear (**abstraction uplift**),
- or several tasks in one turn (**multi-ask**).

Experience drops even when the **information goal** of the probe is legitimate.

## Wrong diagnosis

| Tempting story | Why it misleads |
|----------------|-----------------|
| “Model was impolite” | Tone is usually fine; the **difficulty** is wrong |
| “Every later turn gets harder” | Population risk can rise later; **not** every dialogue. There is often **no round index** in the system |
| “Put thin answers in Abnormal” | Thin / long-empty talk is still a valid answer; hard-abnormal recovery tone is the wrong tool |

## Contract gap this note points at

Overview used to be:

```text
Substantive? → Yes → Strategy_Pack(type) → one question
```

That skips **answer-depth banding**. Typed packs optimize “what evidence is missing,” not “how hard this ask should be given what they just produced.”

## Fix shape (now on the overview)

Inside **Normal**:

| Band | Action class |
|------|----------------|
| Short low-info | `Supportive_Scaffold` |
| Long low-info | `Pin_Concrete_Episode` |
| Fact anchor | `Strategy_Pack(type)` (optional) |

Companion rules: one open question; no synonym re-ask of an already-answered point; if avoidance left a goal unmet, return with a **clearer / easier** ask.

Principles: [principles.md](./principles.md) §10a–10b, §12.  
Map: [../maps/overview.md](../maps/overview.md).

## Eval hint

When comparing probes, treat **difficulty above current answer depth** as a clear flaw on **candidate experience** (and often on **understanding**), even if strategic intent looks right. See [followup-quality.md](./followup-quality.md).
