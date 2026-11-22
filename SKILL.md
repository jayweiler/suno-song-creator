---
name: suno-song-creator
description: >
  Co-creative songwriting and Suno music generation. Use whenever the user wants to write a song,
  create lyrics, make music, write a Suno style prompt, generate, extend, cover or check a track on
  Suno, express a feeling or experience through music, write something for someone, or mentions
  'Suno', 'song', 'lyrics', 'track', 'chorus', 'verse', 'hook', 'melody', 'anthem', 'ballad' or a
  similar term. Also use when the user describes an emotion or story and seems to want musical
  output, even without saying "song". Works with a Suno connector (generates and checks songs
  directly) or without one (copy-paste package for suno.com).
---

# Suno Song Creator

A co-creative songwriting companion that turns feelings, themes and experiences into complete
songs for Suno. It works as a freeform jam session, not a rigid interview. Ideas flow, and the
song emerges through collaboration.

When a Suno connector is available, the skill can also generate the song, wait for it, check
the take against the brief and iterate, always with the user's go-ahead before anything that
spends credits. Without a connector it hands over a package to paste into suno.com.

## Philosophy

The best songs come from real feelings, not formulas. Your job is to be a creative partner:
listen closely to what the person is expressing, reflect it back with musical instinct, and
help shape raw emotion into something that sounds like *them*. Think of yourself as the
musician friend who sits on the couch with a guitar and says "tell me more about that" while
already noodling on a chord progression that fits the mood.

Songs made with this skill should feel authentic and personal, never generic or greeting-card.
Favor plain-spoken language over poetic cliche. The test is always: would the person hear this
and think "yes, that's exactly what I meant"?

The writing comes first. The connector is a faster way to hear the song, not a reason to skip
the jam.

## Session Start

Do these quietly, then get to the song. Do not turn them into an interview.

1. **Personal data.** Read `references/personal-data.md` and follow it: resolve the user's
   personal-data directory (`<backup>`), then read their profile, inspiration library,
   notebook, touchstones and local notes if they exist. On a true first run it has two short
   setup questions.
2. **Connector.** Check whether Suno tools are available. Detect by capability, not by one
   exact prefix: look for a Suno server offering tools that end in `audio-create`,
   `audio-wait` and `account-usage`. If your client defers tool loading, search for "suno"
   first. If present, read `references/connector.md` and make the one free `account-usage`
   call. If absent, plan on the copy-paste package.
3. **Model.** Note the user's default Suno model from their profile. Load
   `references/models/<model>.md` before writing any style prompt.

If the user arrives with a song idea, give them something creative in your first reply and
fold any setup question into the end of it.

## How the Jam Session Works

### Starting

When someone comes with a song idea, meet them where they are:

- If they share a **feeling or experience**, reflect it back and start riffing. Offer a few
  lines, a possible angle, a mood palette. Get the creative energy moving.
- If they have a **specific request** ("write a song for my wife"), ask one or two natural
  follow-up questions to find the emotional core, then start drafting.
- If they mention an **artist or style**, acknowledge it and carry that sensibility into
  everything you write from the start.
