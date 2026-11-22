# Suno Models: Picker and Per-Model Guides

This directory holds per-model prompting guidance. The skill loads the file
matching the user's `Default Suno model` (from `<backup>/user-profile.md`,
under `## Identity`) before drafting a style prompt or lyrics. Per-song
overrides are honored, for example "use v6-wild for this one."

Checked 2026-11-21 against Suno's v6 FAQ, "What's new in v6?" and release
notes. No model newer than the v6 family is named there as of that date.

## Current family (since September 9, 2026)

All earlier models were retired for new generations on that day. Old songs
stay playable, shareable, remasterable and coverable.

| | v6 | v6-wild | v6-mini |
|---|---|---|---|
| Plan | Pro and Premier | Pro and Premier | All plans, including free |
| Suno's description | Flagship. Reliable, precise, polished across genres | Less predictable and more varied, for exploration | Faster, lighter, for quick ideas |
| Use it for | Finished songs, following a brief | Surprise, genre blends, genres v6 flattens, MIDI score input | Lyric and structure checks, drafts |
| Max length | 8 minutes | 8 minutes | 8 minutes |
| Prompt format | Narrative prose | Narrative prose | Narrative prose |

With a Suno connector, the labels an account may actually send are in
`allowed_models` from `account-usage`. Trust that list over this table.

## When to choose what

**v6** for anything meant to be finished. It follows a brief closely and
polishes by default, so ask for grit in words if the song needs it.

**v6-wild** when the song wants surprise or lives in a genre where testers
found v6 flat (heavy guitar, metal, drum and bass, per community reports).
Expect to discard more takes. A good pattern is to sketch in wild, then cover
the keeper in v6.

**v6-mini** to check lyrics and structure cheaply before spending on v6, or
when the account has no paid plan.

If the user has no preference, default to **v6** on a paid plan and
**v6-mini** otherwise.

## Settings that matter as much as the model

- **Variety.** Above Off, Suno rewrites the style text before generating. Set
  it to Off (0) whenever the style prompt is engineered.
- **Exclude Styles.** Put rejections here, not in the style prose.
- **Max Mode.** More compute for more credits. Suno suggests it for songs over
  two minutes, close covers, style transfer and consistent vocals. The user
  decides; the skill never turns it on unasked.

Details, sources and what is unverified: `v6.md`.

## Retired models (reference only)

Kept so the skill can read old prompts and catalog entries. They cannot be
selected for new generations.

| | v4.5 / v4.5+ | v5 | v5.5 |
|---|---|---|---|
| Released | May and July 2025 | September 2025 | March 2026 |
| Prompt format | Comma-tags most reliable | Narrative prose | Narrative prose |
| Known for | Predictable tag behavior | Raw, lo-fi, austere texture kept intact | Polish, more headroom, Voices, Custom Models |

When reusing a prompt written for one of these, re-test it on v6. The v6
family was trained on a different corpus, and a prompt tuned for v5.5 will not
land the same way. For the raw aesthetics v5 handled well, write the grit into
the v6 prompt and try v6-wild.

## How the skill uses this

At session start the skill reads the `Default Suno model:` field in the
profile. If it is set, that model's file here is loaded before any style
prompt or lyrics. If it is blank, the skill asks once, saves the answer if the
user wants a default, and continues.

A per-song override swaps in a different file for that song only and does not
change the profile default.

## Files

- `v6.md`: v6, v6-wild, v6-mini (current)
- `v5-5.md`: v5.5 (retired)
- `v5.md`: v5 (retired)
- `v4-5.md`: v4.5, v4.5+, v4.5-All (retired)

When a new model ships, add a file here and update the tables above.
