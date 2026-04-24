# Suno Prompting Guide

> **Last updated:** 2026-04-24 | **Covers:** Suno v5.5
>
> This file contains platform-specific technical knowledge for Suno. Update it as new versions release or best practices evolve. The main SKILL.md handles the creative process; this file handles the platform mechanics.

## Table of Contents

1. [Character Limits & Structure](#character-limits--structure)
2. [Style Prompt Best Practices](#style-prompt-best-practices)
3. [Lyrics Formatting](#lyrics-formatting)
4. [Section Tags Reference](#section-tags-reference)
5. [Performance Directions](#performance-directions)
6. [Genre & Style Descriptors](#genre--style-descriptors)
7. [Vocal Descriptors](#vocal-descriptors)
8. [Instrument & Production Vocabulary](#instrument--production-vocabulary)
9. [v5.5 UI & Features](#v55-ui--features)
10. [Common Pitfalls](#common-pitfalls)
11. [Restraint Prompting Patterns](#restraint-prompting-patterns)
12. [Prompt Templates by Genre](#prompt-templates-by-genre)

---

## Character Limits & Structure

- **Style field character limit: 1000 on v4.5, v4.5+, v5, v5.5.** (v5 earlier was 200; upgraded at v4.5.)
- **Lyrics field:** Keep total under ~3000 characters for best results. Split longer lyrics into parts.
- **Front-load priority so essentials survive truncation.** Order: genre/subgenre → vocal direction → key mood → 1–2 priority instruments → BPM → production notes → dynamic directives → negatives.
- A well-crafted 400–600 char prompt usually outperforms a cluttered 900+ char one.

---

## Style Prompt Best Practices

These practices produce consistently better results in Suno v5 / v5.5:

### Be Specific and Descriptive
Don't just list genres. Describe the *sound*: instrumentation, textures, vocal character, dynamics. "Laid-back acoustic singer-songwriter with soft fingerpicked guitar, mellow beachy grooves, and warm intimate vocals" produces far better results than "acoustic folk."

### Use Narrative Structure, Not Keyword Tags
**This is the single biggest v5/v5.5 change from older models.** v3 and v4 rewarded comma-tag lists. v5 and v5.5 added NLP handling that understands narrative descriptions of how the music unfolds.

Describe the sound as a flowing scene rather than isolated tags:
- ✅ "Begin with soft ambient layers, natural sounds, and a deep steady groove. Build gradually with flowing melodic synths and warm basslines."
- ❌ "ambient layers, deep groove, melodic synths, warm bass"

The narrative form lets the model time arrangement changes (when instruments enter, when dynamics shift). Flat tag lists flatten into "all present from start."

**Especially true for nuanced direction:** arrangement timing, restraint directives, dynamic arcs all require narrative prose to communicate effectively. Comma lists work for simple direction but lose specificity at scale.

### Blend Genres Deliberately
Suno v5.5 handles genre fusion well. Combine styles with intention: "folktronica / found-sound collage" or "chiptune meets midwest emo with glitch-pop textures." Be specific about which elements come from which genre.

### Anchor with Recognizable Reference Points, Not Artist Names
Reference specific *styles* and *eras* rather than artist names. Instead of naming an artist, describe their signature sound: "90s alternative baroque pop with confessional female vocals, expressive piano at the core, shifting from fragile whispers to powerful anthemic cries."

### Do NOT Include Artist Names or Song Titles
Suno style prompts work best with descriptive language. Artist/song references can cause unpredictable results or be ignored entirely. **Important consequence:** Suno preserves the original generation prompt as song metadata displayed under the song title on the song detail page, and this text is NOT editable after generation. If you put `"capture the vocal style of [artist]"` in the prompt, that artist reference lives permanently on the published song page. Always translate artist references into descriptive sound language before generating.

### Define Structure When It Matters
For non-standard structures, spell it out: "progressive structure with abrupt cuts and anachronistic sound-collage inserts" or "verse-pre-chorus-chorus-verse-bridge-final huge chorus with gang 'oh-oh' hooks."

### Specify Tempo, Not Key
- **BPM is honored reliably on v5.5** — worth specifying when the song calls for precision. Examples: "~92 BPM" or "~118 BPM."
- **Key signature is NOT reliably honored.** The generative model doesn't strongly condition on key/time signature per Suno's own acknowledgments. Keep as mood hint if desired, but don't rely on enforcement.
- **Mix directives work well:** "mono low end, ultra-wide highs" or "dry, intimate, plenty of negative space."

### Aim for 4–7 High-Impact Descriptors
Too vague = generic output. Too many conflicting details = confused output. The sweet spot is 4–7 strong, compatible descriptors that paint a coherent sonic picture.

### Iterate in Small Steps
Change one parameter at a time between generations. If the tempo feels wrong, adjust only tempo. If the vocal style is off, adjust only that. This makes it clear what each change does.

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
Use line breaks to indicate phrasing. Suno interprets line breaks as breath/pause points. Keep lines roughly singable — not too long, not fragmented into single words (unless that's a deliberate effect).

### Syllable Count
When remixing or editing lyrics to preserve an existing melody's rhythm, keep the syllable count and stress pattern similar to the original.

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

You can combine descriptors: `[Chorus] (repeat, fuller instrumentation)` or `[Outro] (soft but defiant)`.

---

## Performance Directions

Place in parentheses within lyrics to guide vocal delivery. **Keep them short and actionable.** Arrangement descriptions belong in the Style prompt, not in lyric parentheticals.

**Vocal dynamics:**
- `(whisper)` — breathy, intimate
- `(spoken)` — spoken word, not sung
- `(half-sung)` — between speaking and singing
- `(building intensity)` — gradually louder/more powerful
- `(soft but defiant)` — quiet volume, strong emotion
- `(voice cracks then soars)` — emotional break into power
- `(falsetto)`, `(belted)`, `(harmonized)`, `(ad-lib)` — technique cues

**Arrangement cues (brief):**
- `(stripped)` / `(quiet and stripped)` — minimal instrumentation
- `(fuller instrumentation)` — bring in more instruments
- `(repeat)` — repeat the section

**Character/vibe:**
- `(haunting, intimate)` — mood instruction
- `(playful)` — lighter delivery
- `(wry, half-smiling)` — understated humor

**Important rules:**
- **One cue per section at most.** Stacking multiple cues in one section confuses the model.
- **Short and actionable.** `(whispered)` works; `(fingerpicked acoustic, alone)` doesn't — that's an arrangement description, move it to the Style prompt.
- **v5.5 reads parentheticals as an internal control system** alongside section tags. They carry real weight.

---

## Genre & Style Descriptors

Effective descriptors that produce distinct results in Suno v5 / v5.5:

**Folk/Acoustic:** indie-folk, cinematic folk, folktronica, singer-songwriter, coffeehouse acoustic, fingerpicked intimate, baroque folk

**Rock/Alt:** alternative rock, post-punk, shoegaze, grunge, garage rock, surf rock, psychedelic rock, indie rock, stadium rock, art rock

**Electronic:** synthpop, new wave, chiptune, glitch pop, indietronica, ambient electronic, bloghouse, industrial electro, hyperpop

**Pop:** dream pop, chamber pop, baroque pop, power pop, synth-pop, art pop

**Mood combos:** "melancholic indie-folk," "euphoric synthpop," "gritty industrial electro," "intimate lo-fi bedroom pop," "cinematic post-rock," "muted resigned indie-folk"

**Era references:** "90s alternative," "80s new-wave," "60s surf," "mid-2000s bloghouse"

Stacking 2–3 mood adjectives before the genre helps Suno locate the specific subgenre intended.

---

## Vocal Descriptors

Be specific about the voice you want:

- **Register:** alto, tenor, baritone, soprano
- **Character:** raspy, smooth, breathy, powerful, nasal, gravelly, airy, intimate
- **Delivery:** confessional, defiant, playful, wistful, anthemic, conversational, half-spoken, plain-spoken
- **Technical:** close-mic'd, roomy reverb, dry and present, layered harmonies, gang vocals
- **Gender:** male vocals, female vocals, androgynous vocals
- **Style modifiers:** "gentle rasp and tender phrasing," "shifting from fragile whispers to powerful anthemic cries," "calm spoken-word with half-sung phrases"

---

## Instrument & Production Vocabulary

These terms give Suno strong signals:

**Guitar:** fingerpicked nylon, jangly chorus-guitar, distorted power chords, clean arpeggios, lo-fi acoustic, surf guitar with spring reverb, tremolo picking

**Piano/Keys:** expressive piano arpeggios, warm Rhodes, felt piano, bright synth pads, analog monosynth, airy synth blips, vintage organ

**Bass:** upright bass, sine-wave bass, distorted saw bass, overdriven mono-sub, walking bassline

**Drums:** brushed drums, gated snare, machine-gun tom fills, lo-fi drum loops, handclaps, stadium claps, 4-on-the-floor kick, breakbeat

**Texture:** tape hiss, glitch clicks, found-sound samples, ambient pads, string swells, choir pads, white-noise risers, reverse crashes, pedal steel drone

**Mix/Production:** sidechain pumping, brickwall limiting, stereo micro-edits, negative space, warm tape feel, transient clarity, wide stereo field, mono low end, lo-fi processing

---

## v5.5 UI & Features

### Creating a New Song
1. Go to Suno Studio
2. Click "Create" → "Custom Mode"
3. Paste style prompt into the Style box (1000 char max)
4. Paste lyrics into the Lyrics box
5. Click Generate

### Remixing / Editing an Existing Song
You can't directly edit lyrics on a finished generation. Instead:
1. Open the song → click "Remix/Edit" (circular arrow icon or menu item)
2. Choose the right submenu option (see below)
3. Generate a new version

### Remix/Edit Submenu (v5.5)

- **Open in Studio (New)** — full multitrack editing environment
- **Open in Editor (Pro)** — single-song editor
- **Cover** — recreate a song with different voice/style, preserving vocals/melody
- **Extend** — continue a song past its current end
- **Mashup** — blend two existing songs
- **Sample this song** — extract samples to reuse in new creations
- **Use as Inspiration (Pro)** — style transfer from the source song. Opens Custom Mode with the song loaded in the **Inspo** tab. This replaced the old "Create → Make Persona" workflow.
- **Adjust Speed**
- **Reuse Prompt** — pre-fills Custom Mode with the original generation parameters so you can tweak and regenerate
- **Crop** — trim to specific regions
- **Replace Section (Pro)** — surgical regeneration of a single section while keeping the rest

### Create Submenu (v5.5)

- **Voice (Pro)** — biometric voice upload (record or upload your own singing voice for use in generations). Requires consent checkbox. Beta. Pro/Premier only.
- **Remaster (Pro)**
- **Song Radio**

### Personas → Voices (Critical Rename)

The old "Personas" feature was renamed and split in v5.5:

- **"Voice"** now specifically = biometric voice upload (not style transfer).
- **"Style Persona from song" workflow → Remix/Edit → Use as Inspiration.**

These are commonly confused. When you want to use an existing song as a style/vocal reference for a new song, use **Use as Inspiration**, not Create → Voice.

### Inspo Tab

The Inspo tab (accessed via Use as Inspiration, or by clicking "Inspo" in the audio reference tabs of Custom Mode) accepts **up to 4 reference tracks** for blended style transfer. Stack multiple songs to average voice/style across them.

### When Using Use as Inspiration

- Keep the Style prompt **shorter** — the reference track carries the baseline.
- Use the Style prompt only for what's *new* or needs explicit restraint directives.
- Suno's v5.5 guidance: 1–2 genres, 1 mood line, 2–4 priority instruments max when Inspo is loaded.

### Song Details (Metadata Editor)

Triple-dot menu on a song → **Song Details** lets you edit:
- Title
- Caption
- Short Style tags (comma-separated genres/moods shown on song cards)
- Displayed Lyrics
- Cover Art
- Visibility, Allow Comments, Allow Remix, Pin toggles

**Song Details does NOT let you edit the long generation prompt** displayed under the song title on the detail page. That text is preserved as generation provenance and is permanent. Plan prompts accordingly — especially around artist references.

### Other v5.5 Features

- **Custom Models (Pro):** train a personalized model on your own catalog to capture your style consistently.
- **My Taste (v5.5):** personalization layer that learns from your listens/likes.
- **Get Stems / MIDI (Pro):** export separated stems for DAW post-processing.

---

## Common Pitfalls

1. **Too vague:** "Make a rock song" → generic output. Be specific about *which kind* of rock.

2. **Conflicting descriptors:** "Aggressive punk with gentle lullaby vocals" confuses the model unless you frame it as intentional contrast with structure cues.

3. **Too many artist names:** Even when tempted, describe the *sound* instead. Artist references also get baked into displayed song metadata permanently.

4. **Comma-keyword flat lists on v5.5:** Older-model habit. v5.5 rewards narrative. Use prose sentences that describe how the music unfolds, not just what's present.

5. **Ignoring structure:** If you want a specific arrangement, spell it out. Otherwise Suno defaults to standard pop structure.

6. **Over-polished output:** v5.5 tends to add gloss (reverb, harmonies, build on final chorus) even when you ask for dry/restrained. Explicitly state anti-build directives and be prepared to iterate. See [Restraint Prompting Patterns](#restraint-prompting-patterns).

7. **Too many negatives:** Cap at 2–3. Consolidate overlapping ones. "No crescendo, no cymbal wash, no triumphant lift, no key change, no build" is the same directive five ways — use `"no build anywhere"` to cover them all.

8. **Lyrics too long:** Keep total lyrics under ~3000 characters for best results. If longer, consider splitting into parts.

9. **Parenthetical overreach in lyrics:** Keep parentheticals to short vocal cues. `(fingerpicked acoustic, alone)` is an arrangement description that belongs in the Style prompt, not the Lyrics field.

10. **Expecting key signature enforcement:** BPM is honored reliably. Key is not. Plan accordingly.

---

## Restraint Prompting Patterns

Suno's v5.5 default is to add gloss and build to a satisfying payoff on the final chorus. For songs that need to stay quiet or resigned (intimate indie-folk, hymnal, muted ballads), you must actively fight this.

### What works:

- **Explicit dynamic lock:** "The song stays at the dynamic of a whisper from start to finish. The final chorus is at the same volume as verse one."
- **Consolidated anti-build negative:** "No build anywhere."
- **Cut specific default-to-polish instruments:** "No drums. No banjo." Banjo rolls and drum kits both push toward poppy on fingerpicked folk. If you want the muted register, cut them explicitly — Suno reaches for them by default.
- **Dry production directive:** "Dry production, no reverb on the vocal, plenty of silence between phrases."
- **Instrument narrative arc:** describe when instruments enter, not just that they exist. "Warm upright bass enters in verse two and holds long tones from there" beats "upright bass."
- **No harmonies:** Suno loves to stack vocal harmonies on choruses. Cut them with "no vocal harmonies" if you want an isolated voice.

### Escalation pattern

If the first generation still lifts:
1. Add "absolutely no dynamic change anywhere in the song — the final bar is at the same volume as the first bar" as its own sentence.
2. Remove any instruments from the prompt that Suno is foregrounding inappropriately.
3. Regenerate with only one changed variable.

---

## Prompt Templates by Genre

### Intimate Indie-Folk (restrained/muted)
> Muted, resigned indie-folk ballad with a hymnal quality. The vocal is an intimate male voice, close-mic'd and half-spoken, with a slight rasp, no vibrato, no harmonies. Fingerpicked acoustic guitar leads throughout, slow and sparse. Warm upright bass enters in verse two. An ambient string pad drones quietly from the pre-chorus onward without ever swelling. Felt piano appears in the bridge playing single notes beneath the voice. Around 62 BPM. Dry production, no reverb on the vocal, silence between phrases. The song stays at the dynamic of a whisper from start to finish. No drums. No banjo. No build anywhere.

### Cinematic Indie-Folk (expansive but restrained)
> Cinematic, melancholic indie-folk. Intimate male vocals with poetic, nature-inspired lyrics. Acoustic guitar fingerpicking leads, warm upright bass, sparse piano accents. Atmospheric textures: subtle ambient pads, brushed drums, delicate string swells. Mood: nostalgic, reflective, quietly hopeful. ~92 BPM. Gentle rasp, tender phrasing. Warm, dynamic master.

### 90s Alt-Rock (Female-Fronted)
> Moody, emotionally raw alternative rock, mid-90s female-fronted band vibe. Intimate vocals with aching vulnerability shifting into fierce, distorted crescendos. Lo-fi drum loops, layered vocals, melancholy cello. Themes of [X]. Gritty, ghostly harmonies in the chorus. Introspective, defiant, wounded, beautiful.

### Piano-Driven Confessional Anthem
> Emotional piano-driven ballad with soaring vocals, confessional and intimate, shifting from fragile whispers to powerful anthemic cries. Dynamic piano arpeggios and chords at the core, warm strings, subtle ethereal textures. Raw, poetic, vulnerable. Dramatic yet tender, haunting but empowering. 90s alternative baroque pop feel.

### Synthpop / New Wave
> 80s [theme] soundtrack. Glossy new-wave synthpop with big gated drums, stadium claps, jangly chorus-guitar, shimmering analog pads, bright bass arpeggio. ~[BPM] BPM. Anthemic, defiant, heart-on-sleeve vocals, roomy gymnasium reverb, wide stereo. Euphoric, hopeful, fist-in-the-air.

### Folktronica / Experimental
> Minimalist folktronica with found-sound collage. ~[BPM] BPM. Looping plucked nylon guitar, pizzicato cello, subtle glitch percussion, sine-wave bass, handclaps. Calm [gender] spoken-word with half-sung phrases, close-mic'd, dry and present. Percussive samples (page flips, pencil marks, tape hiss) as rhythmic texture. Spacious, micro-edits, motif-driven. Warm tape feel, wide stereo.

### Epic Rock Anthem
> Epic rock ballad with soaring emotional vocals and powerful group harmonies. Starts dramatic, heartfelt piano and intimate delivery, builds into sweeping anthem with layered guitars, steady drums, triumphant crescendos. Themes of victory, resilience, unity. Chorus: massive, sing-along, uplifting. Blends grandeur and intimacy.

### Neoclassical Ambient (for found-text pieces)
> A delicate neoclassical ambient piece with felt piano, soft string swells, and light electronic textures. Sparse and emotional, with slow evolving chords and a warm, intimate sound. Gentle, cinematic, and introspective — like a quiet Icelandic cabin. Evokes nostalgia, stillness, and quiet hope.

---

## Changelog

- **2026-04-24:** Major update for v5.5. Added Character Limits section (1000 on v4.5+), corrected Style Prompt Best Practices to emphasize narrative-over-tags on v5/v5.5, added BPM-works-key-doesn't note, expanded Performance Directions with scope rules (short actionable only, not arrangement), added full v5.5 UI & Features section (Use as Inspiration, Inspo tab, Personas→Voices rename, Song Details limitations, Custom Models, My Taste), added Restraint Prompting Patterns section capturing anti-build techniques for muted-register songs, added Neoclassical Ambient template for found-text pieces, added pitfall about artist references getting baked into song metadata permanently. Prior version (April 2026 initial) was pre-v5.5 in content despite the date.
- **2026-04 (initial):** Initial version based on Suno v5 best practices and real-world session patterns.
