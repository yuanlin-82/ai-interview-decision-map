# Failure case: off-locale tokens in English follow-ups

A field note for builders of **English-locale** interview follow-ups—not a prompt dump, and **not** a claim about every language track.

## Scope (read this first)

This note is grounded in **English** spoken probes where a **non-English** fragment appeared mid-sentence (composite shape below).

Do **not** treat the same mix (or its reverse) as established for other target locales unless you measure those tracks. Locale risk is real; **this morphology and fix story are English-track findings**.

## Who this is for

You already require **English-only** for spoken follow-ups. Constraints look clear. Then a turn appears that is *almost* right:

> `what具体 steps did you take…`

Mostly English. One foreign fragment welded into the scaffold. Rare enough to feel random; sharp enough to break the English-track promise.

## Why this is surprising (on English)

Teams often assume: “If the prompt already says English-only, locale is solved.”

What showed up was often **not** paragraph code-switching. It was a **high-frequency probe lemma** from another-locale interview wording for “specific / concrete,” sitting where English would use *specific / concrete / exactly*.

Easy misread: “the model ignored the language rule.”  
Better read: **lexical gravity** on a long, strategy-heavy English pack authored beside other-locale siblings—especially when **non-English tokens still sit in the instruction surface** (including placeholder names)—can pull that lemma into generation even under an explicit purity line.

## Separate from process leak

| Failure | What enters speech | Typical look |
|---------|-------------------|--------------|
| [Process leak](./failure-case-process-leak.md) | Same-locale analysis / stages / handbook asides | Parentheses, numbered steps, “why we ask” |
| **This note** | **Non-English** characters/words inside an otherwise English question | Mid-phrase foreign token; sometimes glued with no space |

Same umbrella (`fail_surface`). Different logs and fixes. Do not merge metrics. Do not assume the same leak on other language tracks without evidence.

## Easy misread vs useful read

| Easy misread | Better read |
|--------------|-------------|
| “Model can’t speak English” | English purity lost to a **habit word** from parallel packs |
| “Every locale will mirror this” | **Unproven**—re-check per target language |
| “Need a longer English-only paragraph” | Strengthening bans helps less than **shrinking** the live English pack and removing off-locale tokens from the **template itself** |
| “Same as thinking aloud” | No worksheet—just a wrong-language fragment |
| “Candidates still understand” | TTS and the English-only promise still fail |

## What tended to help on the English path (contract-level)

Without publishing production wording:

1. **Simplify the live English pack** — state handling + one typed strategy spine; cut meta layers that compete with the locale rule.  
2. **Keep the English instruction surface clean** — including slot / placeholder spellings in the prompt text, not only the “never output non-English” sentence.  
3. **Explicit script bans** (e.g. no non-Latin script in English output) as a hard fail—useful, incomplete alone if the pack stays fat and template-contaminated.  
4. **Gate in eval** for the English track: off-locale tokens = fatal in [followup-quality](./followup-quality.md) compares.  
5. Live **generators** stay fast; offline **judges** can be stronger—neither replaces a locale check on the speakable string.

## Why developers should care

- English **TTS** stumbles on mixed script.  
- International **go/no-go** often treats language fatals like hallucination.  
- **Debug English logs** for mixed script before re-litigating STAR strategy.  
- **Don’t copy the scare story wholesale** to other locales without sampling those tracks.

## One-line summary

> On **English** follow-ups, purity lines are not enough if the live pack is heavy and still contains other-locale tokens—gate single habit-word leaks as hard fails; **don’t assume the same pattern on other target languages.**
