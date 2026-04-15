# Suno Prompting Guide

> **Last updated:** April 2026 | **Covers:** Suno v5+
>
> This file contains platform-specific technical knowledge for Suno. Update it as new versions
> release or best practices evolve. The main SKILL.md handles the creative process; this file
> handles the platform mechanics.

## Table of Contents

1. [Style Prompt Best Practices](#style-prompt-best-practices)
2. [Lyrics Formatting](#lyrics-formatting)
3. [Section Tags Reference](#section-tags-reference)
4. [Performance Directions](#performance-directions)
5. [Genre & Style Descriptors](#genre--style-descriptors)
6. [Vocal Descriptors](#vocal-descriptors)
7. [Instrument & Production Vocabulary](#instrument--production-vocabulary)
8. [Suno Studio Features](#suno-studio-features)
9. [Common Pitfalls](#common-pitfalls)
10. [Prompt Templates by Genre](#prompt-templates-by-genre)

---

## Style Prompt Best Practices

These practices produce consistently better results in Suno v5+:

### Be Specific and Descriptive
Don't just list genres. Describe the *sound*: instrumentation, textures, vocal character, dynamics.
"Laid-back acoustic singer-songwriter with soft fingerpicked guitar, mellow beachy grooves, and warm
intimate vocals" produces far better results than "acoustic folk."

### Use Conversational Structure, Not Keywords
Describe the sound as a flowing scene rather than isolated tags. "Begin with X, then transition
to Y, evolve into Z" outperforms "X, Y, Z" as a comma list. The model responds to narrative
descriptions of how the music should unfold.

### Blend Genres Deliberately
Suno v5 handles genre fusion well. Combine styles with intention: "folktronica / found-sound
collage" or "chiptune meets midwest emo with glitch-pop textures." Be specific about which
elements come from which genre.

### Anchor with Recognizable Reference Points
Reference specific *styles* and *eras* rather than artist names. Instead of naming an artist,
describe their signature sound: "90s alternative baroque pop with confessional female vocals,
expressive piano at the core, shifting from fragile whispers to powerful anthemic cries."

### Do NOT Include Artist Names or Song Titles
Suno style prompts work best with descriptive language. Artist/song references can cause
unpredictable results or be ignored entirely. Translate the reference into sound description.

### Define Structure When It Matters
For non-standard structures, spell it out: "progressive structure with abrupt cuts and
anachronistic sound-collage inserts" or "verse-pre-chorus-chorus-verse-bridge-final huge chorus
with gang 'oh-oh' hooks."

### Specify Tempo, Key, and Mix When Relevant
These technical details give Suno strong steering signals:
- Tempo: "~92 BPM" or "~118 BPM"
- Key: "D major" or "A minor"
- Mix: "mono low end, ultra-wide highs" or "dry, intimate, plenty of negative space"

### Aim for 4-7 High-Impact Descriptors
Too vague = generic output. Too many conflicting details = confused output. The sweet spot is
4-7 strong, compatible descriptors that paint a coherent sonic picture.

### Iterate in Small Steps
Change one parameter at a time between generations. If the tempo feels wrong, adjust only tempo.
If the vocal style is off, adjust only that. This makes it clear what each change does.

---

## Lyrics Formatting

### Section Tags
Wrap each section in square brackets on its own line. The lyrics follow on subsequent lines.

```
[Verse 1]
First line of the verse
Second line of the verse

[Chorus]
First line of chorus
Second line of chorus
```

### Line Breaks
Use line breaks to indicate phrasing. Suno interprets line breaks as breath/pause points.
Keep lines roughly singable — not too long, not fragmented into single words (unless that's
a deliberate effect).

### Syllable Count
When remixing or editing lyrics to preserve an existing melody's rhythm, keep the syllable
count and stress pattern similar to the original.

---

## Section Tags Reference

Standard tags Suno recognizes:

| Tag | Use |
|-----|-----|
| `[Intro]` | Instrumental or vocal opening |
| `[Verse]` / `[Verse 1]` | Main narrative sections |
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
| `[Final Chorus]` | Last chorus (signals build/climax) |

You can combine descriptors: `[Chorus] (repeat, fuller instrumentation)` or
`[Outro] (soft but defiant)`.

---

## Performance Directions

Place in parentheses within lyrics to guide vocal delivery and arrangement:

**Vocal dynamics:**
- `(whisper)` — breathy, intimate
- `(spoken)` — spoken word, not sung
- `(half-sung)` — between speaking and singing
- `(building intensity)` — gradually louder/more powerful
- `(soft but defiant)` — quiet volume, strong emotion
- `(voice cracks then soars)` — emotional break into power

**Arrangement cues:**
- `(piano builds)` — instrument swells
- `(stripped)` / `(quiet and stripped)` — minimal instrumentation
- `(fuller instrumentation)` — bring in more instruments
- `(repeat)` — repeat the section

**Character/vibe:**
- `(haunting, intimate)` — mood instruction
- `(playful)` — lighter delivery
- `(wry, half-smiling)` — understated humor

---

## Genre & Style Descriptors

Effective descriptors that produce distinct results in Suno v5:

**Folk/Acoustic:** indie-folk, cinematic folk, folktronica, singer-songwriter, coffeehouse acoustic,
fingerpicked intimate, baroque folk

**Rock/Alt:** alternative rock, post-punk, shoegaze, grunge, garage rock, surf rock, psychedelic rock,
indie rock, stadium rock, art rock

**Electronic:** synthpop, new wave, chiptune, glitch pop, indietronica, ambient electronic,
bloghouse, industrial electro, hyperpop

**Pop:** dream pop, chamber pop, baroque pop, power pop, synth-pop, art pop

**Mood combos:** "melancholic indie-folk," "euphoric synthpop," "gritty industrial electro,"
"intimate lo-fi bedroom pop," "cinematic post-rock"

**Era references:** "90s alternative," "80s new-wave," "60s surf," "mid-2000s bloghouse"

---

## Vocal Descriptors

Be specific about the voice you want:

- **Register:** alto, tenor, baritone, soprano
- **Character:** raspy, smooth, breathy, powerful, nasal, gravelly, airy, intimate
- **Delivery:** confessional, defiant, playful, wistful, anthemic, conversational
- **Technical:** close-mic'd, roomy reverb, dry and present, layered harmonies, gang vocals
- **Gender:** male vocals, female vocals, androgynous vocals
- **Style modifiers:** "gentle rasp and tender phrasing," "shifting from fragile whispers to
  powerful anthemic cries," "calm spoken-word with half-sung phrases"

---

## Instrument & Production Vocabulary

These terms give Suno strong signals:

**Guitar:** fingerpicked nylon, jangly chorus-guitar, distorted power chords, clean arpeggios,
lo-fi acoustic, surf guitar with spring reverb, tremolo picking

**Piano/Keys:** expressive piano arpeggios, warm Rhodes, bright synth pads, analog monosynth,
airy synth blips, vintage organ

**Bass:** upright bass, sine-wave bass, distorted saw bass, overdriven mono-sub, walking bassline

**Drums:** brushed drums, gated snare, machine-gun tom fills, lo-fi drum loops, handclaps,
stadium claps, 4-on-the-floor kick, breakbeat

**Texture:** tape hiss, glitch clicks, found-sound samples, ambient pads, string swells,
choir pads, white-noise risers, reverse crashes

**Mix/Production:** sidechain pumping, brickwall limiting, stereo micro-edits, negative space,
warm tape feel, transient clarity, wide stereo field, mono low end, lo-fi processing

---

## Suno Studio Features

### Creating a New Song
1. Go to Suno Studio
2. Click "Create" → "Custom Mode"
3. Paste style prompt into the Style box
4. Paste lyrics into the Lyrics box
5. Click Generate

### Remixing/Editing an Existing Song
You can't directly edit lyrics on a finished generation. Instead:
1. Open the song → click "Remix" (circular arrow icon)
2. In remix dialog → click "Custom Mode"
3. Edit/replace lyrics and tweak the style prompt
4. Generate a new version

### Tips
- "Keep vibe" toggle (when available) preserves melody while changing words
- Generate multiple versions and pick the best — outputs vary significantly
- Export as audio or MIDI for DAW post-processing

### Creative Uses
- **Vocal → Instrument transfer:** Upload a vocal/hum, ask Suno to resynthesize as an instrument
  while preserving pitch contour and timing
- **Beatbox → Drum kit:** Upload beatbox, request kick/snare/hihat stems matching the timing
- **Spoken phrase → Rhythmic instrument:** Feed spoken cadence, translate prosody into instrument
- **Harmony morphs:** Upload stacked vocal pad, convert to strings/synth pad
- **Structural remixes:** Upload a track, ask Suno to reinterpret elements

---

## Common Pitfalls

1. **Too vague:** "Make a rock song" → generic output. Be specific about *which kind* of rock.
2. **Conflicting descriptors:** "Aggressive punk with gentle lullaby vocals" confuses the model
   unless you frame it as intentional contrast with structure cues.
3. **Too many artist names:** Even when tempted, describe the *sound* instead.
4. **Ignoring structure:** If you want a specific arrangement, spell it out. Otherwise Suno
   defaults to standard pop structure.
5. **Over-polished output:** v5 tends to add gloss (reverb, harmonies) even when you ask for dry.
   Explicitly state "no reverb, no harmonies, dry mix" and be prepared to iterate.
6. **Lyrics too long for Suno's window:** Keep total lyrics under ~3000 characters for best results.
   If longer, consider splitting into parts.

---

## Prompt Templates by Genre

### Intimate Indie-Folk
> Cinematic, melancholic indie-folk. Intimate [male/female] vocals with poetic, nature-inspired
> lyrics. Acoustic guitar fingerpicking leads, warm upright bass, sparse piano accents. Atmospheric
> textures: subtle ambient pads, brushed drums, delicate string swells. Mood: nostalgic, reflective,
> quietly hopeful. ~[BPM] BPM, [key]. Gentle rasp, tender phrasing. Warm, dynamic master.

### 90s Alt-Rock (Female-Fronted)
> Moody, emotionally raw alternative rock, mid-90s female-fronted band vibe. Intimate vocals with
> aching vulnerability shifting into fierce, distorted crescendos. Lo-fi drum loops, layered vocals,
> melancholy cello. Themes of [X]. Gritty, ghostly harmonies in the chorus. Introspective, defiant,
> wounded, beautiful.

### Piano-Driven Confessional Anthem
> Emotional piano-driven ballad with soaring vocals, confessional and intimate, shifting from fragile
> whispers to powerful anthemic cries. Dynamic piano arpeggios and chords at the core, warm strings,
> subtle ethereal textures. Raw, poetic, vulnerable. Dramatic yet tender, haunting but empowering.
> 90s alternative baroque pop feel.

### Synthpop/New Wave
> 80s [theme] soundtrack. Glossy new-wave synthpop with big gated drums, stadium claps, jangly
> chorus-guitar, shimmering analog pads, bright bass arpeggio. ~[BPM] BPM, [key]. Anthemic, defiant,
> heart-on-sleeve vocals, roomy gymnasium reverb, wide stereo. Euphoric, hopeful, fist-in-the-air.

### Folktronica / Experimental
> Minimalist folktronica with found-sound collage. ~[BPM] BPM. Looping plucked nylon guitar,
> pizzicato cello, subtle glitch percussion, sine-wave bass, handclaps. Calm [gender] spoken-word
> with half-sung phrases, close-mic'd, dry and present. Percussive samples (page flips, pencil marks,
> tape hiss) as rhythmic texture. Spacious, micro-edits, motif-driven. Warm tape feel, wide stereo.

### Epic Rock Anthem
> Epic rock ballad with soaring emotional vocals and powerful group harmonies. Starts dramatic,
> heartfelt piano and intimate delivery, builds into sweeping anthem with layered guitars, steady
> drums, triumphant crescendos. Themes of victory, resilience, unity. Chorus: massive, sing-along,
> uplifting. Blends grandeur and intimacy.

---

## Changelog

- **2026-04:** Initial version based on Suno v5 best practices and real-world session patterns.
