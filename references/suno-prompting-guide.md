# Suno Prompting Guide — Cross-Version

> **Last updated:** 2026-05-01 | **Covers:** Suno v4.5, v5, v5.5
>
> This file holds the songwriting craft and shared vocabulary that applies
> across modern Suno models. **For model-specific behavior — prompt format
> (narrative vs. comma-tag), cue reliability, character limits, model-only
> features — load the appropriate file from `references/models/`** before
> writing the style prompt and lyrics:
>
> - `references/models/README.md` — picker matrix and decision rubric
> - `references/models/v4-5.md` — Suno v4.5 / v4.5+ / v4.5-All
> - `references/models/v5.md` — Suno v5
> - `references/models/v5-5.md` — Suno v5.5

## Table of Contents

1. [Lyrics Formatting](#lyrics-formatting)
2. [Section Tags Reference](#section-tags-reference)
3. [Bracketed vs. Parenthetical Cues — Mechanics](#bracketed-vs-parenthetical-cues--mechanics)
4. [Performance Direction Vocabulary](#performance-direction-vocabulary)
5. [Genre & Style Descriptors](#genre--style-descriptors)
6. [Vocal Descriptors](#vocal-descriptors)
7. [Instrument & Production Vocabulary](#instrument--production-vocabulary)
8. [Common Pitfalls](#common-pitfalls)
9. [Restraint Prompting Patterns](#restraint-prompting-patterns)
10. [Prompt Templates by Genre](#prompt-templates-by-genre)

---

## Lyrics Formatting

### Section tags

Wrap each section in square brackets on its own line. The lyrics follow on
subsequent lines.

```
[Verse 1]
First line of the verse
Second line of the verse

[Chorus]
First line of chorus
Second line of chorus
```

### Line breaks

Use line breaks to indicate phrasing. Suno interprets line breaks as
breath/pause points. Keep lines roughly singable — not too long, not fragmented
into single words (unless that's a deliberate effect).

### Syllable count

When remixing or editing lyrics to preserve an existing melody's rhythm, keep
the syllable count and stress pattern similar to the original.

### Lyrics length

Keep total lyrics under ~3000 characters for best results across all current
models. If longer, consider splitting into parts.

---

## Section Tags Reference

Standard tags Suno recognizes (work across v4.5, v5, v5.5):

| Tag | Use |
|-----|-----|
| `[Intro]` | Instrumental or vocal opening |
| `[Verse]` / `[Verse 1]` / `[Verse 2]` | Main narrative sections |
| `[Pre-Chorus]` | Build section before chorus |
| `[Chorus]` | Main hook/refrain |
| `[Post-Chorus]` | Section immediately after chorus |
| `[Bridge]` | Contrasting section, often before final chorus |
| `[Break]` | Instrumental or rhythmic pause |
| `[Interlude]` | Transition between sections |
| `[Hook]` | Short repeated melodic/lyrical motif |
| `[Outro]` | Closing section |
| `[Tag]` | Short repeated phrase at the very end |
| `[Refrain]` | Recurring line or couplet |
| `[Final Chorus]` | Last chorus (signals build/climax — drop if you want flat dynamics) |
| `[Instrumental]` | Pure instrumental section |

You can combine descriptors: `[Chorus] (repeat, fuller instrumentation)` or
`[Outro] (soft but defiant)`.

**Reliability of tags varies by model — see the per-model files.** Older models
(v4.5) honor tags in their immediate section but often drop instructions on
later callbacks. v5+ holds tags across the song.

---

## Bracketed vs. Parenthetical Cues — Mechanics

These are two distinct mechanisms that often get conflated. Both work on
modern Suno (v4.5+), with reliability increasing in v5 and v5.5.

- **Bracketed `[Whispered Verse]`, `[Spoken Word]`, `[Spoken Bridge]`,
  `[Haunting Whispered Pre-Chorus]`** — section-level. Apply to the *entire*
  tagged section. Replace or augment standard `[Verse]` / `[Bridge]` etc.
- **Parenthetical `(spoken)`, `(whispered)`, `(belted)`** — inline / line-level.
  Sit inside a section. Apply to the moment they appear (typically the line
  that follows).

Use bracketed when the whole section should carry the delivery (e.g., a
fully-spoken bridge). Use parenthetical when only one line or moment shifts
(e.g., a sung verse with one whispered line in the middle, or a sung bridge
whose final stinger drops to spoken).

Combinations of one delivery word + one mood word + a section tag are the safe
pattern (`[Haunting Whispered Pre-Chorus]`). Stack more and reliability drops.

---

## Performance Direction Vocabulary

Inline parenthetical cues sit inside lyrics and apply to the line they precede.
**Keep them short and actionable.** Arrangement descriptions belong in the
Style prompt, not in lyric parentheticals.

Reliability tiers below are the cross-model defaults; per-model files note where
specific cues are more or less reliable on each model.

### Vocal delivery (most reliable across all current models)

| Cue | Effect |
|---|---|
| `(spoken)` / `(spoken word)` | Spoken delivery for the line |
| `(whispered)` / `(whisper)` | Whispered delivery |
| `(half-sung)` | Between speaking and singing |
| `(belted)` | Powerful sung delivery |
| `(falsetto)` | High-register head voice |
| `(harmonized)` | Vocal harmonies on the line |
| `(ad-lib)` | Improvisational vocal |

### Dynamics / arrangement-light

| Cue | Effect |
|---|---|
| `(building intensity)` | Gradual lift in energy |
| `(stripped back)` / `(stripped)` | Pull instruments out |
| `(soft but defiant)` | Quiet volume + emotional weight |
| `(voice cracks then soars)` | Emotional break into power |
| `(half-time feel)` | Rhythmic feel shift |
| `(key change)` | Modulation |

### Mood / character

| Cue | Effect |
|---|---|
| `(haunting, intimate)` | Mood pair |
| `(playful)` | Lighter delivery |
| `(wry, half-smiling)` | Understated humor |
| `(quiet)` | Volume-down (pair with dynamic-controlling prompt) |

### What does NOT work reliably (avoid across models)

- **Compound cues mixing documented vocab with custom modifiers** —
  `(spoken, alone)` ❌. "alone" isn't a documented vocal direction. If you need
  spoken delivery AND isolation, use `(spoken)` and create the isolation
  structurally (blank line above, brief stanza).
- **Long stage-direction prose** — `(piano builds, voice cracks then soars)` ⚠️.
  Inconsistently honored. Break into shorter cues OR move arrangement direction
  to the Style prompt.
- **Arrangement descriptions in lyric parentheticals** —
  `(fingerpicked acoustic, alone)` ❌. Arrangement belongs in the Style prompt.
- **Custom emotional descriptors** — `(with growing despair)`, `(seething)`,
  `(brokenly)` ⚠️. Stick to documented terms; let lyrics carry emotion.
- **Stacking multiple cues per section** — `(whispered) (slow) (intimate)` ⚠️.
  Confuses the model. One cue, the one that matters most.
- **Stage-script formatting** — `[narrator: "..."]` or `(she enters)` ❌. Suno
  isn't a screenplay parser.
- **Empty/silent cues** — `(silence)`, `(pause)`, `(break)` ❌. Use `[Break]`
  for an instrumental pause, or just leave a blank line.

### Combining safely

- Section tag + one mood/dynamic in parentheses on the same line is fine:
  `[Chorus] (repeat, fuller instrumentation)` or `[Outro] (soft but defiant)`.
- One inline parenthetical cue per section, immediately before the line it
  modifies.
- Bracketed section-level + inline parenthetical CAN combine
  (`[Whispered Verse]` for the whole verse + `(belted)` on one line inside it
  for contrast) but test carefully — competing instructions sometimes cancel.

### Failure modes to expect even when cues are documented

- Suno can ignore cues if surrounding context strongly suggests otherwise (e.g.,
  an "anthem" Style prompt overriding a `(whispered)` cue).
- First generation may not honor a cue; second often does. Re-roll once before
  assuming the cue is broken.
- A cue that lands once may not land identically on regeneration — Suno is
  stochastic.

---

## Genre & Style Descriptors

Effective descriptors that produce distinct results across modern Suno models:

**Folk/Acoustic:** indie-folk, cinematic folk, folktronica, singer-songwriter,
coffeehouse acoustic, fingerpicked intimate, baroque folk

**Rock/Alt:** alternative rock, post-punk, shoegaze, grunge, garage rock, surf
rock, psychedelic rock, indie rock, stadium rock, art rock

**Electronic:** synthpop, new wave, chiptune, glitch pop, indietronica, ambient
electronic, bloghouse, industrial electro, hyperpop

**Pop:** dream pop, chamber pop, baroque pop, power pop, synth-pop, art pop

**Mood combos:** "melancholic indie-folk," "euphoric synthpop," "gritty
industrial electro," "intimate lo-fi bedroom pop," "cinematic post-rock," "muted
resigned indie-folk"

**Era references:** "90s alternative," "80s new-wave," "60s surf," "mid-2000s
bloghouse"

Stacking 2–3 mood adjectives before the genre helps Suno locate the specific
subgenre intended.

---

## Vocal Descriptors

Be specific about the voice you want:

- **Register:** alto, tenor, baritone, soprano
- **Character:** raspy, smooth, breathy, powerful, nasal, gravelly, airy,
  intimate
- **Delivery:** confessional, defiant, playful, wistful, anthemic,
  conversational, half-spoken, plain-spoken
- **Technical:** close-mic'd, roomy reverb, dry and present, layered harmonies,
  gang vocals
- **Gender:** male vocals, female vocals, androgynous vocals
- **Style modifiers:** "gentle rasp and tender phrasing," "shifting from fragile
  whispers to powerful anthemic cries," "calm spoken-word with half-sung
  phrases"

---

## Instrument & Production Vocabulary

These terms give Suno strong signals across all current models:

**Guitar:** fingerpicked nylon, jangly chorus-guitar, distorted power chords,
clean arpeggios, lo-fi acoustic, surf guitar with spring reverb, tremolo picking

**Piano/Keys:** expressive piano arpeggios, warm Rhodes, felt piano, bright
synth pads, analog monosynth, airy synth blips, vintage organ

**Bass:** upright bass, sine-wave bass, distorted saw bass, overdriven mono-sub,
walking bassline

**Drums:** brushed drums, gated snare, machine-gun tom fills, lo-fi drum loops,
handclaps, stadium claps, 4-on-the-floor kick, breakbeat

**Texture:** tape hiss, glitch clicks, found-sound samples, ambient pads, string
swells, choir pads, white-noise risers, reverse crashes, pedal steel drone

**Mix/Production:** sidechain pumping, brickwall limiting, stereo micro-edits,
negative space, warm tape feel, transient clarity, wide stereo field, mono low
end, lo-fi processing

---

## Common Pitfalls

1. **Too vague:** "Make a rock song" → generic output. Be specific about *which
   kind* of rock.

2. **Conflicting descriptors:** "Aggressive punk with gentle lullaby vocals"
   confuses the model unless you frame it as intentional contrast with structure
   cues.

3. **Too many artist names:** Even when tempted, describe the *sound* instead.
   Artist references also get baked into displayed song metadata permanently
   (the long generation prompt is preserved as provenance and is NOT editable
   after generation).

4. **Wrong prompt format for the model.** v4.5 still rewards comma-tag lists;
   v5 and v5.5 reward narrative prose that describes how the music unfolds.
   Using comma-tag style on v5/v5.5 produces flatter results; using heavy
   narrative prose on v4.5 produces under-honored direction. Check the per-model
   file before drafting the style prompt.

5. **Ignoring structure:** If you want a specific arrangement, spell it out.
   Otherwise Suno defaults to standard pop structure.

6. **Over-polished output (v5/v5.5):** modern Suno tends to add gloss
   (reverb, harmonies, build on final chorus) even when you ask for
   dry/restrained. Explicitly state anti-build directives and be prepared to
   iterate. See [Restraint Prompting Patterns](#restraint-prompting-patterns).
   Note: v5 preserves more raw texture than v5.5 for songs that should sound
   austere — for raw/lo-fi/dark/austere aesthetics, prefer v5 over v5.5.

7. **Too many negatives:** Cap at 2–3. Consolidate overlapping ones. "No
   crescendo, no cymbal wash, no triumphant lift, no key change, no build" is
   the same directive five ways — use `"no build anywhere"` to cover them all.

8. **Lyrics too long:** Keep total under ~3000 characters for best results
   across all current models.

9. **Parenthetical overreach in lyrics:** Keep parentheticals to short vocal
   cues. `(fingerpicked acoustic, alone)` is an arrangement description that
   belongs in the Style prompt, not the Lyrics field.

10. **Expecting key signature enforcement:** BPM is honored reliably across v5
    and v5.5. Key signature is not — the generative model doesn't strongly
    condition on key. Plan accordingly.

11. **Forgetting tokenizer front-loading:** the first ~3 words of the style
    field carry more weight than the last ten. Order: genre/subgenre → vocal
    direction → key mood → 1–2 priority instruments → BPM → production notes
    → dynamic directives → negatives.

---

## Restraint Prompting Patterns

Modern Suno (v5/v5.5) defaults to adding gloss and building to a satisfying
payoff on the final chorus. For songs that need to stay quiet or resigned
(intimate indie-folk, hymnal, muted ballads), you must actively fight this.
v5.5 leans harder into polish than v5; if the song should sound raw or austere,
also consider switching to v5.

### What works:

- **Explicit dynamic lock:** "The song stays at the dynamic of a whisper from
  start to finish. The final chorus is at the same volume as verse one."
- **Consolidated anti-build negative:** "No build anywhere."
- **Cut specific default-to-polish instruments:** "No drums. No banjo." Banjo
  rolls and drum kits both push toward poppy on fingerpicked folk. If you want
  the muted register, cut them explicitly — Suno reaches for them by default.
- **Dry production directive:** "Dry production, no reverb on the vocal, plenty
  of silence between phrases."
- **Instrument narrative arc:** describe when instruments enter, not just that
  they exist. "Warm upright bass enters in verse two and holds long tones from
  there" beats "upright bass." (This narrative approach works best on v5/v5.5;
  on v4.5 the instrument timing may be ignored.)
- **No harmonies:** Suno loves to stack vocal harmonies on choruses. Cut them
  with "no vocal harmonies" if you want an isolated voice.

### Escalation pattern

If the first generation still lifts:
1. Add "absolutely no dynamic change anywhere in the song — the final bar is
   at the same volume as the first bar" as its own sentence.
2. Remove any instruments from the prompt that Suno is foregrounding
   inappropriately.
3. Regenerate with only one changed variable.

---

## Prompt Templates by Genre

These templates are written in v5/v5.5-friendly narrative prose. For v4.5,
either translate to comma-tag style or keep the prose short with strong
keyword anchors at the front.

### Intimate Indie-Folk (restrained/muted)
> Muted, resigned indie-folk ballad with a hymnal quality. The vocal is an
> intimate male voice, close-mic'd and half-spoken, with a slight rasp, no
> vibrato, no harmonies. Fingerpicked acoustic guitar leads throughout, slow
> and sparse. Warm upright bass enters in verse two. An ambient string pad
> drones quietly from the pre-chorus onward without ever swelling. Felt piano
> appears in the bridge playing single notes beneath the voice. Around 62 BPM.
> Dry production, no reverb on the vocal, silence between phrases. The song
> stays at the dynamic of a whisper from start to finish. No drums. No banjo.
> No build anywhere.

### Cinematic Indie-Folk (expansive but restrained)
> Cinematic, melancholic indie-folk. Intimate male vocals with poetic,
> nature-inspired lyrics. Acoustic guitar fingerpicking leads, warm upright
> bass, sparse piano accents. Atmospheric textures: subtle ambient pads,
> brushed drums, delicate string swells. Mood: nostalgic, reflective, quietly
> hopeful. ~92 BPM. Gentle rasp, tender phrasing. Warm, dynamic master.

### 90s Alt-Rock (Female-Fronted)
> Moody, emotionally raw alternative rock, mid-90s female-fronted band vibe.
> Intimate vocals with aching vulnerability shifting into fierce, distorted
> crescendos. Lo-fi drum loops, layered vocals, melancholy cello. Themes of
> [X]. Gritty, ghostly harmonies in the chorus. Introspective, defiant,
> wounded, beautiful.

### Piano-Driven Confessional Anthem
> Emotional piano-driven ballad with soaring vocals, confessional and intimate,
> shifting from fragile whispers to powerful anthemic cries. Dynamic piano
> arpeggios and chords at the core, warm strings, subtle ethereal textures.
> Raw, poetic, vulnerable. Dramatic yet tender, haunting but empowering. 90s
> alternative baroque pop feel.

### Synthpop / New Wave
> 80s [theme] soundtrack. Glossy new-wave synthpop with big gated drums,
> stadium claps, jangly chorus-guitar, shimmering analog pads, bright bass
> arpeggio. ~[BPM] BPM. Anthemic, defiant, heart-on-sleeve vocals, roomy
> gymnasium reverb, wide stereo. Euphoric, hopeful, fist-in-the-air.

### Folktronica / Experimental
> Minimalist folktronica with found-sound collage. ~[BPM] BPM. Looping plucked
> nylon guitar, pizzicato cello, subtle glitch percussion, sine-wave bass,
> handclaps. Calm [gender] spoken-word with half-sung phrases, close-mic'd,
> dry and present. Percussive samples (page flips, pencil marks, tape hiss) as
> rhythmic texture. Spacious, micro-edits, motif-driven. Warm tape feel, wide
> stereo.

### Epic Rock Anthem
> Epic rock ballad with soaring emotional vocals and powerful group harmonies.
> Starts dramatic, heartfelt piano and intimate delivery, builds into sweeping
> anthem with layered guitars, steady drums, triumphant crescendos. Themes of
> victory, resilience, unity. Chorus: massive, sing-along, uplifting. Blends
> grandeur and intimacy.

### Neoclassical Ambient (for found-text pieces)
> A delicate neoclassical ambient piece with felt piano, soft string swells,
> and light electronic textures. Sparse and emotional, with slow evolving
> chords and a warm, intimate sound. Gentle, cinematic, and introspective —
> like a quiet Icelandic cabin. Evokes nostalgia, stillness, and quiet hope.

---

## Changelog

- **2026-05-01:** Restructured to cross-version. Per-model behavior (prompt
  format, cue reliability, character limits, model-only features) moved to
  `references/models/` — one file per model (v4-5.md, v5.md, v5-5.md) plus a
  picker README. This file now holds shared songwriting craft, vocabulary
  tables, lyrics formatting mechanics, and prompt templates that apply across
  v4.5, v5, and v5.5. Added cross-version notes to Common Pitfalls and
  Restraint Prompting Patterns where behavior shifts between model families.
- **2026-04-25 (later):** Added Comprehensive Cue & Tag Reference subsection
  (now in `models/v5-5.md`) — full tables of documented working tags
  (section, bracketed performance modifiers, inline parenthetical cues across
  vocal/dynamic/mood categories) plus an explicit "what does NOT work" list
  plus failure-mode notes.
- **2026-04-25:** Verified parenthetical performance cues and v5.5 Inspire
  feature against current docs and community guides. Added: bracketed vs.
  parenthetical cue distinction; compound-cue caveat; new Inspire
  (playlist-based) feature section (now in `models/v5-5.md`).
- **2026-04-24:** Major update for v5.5. Added Character Limits section,
  corrected Style Prompt Best Practices to emphasize narrative-over-tags on
  v5/v5.5, added BPM-works-key-doesn't note, expanded Performance Directions
  with scope rules, added v5.5 UI & Features section (now in `models/v5-5.md`),
  added Restraint Prompting Patterns and Neoclassical Ambient template, added
  pitfall about artist references getting baked into song metadata permanently.
- **2026-04 (initial):** Initial version based on Suno v5 best practices.