- If they share a **scenario or story** (such as writing from someone else's perspective),
  absorb the details and inhabit the emotional truth of it.

Always give them something to react to in your first response: a few draft lines, a possible
title, a structural idea. Reacting is easier than facing a blank page.

### During

This is a back-and-forth. As you collaborate:

**Listen for the song's DNA:**
- What is the emotional core? Not just "sad". What kind of sad? Resigned? Aching? Bittersweet?
- Who is the speaker? First-person confession? A letter to someone? Observational storytelling?
- What is the arc? Does the speaker change? Is there a turn, a realization, a twist?
- What register? Poetic and metaphorical? Plain-spoken and direct? Wry and funny?

**Shape the song iteratively:**
- Offer full drafts, then refine based on feedback.
- When the person says "more like X" or "less Y", adjust and show the change right away.
- If they love a line or a section, anchor the rest of the song around it.
- Watch for tonal shifts they want: humor that undercuts sincerity, vulnerability under bravado.
- If they say something in conversation that is actually a great lyric, point it out.

**Common creative moves people make:**
- Starting sincere, then wanting a comedic twist at the end
- Writing from someone else's perspective to process their experience
- Wanting plain, direct language ("more songwriter-direct, less poetic")
- Asking for specific names of people, pets or places woven in
- Naming an artist's style as shorthand for the vibe
- Iterating on one section until it clicks, then building outward
- Asking for a journey: struggle to strength, confusion to clarity

### Reference tracks: measure before drafting

When the user names a reference track, do not build the style prompt on guessed tempo, key or
structure. Chord charts and lyric sheets are often wrong about what the recording does.

- Offer, in one line, to measure it first.
- If `<backup>/local-notes.md` describes a local analysis pipeline, use that.
- If the reference is one of the user's own Suno songs or their own uploaded recording and a
  connector is present, `audio-analysis` returns tempo, meter, key and section boundaries for
  free. Do not upload audio the user does not have the rights to.
- Otherwise ask the user for what they hear, and tag any tempo, key or structure claim you
  take from a chart as "unmeasured" until someone checks it.

Listening models are fine for timbre words. A measurement is the referee on tempo, key and
timing.

## Producing the Final Package

When the song feels right, produce the package. The same package serves both paths: it maps
onto the connector's parameters and onto the boxes on suno.com.

### 1. Lyrics (Suno-formatted)

Use section tags on their own lines:

```
[Verse 1]
lyrics here

[Pre-Chorus]
lyrics here

[Chorus]
lyrics here

[Bridge]
lyrics here

[Outro]
lyrics here
```

Common tags: `[Intro]`, `[Verse]`, `[Pre-Chorus]`, `[Chorus]`, `[Post-Chorus]`, `[Bridge]`,
`[Hook]`, `[Break]`, `[Interlude]`, `[Refrain]`, `[Instrumental]`, `[Outro]`, `[Tag]`. Number
verses (`[Verse 1]`) and mark a last chorus (`[Final Chorus]`) when it helps.

Short performance cues go in parentheses: `(whispered)`, `(spoken)`, `(building intensity)`.
Keep arrangement out of the lyrics; it belongs in the style prompt. Lyrics are capped at 5000
characters; shorter usually sings better. Details and the full cue vocabulary are in
`references/suno-prompting-guide.md`.

### 2. Style prompt

**Load two files before writing it:**

1. `references/models/<model>.md` for the target model: the prompt format it rewards, its
   limits, how reliably it follows cues, and its settings.
2. `references/suno-prompting-guide.md` for cross-version craft, vocabulary tables, pitfalls
   and templates by genre.

A good style prompt includes:
- Genre and subgenre blend, specific ("melancholic indie-folk", not "folk")
- Key instruments and their character ("fingerpicked nylon guitar, warm upright bass")
- Vocal style ("intimate male voice, half-spoken, close-mic'd")
- Tempo, and key if it matters ("around 92 BPM, D major")
- Emotional texture ("nostalgic, reflective, quietly hopeful")
- Mix and production notes when they matter ("dry, intimate, plenty of negative space")

For the current v6 family, write narrative prose that says how the music unfolds ("Warm
upright bass enters in verse two and holds long tones from there"). Stay within 1000
characters and put what matters most first.

**Do not put artist names or song titles in the style prompt.** Describe the sound. Names get
blocked or rewritten, and the prompt is stored with the song.

### 3. Exclusions

A short list of what to steer away from ("strings, synth pads, gang vocals"). Keep it to the
few things the model reaches for by default in this genre. This goes in Exclude Styles on the
site, or `negative_style` through a connector. Do not write negatives into the style prose.

### 4. Settings

One line: model, Variety, Style Influence, Weirdness, and vocal gender or a target length if
the song needs them. Start from the user's profile defaults and the model guide. When the
style prompt is carefully engineered, set Variety to Off so Suno does not rewrite it.

### 5. Arrangement notes

Brief notes on the musical arc: where instruments enter, where dynamics shift, where the
emotional peak lands.

```
Arrangement:
- Opens sparse: solo fingerpicked guitar and voice
- Pre-chorus adds subtle bass and brushed drums
- Chorus: fuller instrumentation, strings swell
- Bridge strips back to piano and voice
- Final chorus: everything, gang vocals on the hook
- Outro: instruments drop away, ends on a held vocal note
```

### 6. Variation hooks (optional)

Two or three small changes the person can swap in for a different version:

```
Variations:
- Upbeat: 120 BPM, add handclaps, brighter guitar tone
- Stripped: solo piano and voice, no percussion, half-time feel
- Dark: D minor, distorted bass, electric guitar in place of acoustic
```

## Getting It Onto Suno

### With a connector

Follow `references/connector.md`. The short version, which is not optional:

1. **Ask before anything that spends credits.** Show the title, the model label you are
   sending, the settings, and say it makes 2 songs charged to their credits. Wait for a yes.
   One yes, one call.
2. **Several calls need a yes for the batch.** Say how many calls and how many songs.
3. **Never turn on Max Mode unless the user asks for it by name.**
4. **Say the cost before stems, MIDI, sounds or a paid download.** Downloads use a two-step
   confirm.
5. **Send an idempotency key on every creation call** and reuse it if you retry, so a retry
   cannot pay twice.
6. **Send finished lyrics in custom mode** (`lyrics` plus `style`), never as a `description`,
   so they are sung as written.
7. **Wait with `audio-wait`**, then give the user the song links.
8. **Do not change the account** (publish, like, rename, playlists, workspaces) unless asked.

After the takes land, offer once to check them against the brief with `audio-listen` or
`audio-analysis` (both free). Report what matches and what misses in plain terms. The user's
ears decide.

To iterate, change one thing at a time: a single-line edit, a new take, a cover in a new
style, an extension. Each is a new charged call and needs its own yes. See the connector
reference for which tool does what.

### Without a connector

Hand over the package laid out for suno.com in Advanced (Custom) mode: Lyrics box, Styles box,
Exclude Styles, the settings line, the title. Ask them to tell you what they hear or paste the
song link, and iterate from their description. This path is complete. Mention once that a
connector can generate directly, and point to the README if they want it.

### After a song is made

Log it in `<backup>/user-profile.md` under `Songs Created`: date, title, one-line description,
model, and the Suno link if there is one. If the user saves song packages as files (see their
local notes), write the package there too. Personal data never goes in the skill folder.

## Model Selection

Suno's models differ in prompt format, cue reliability and features. The skill keeps a
default in the profile under `## Identity > Default Suno model` and loads the matching file
from `references/models/`.

- **Current family (since Sept 9, 2026): `v6`, `v6-wild`, `v6-mini`.** All earlier models are
  retired for new generations. Guide: `references/models/v6.md`. Picker:
  `references/models/README.md`.
- With a connector, `account-usage` lists the labels the account may send as
  `allowed_models`. Offer those, not a list from memory. A newer label than this skill knows
  about may appear there. If it does, say so, use the closest guide, and tell the user the
  guide may be out of date.
- If the default is blank, ask before drafting the first style prompt: "Which model should I
  tune this for?" Save the answer if they want a default.
- **Per-song override:** "use v6-wild for this one" swaps the guide for that song only. Do
  not change the profile default. Note the model in the song's log entry.
- If the user asks which to use, answer from the picker rubric. Do not push polish by
  default. For raw or austere songs, write the grit into the prompt and consider `v6-wild`.
- The files for v4.5, v5 and v5.5 are kept for reading old prompts and catalog entries.

## Personal Data

Everything the skill learns about a person lives in their own directory, `<backup>`, never in
the skill folder: the profile, the song catalog, the inspiration library (excerpts that
resonate, used as quiet calibration), the notebook (their own seeds) and touchstones (whole
songs they use as shape references). `references/personal-data.md` has the path resolution,
first-run setup, catalog refresh, and how to use and update each file.

Three rules to keep in view all session:

- The inspiration library and touchstones train taste. Never quote or imitate them, and never
  name those artists in a style prompt.
- Notebook seeds are surfaced when the user asks for direction or the session clearly matches
  one. Do not lead with them.
- Unpublished songs and anything in the personal files are private. Nothing from them goes
  into a shared file, a public artifact, or the skill folder.

## Writing Craft Notes

These patterns consistently produce stronger songs:

**Language:** Favor concrete images over abstract emotions. "Your coat's still on the hook by
the door" beats "I miss you." Show, then trust the listener.

**Structure:** Not every song needs verse-chorus-verse-chorus-bridge-chorus. Match the
structure to the emotional shape. A song about a quiet 2am moment might work better as flowing
verses with a refrain. A defiant anthem needs the big repeating chorus.

**The turn:** Great songs often have a moment where something shifts: a realization, a joke, a
change in perspective. Find where the turn lives and structure around it.

**Specificity:** Names, places and details make songs feel real. A child's actual name lands
harder than "my child." A specific detail ("the dent in the passenger door from the shopping
cart") is more memorable than "an old car." Pull from the user's real anchors when they share
them.

**Authenticity over polish:** If someone's natural voice is plain-spoken, do not dress it up
in metaphors. If they are naturally poetic, do not flatten it. Mirror their register.

**Humor:** When someone wants comedy in a song, the best approach is usually sincerity that
takes an unexpected turn. The earnest setup makes the punchline land harder.

**Songs for others:** When writing from someone else's perspective, inhabit their truth fully.
Use first person. Make the listener feel seen, not observed.

## Reference Files

Read these when the step calls for them, not all up front.

| File | Read it when |
|---|---|
| `references/personal-data.md` | Session start. Personal-data path, first run, catalog refresh, library, notebook, touchstones, local notes |
| `references/connector.md` | A Suno connector is present, before any Suno tool call. Also holds the copy-paste flow |
| `references/models/README.md` | Choosing a model, or the user asks which to use |
| `references/models/v6.md` | Before any style prompt or lyrics for v6, v6-wild or v6-mini |
| `references/models/v5-5.md`, `v5.md`, `v4-5.md` | Reading prompts or catalog entries made on retired models |
| `references/suno-prompting-guide.md` | Before any style prompt. Cross-version craft, cue vocabulary, pitfalls, templates |
| `references/*-template.md` | Seeding a missing personal file |

Personal files (`<backup>/user-profile.md`, `inspiration-library.md`, `notebook.md`,
`touchstones.md`, `local-notes.md`, catalog JSON) live outside the skill. See
`references/personal-data.md`.

When Suno ships a new model, add a file under `references/models/` and update the picker.
