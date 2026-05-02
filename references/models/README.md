# Suno Models — Picker & Per-Model Guides

This directory holds per-model prompting guidance. The skill loads the file matching
the user's `Default Suno model` (from `<backup>/user-profile.md` → `## Identity`)
before drafting the style prompt and lyrics. Per-song overrides are honored —
e.g., the user saying "use v5 for this one."

## Quick comparison

| | v4.5 / v4.5+ | v5 | v5.5 |
|---|---|---|---|
| Released | May 2025 / Jul 2025 | Sept 2025 | Mar 2026 |
| Style char limit | 1000 | 1000 | 1000 |
| First-gen length | 8 min (v4.5+); 4 min + Extend on v4.5 | 8 min | 8 min |
| Prompt format | Hybrid — comma-tags reliable, conversational works | Narrative / NLP-driven | Narrative / NLP-driven (best) |
| Cue reliability | Usable; often dropped on later sections | Reliable across the song | Reliable; cues are an internal control system |
| Audio profile | Polished, broad-appeal | Radio-ready; texture preserved (best for raw/lo-fi) | Softer, quieter, more headroom; stem-friendly |
| Standout features | Add Vocals, Add Instrumentals, Spark from Playlist (v4.5+ Pro) | Studio mode | Voices, Custom Models, My Taste, Replace Section, Open in Studio |

## When to choose what

**Pick v4.5 / v4.5+** when you want predictable comma-tag behavior, fast iteration,
or you're using the Pro-only Add Vocals / Add Instrumentals workflow (start from
just vocals or just an instrumental).

**Pick v5** when you want the best output most of the time. It's the strongest
default for songs that need narrative arrangement direction *and* preserve a raw,
unpolished, or austere texture (lo-fi, dark ambient, raw black metal, austere
folk). v5.5 over-polishes those aesthetics; v5 doesn't.

**Pick v5.5** when you want polished, professional-sounding output, you're
incorporating your own voice (Voices), you've trained a Custom Model, you want
to surgically replace a single section (Replace Section), or you'll mix in a
DAW after using the stem-friendly profile. Expect ~1.5–2 dB quieter masters
than v5.

If the user has no preference, default to **v5.5** for the strongest narrative
prompt understanding and richest cue control — but suggest v5 explicitly if the
song is meant to be raw / unpolished.

## How the skill uses this

At session start, the skill reads `<backup>/user-profile.md` for the
`Default Suno model:` field. If it's set, that model's file in this directory is
loaded before drafting any style prompt or lyrics. If it's blank, the skill asks
once, persists the answer, and continues.

Per-song overrides are honored at any point in the session — saying "let's do
this one in v5" causes the skill to swap in `v5.md` for that song's prompt
shaping without changing the profile default.

## Files

- `v4-5.md` — covers v4.5, v4.5+, and v4.5-All
- `v5.md` — covers v5
- `v5-5.md` — covers v5.5

When a new model ships, drop a new file in this directory and update the comparison
table above plus the picker rubric.
