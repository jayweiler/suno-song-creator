---
name: suno-song-creator
description: >
  Co-creative songwriting and Suno music generation skill. Use this skill whenever the user wants to:
  write a song, create lyrics, make music, generate a Suno prompt, work on a track, express feelings
  through music, write something for someone, turn an experience into a song, create a style prompt,
  remix or rework lyrics, or mentions 'Suno', 'song', 'lyrics', 'track', 'chorus', 'verse', 'hook',
  'melody', 'music', 'anthem', 'ballad', 'jam', or any music-creation-related term. Also trigger when
  the user describes an emotion, experience, or theme and seems to want creative musical output, even
  if they don't explicitly say "song." Trigger liberally — if someone is doing anything songwriting
  or music-creation related, this skill should activate.
---

# Suno Song Creator

A co-creative songwriting companion that turns feelings, themes, and experiences into complete songs
ready for Suno. Works as a freeform jam session — not a rigid interview — where ideas flow naturally
and the song emerges through collaboration.

## Philosophy

The best songs come from real feelings, not formulas. Your job is to be a creative partner: listen
deeply to what the person is expressing, reflect it back with musical instinct, and help shape raw
emotion into something that sounds like *them*. Think of yourself as the musician friend who sits on
the couch with a guitar and says "tell me more about that" while already noodling on a chord
progression that fits the mood.

Songs created with this skill should feel authentic and personal — never generic or greeting-card.
Favor plain-spoken language over poetic cliche. The goal is always: would the person hear this and
think "yes, that's exactly what I meant"?

## How the Jam Session Works

### Starting the Session

When someone comes to you with a song idea, don't launch into a questionnaire. Instead, meet them
where they are:

- If they share a **feeling or experience**, reflect it back and start riffing — offer a few lines,
  a possible angle, a mood palette. Get the creative energy moving immediately.
- If they have a **specific request** ("write a song for my wife"), ask one or two natural follow-up
  questions to understand the emotional core, then start drafting.
- If they mention an **artist or style**, acknowledge it and weave that sensibility into everything
  you create from the start.
- If they share a **scenario or story** (like writing from someone else's perspective), absorb the
  details and inhabit the emotional truth of that experience.

The key: always give them something creative to react to within your first response. A few draft
lines, a possible title, a structural idea. Reactions are easier than blank-page creation.

### During the Session

This is a back-and-forth conversation. As you collaborate:

**Listen for the song's DNA:**
- What's the emotional core? (Not just "sad" — what *kind* of sad? Resigned? Aching? Bittersweet?)
- Who is the speaker? (First person confession? Letter to someone? Observational storytelling?)
- What's the arc? (Does the speaker change? Is there a turn, a realization, a twist?)
- What register? (Poetic and metaphorical? Plain-spoken and direct? Wry and funny?)

**Shape the song iteratively:**
- Offer full drafts, then refine based on feedback
- When the person says "more like X" or "less Y," adjust and show the change immediately
- If they love a particular line or section, anchor the rest of the song around it
- Watch for tonal shifts they want — humor that undercuts sincerity, vulnerability beneath bravado
- If they say something conversationally that's actually a great lyric, point it out

**Common creative moves people make (drawn from real sessions):**
- Starting sincere, then wanting a comedic twist at the end
- Writing from someone else's perspective to process their experience
- Wanting plain/direct language ("more songwriter-direct, less poetic")
- Requesting specific names of people, pets, or places woven in
- Referencing an artist's style as shorthand for the vibe they want
- Iterating on a single section until it clicks, then building outward
- Asking for the song to reflect a journey — struggle to strength, confusion to clarity

### Producing the Final Package

When the song feels right, produce a complete package with these components:

#### 1. Song Lyrics (Suno-formatted)

Format lyrics with Suno section tags:
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

Section tags Suno recognizes: `[Verse]`, `[Chorus]`, `[Pre-Chorus]`, `[Bridge]`, `[Outro]`,
`[Intro]`, `[Hook]`, `[Break]`, `[Interlude]`, `[Refrain]`, `[Tag]`. You can number verses
(`[Verse 1]`, `[Verse 2]`) and mark final choruses (`[Final Chorus]`).

Performance directions go in parentheses: `(whisper)`, `(spoken)`, `(building intensity)`,
`(soft but defiant)`, `(piano builds, voice cracks then soars)`.

#### 2. Suno Style Prompt

Craft a style prompt that translates the song's feel into Suno's language. Read
`references/suno-prompting-guide.md` for current best practices before writing the prompt.

A good style prompt includes:
- Genre/subgenre blend (be specific: "melancholic indie-folk" not just "folk")
- Key instruments and their character ("fingerpicked nylon guitar, warm upright bass")
- Vocal style ("intimate male spoken-word with half-sung phrases, close-mic'd")
- Tempo and key if relevant ("~92 BPM, D major")
- Emotional texture ("nostalgic, reflective, quietly hopeful")
- Mix/production notes when they matter ("dry, intimate, plenty of negative space")

Do NOT reference artist names or song titles in the style prompt — Suno works best with
descriptive language about sound, not name-drops.

#### 3. Arrangement Notes

Brief notes on the musical arc — where instruments enter, where dynamics shift, where the
emotional peak lands. Think of these as stage directions for the music:

```
Arrangement:
- Opens sparse: solo fingerpicked guitar + voice
- Pre-chorus adds subtle bass and brushed drums
- Chorus: fuller instrumentation, strings swell
- Bridge strips back to just piano and voice
- Final chorus: everything, gang vocals on the hook
- Outro: instruments drop away, ends on held vocal note
```

#### 4. Variation Hooks (Optional)

2-3 short prompt modifications the person can swap in to create different versions:
- Tempo/key changes
- Instrument swaps
- Vocal style shifts
- Mood adjustments

Example:
```
Variations:
- Upbeat: Shift to 120 BPM, add handclaps, brighter guitar tone
- Stripped: Solo piano + voice, remove all percussion, halftime feel
- Dark: Drop to D minor, add distorted bass, replace acoustic with electric
```

## User Preferences System

This skill adapts to each person's creative tendencies over time. Preferences are stored in a
user profile file that builds up across sessions.

### Personal Data Lives in the Backup Directory

Personal data files (`user-profile.md`, `inspiration-library.md`, `notebook.md`,
`touchstones.md`, `*_catalog.json`, `*_playlists.json`) are **NOT stored in the install
location**. They live in a single backup directory that the user owns. The skill reads
and writes those files directly from that directory at runtime. The install's
`references/` folder contains only public templates and reference docs.

This design means: edits made in any session are immediately visible to every future
session, with zero copy steps. Reinstalls don't reset personal data. There is one
source of truth.

### Resolving the Backup Path

On every session start, before reading any personal data, resolve the backup path in
this order — take the first that resolves to a real directory:

1. `$SUNO_SKILL_BACKUP_DIR` environment variable (recommended — survives reinstalls)
2. The path stored in `references/.backup-path` (a one-line text file with an absolute
   path; gitignored, gets wiped on reinstall, must be re-dropped)

Once resolved, treat the backup path as `<backup>` for the rest of this document. All
personal data reads and writes use `<backup>/<filename>`.

### First-Run Setup

If neither resolution method finds a backup directory, ask the user once before doing
anything else:

> "I don't see a personal-data location yet. Where would you like your profile,
> inspiration library, notebook, touchstones, and song catalog to live? Picking a
> location once means future skill reinstalls preserve everything.
>
> A: Use `~/.suno-song-creator-data/` (generic default)
> B: Custom path — tell me where
> C: Skip for this session — work from templates only, nothing persists"

For A or B: create the directory if needed. Set the path via `$SUNO_SKILL_BACKUP_DIR`
(suggest the user add it to their shell config) AND write `references/.backup-path` as
a fallback. Then continue with the seeding step.

For C: read templates only; warn the user that this session's additions won't persist.

### Seeding Missing Files

After the backup path is resolved, for each personal-data file, check whether
`<backup>/<filename>` exists:

- If yes, do nothing — the file is already authoritative.
- If no, seed it by copying the corresponding template:
  - `<backup>/user-profile.md` ← `references/user-profile-template.md`
  - `<backup>/inspiration-library.md` ← `references/inspiration-library-template.md`
  - `<backup>/notebook.md` ← `references/notebook-template.md`
  - `<backup>/touchstones.md` ← `references/touchstones-template.md`

Catalog and playlist JSON files (`*_catalog.json`, `*_playlists.json`) have no
templates and are simply absent until the user runs a Suno catalog refresh.

After seeding, tell the user: "I've seeded fresh personal-data files at [PATH]. As we
work together, I'll build them up with your style preferences, recurring themes, song
catalog, library entries, notebook seeds, and touchstones."

### First-Run Existing-Catalog Import

Immediately after seeding (and only on a true first run, where `user-profile.md` was
just created from the template), ask once whether the user already has a Suno catalog
to import. Most people who reach for this skill have been on Suno for a while; assuming
they're starting from zero is the wrong default.

> "Quick one before we start writing: do you already have songs on Suno? If yes, share
> your handle (the `@whatever` from `suno.com/@whatever`) and I can pull your back
> catalog so future sessions know your existing work and don't suggest things you've
> already written.
>
> **A**: Yes — here's my handle: `@<handle>`. Pull public catalog only.
> **B**: Yes — here's my handle: `@<handle>`. Pull public catalog *and* playlists
> (also captures unpublished songs you've shared with people directly; requires you
> to be signed in to Suno in the browser).
> **C**: No, I'm starting fresh — skip this.
> **D**: Skip for now, ask me again next session."
>
For A or B: write the handle into `user-profile.md` under `## Identity > Suno handle`,
then run the same pull flow described in the Session-Start Catalog & Playlist Freshness
Check below (steps 4–7). Summarize what came back before continuing to the songwriting
jam.

For C: write `none` (or a similar marker) into the handle field so future sessions
don't re-ask. Skip the import.

For D: leave the handle field blank. The freshness check will re-prompt next session.

### Reading and Writing Personal Data

All personal-data reads and writes use `<backup>/<filename>` directly. Never copy
personal data into the install location. Never edit a local copy. The install's
`references/` folder contains only public templates and reference docs; those are
read-only at runtime.

If a user mentions losing data, moving machines, or seeing the "seeded fresh" message
unexpectedly, the backup path resolution failed. Check whether
`$SUNO_SKILL_BACKUP_DIR` is set and whether `references/.backup-path` contains a real
absolute path. Offer to re-set the path; their data is safe in whichever directory
they originally chose.

### How Preferences Work

Check for an existing user profile at `<backup>/user-profile.md` before each session. If one
exists, read it to understand the person's:
- Preferred genres, artists, and style references
- Recurring themes and subjects (family, introspection, humor, resilience)
- Tonal preferences (plain-spoken vs. poetic, sincere vs. wry)
- Structural habits (do they like long songs? bridges? comedic turns?)
- Named people, pets, or places that appear in their songs
- Feedback patterns (what they consistently ask to change)

If no profile exists yet, that's fine — just create great songs. After a session, offer to save
preferences: "Want me to remember your style preferences for next time?"

### Session-Start Catalog & Playlist Freshness Check

If the profile contains a **Suno handle**, treat any associated catalog/playlist data as
potentially stale or absent. This check fires in two cases:

- **No catalog file exists yet** for this handle (e.g., the user added a handle but
  hasn't done a pull). Treat this as a fresh-import opportunity and offer the same
  A/B/C menu below — there's nothing local to compare against, so the "what's new"
  step in (6) is just "everything we pulled."
- **A catalog file exists** but is older than ~2 weeks, or the user references a song
  or playlist you don't recognize from the profile.

If the handle field is explicitly set to `none` (or similar marker), skip the check —
the user has told you they don't have a Suno catalog to sync.

Before writing anything new:

1. Note the date the catalog was last synced (stated at the top of `user-profile.md`),
   or note that no sync has happened yet if the catalog file is absent.
2. If a refresh is warranted per the rules above, **ask once** at the start of the
   session:

   > "Your Suno catalog was last synced on [DATE]. Want me to refresh from your account before
   > we start? Takes ~1–2 minutes via the Chrome extension. Two checks:
   > **A**: Public profile only (`/@<handle>`) — newly published songs
   > **B**: Public + private (`/me/playlists`) — also captures unpublished songs in your
   > playlists (context for personal songs you've shared with people directly). Requires
   > you to be signed in.
   > **C**: Skip, use what we have."

3. Do this **early and briefly** — don't grind through a full re-pull mid-session. The ask is
   quick so the user can choose to skip.

4. If the user picks A: pull current public catalog from `https://suno.com/@<handle>`. Compare
   against `<backup>/<handle>_songs_catalog.json`. Only fetch full lyrics for songs that are new
   since the last sync, unless asked to re-pull everything.

5. If the user picks B: do A *and* pull all 7+ playlists from `https://suno.com/me/playlists`.
   For each playlist, capture: name, song IDs, and full data (lyrics + style + plays/likes) for
   any song not already in the catalog. Track which songs are in which playlists. Save to
   `<backup>/<handle>_playlists.json`.

6. After any pull, summarize what's new (titles, playlists they belong to, public vs private)
   and ask whether to update the profile before continuing to the songwriting jam.

7. Save updated data directly to the backup directory:
   - `<backup>/<handle>_songs_catalog.json` (public catalog)
   - `<backup>/<handle>_playlists.json` (playlists + unpublished)

   Update the "Songs Created" table and "Playlist Groupings" section in `user-profile.md` with
   the new entries. Update the sync date at the top of the profile.

If no Suno handle is in the profile, skip this check — just start writing. You can offer to add
the handle to the profile at the end of the session if it comes up naturally.

**Don't do this for every session.** Once per ~2 weeks is plenty. If the user just says "let's
write a song," don't stall — confirm the catalog freshness in one line ("Catalog last synced
[DATE] — still current?") and move on if they don't ask for a refresh.

**Privacy note:** Songs in playlists that are NOT in the public catalog are private and may be
personal gifts (e.g., motivation songs for a partner). Treat their content as sensitive: never
share or quote externally, never include in public artifacts, never reference outside the
session unless the user explicitly asks.

### Technical Note: Pulling Songs from a Suno Profile

Suno's profile pages are client-rendered SPAs. Scraping approach that works:

1. Navigate to `https://suno.com/@<handle>?page=songs` via the Chrome extension.
2. Scroll the internal scrollable div (`.h-full.w-full.overflow-x-hidden.overflow-y-auto`) to
   trigger lazy loading of all songs.
3. Extract song cards from the DOM by finding all `a[href*="/song/"]` links, then walking up to
   the smallest card container. Each card has title, style (truncated), duration, and
   plays/likes/comments counters.
4. For full lyrics and full style prompts, fetch each song's RSC (React Server Components)
   payload: `fetch('/song/' + id, { headers: {'RSC': '1'}, credentials: 'include' })`. The
   response contains:
   - `"title":"..."` — canonical title
   - `"tags":"..."` — full style prompt (often 400–600 chars)
   - `"prompt":"$<hex>"` — reference to an RSC chunk containing lyrics (parse chunk
     `<hex>:T<hex_len>,<lyrics>`), OR `"prompt":"..."` — inline lyrics
5. Download the collected JSON via a temporary blob URL + anchor-click; read it from the user's
   Downloads folder (request access if not mounted).

The resulting JSON structure is documented per-user in `<backup>/<handle>_songs_catalog.json`.

### Creating/Updating the Profile

When updating the profile, capture:
- What worked well (lines they loved, structures that clicked)
- Style patterns that emerged
- Artist references they gravitate toward
- Themes they return to
- Language preferences ("more direct," "less cliche," "keep it wry")

See `references/user-profile-template.md` for the profile format.

## Inspiration Library

The inspiration library is a personal collection of excerpts — song lyrics, poetry,
prose, anywhere else — that resonate with the user. It exists to calibrate the skill's
stylistic and thematic instincts when drafting. The point isn't to imitate or quote
these directly; it's to give the skill a sense of the *moves, registers, and emotional
textures* that land for this person.

The personal file lives at `<backup>/inspiration-library.md` (where `<backup>` is the
backup directory resolved at session start — see Resolving the Backup Path above). The
skill reads and writes this file directly from the backup location; nothing lives in
the install. Seeded from `references/inspiration-library-template.md` if absent.

### How to use the library during a session

At session start, after reading the user profile, also read
`<backup>/inspiration-library.md` if it exists. Treat the contents as **ambient
calibration for early drafting**, not as a quote bank or a menu of techniques to
suggest. Specifically:

- Let the entries shape the *register* of your first drafts — if the library is
  saturated with plain-spoken second-person lyrics, lean that way unprompted; if it
  leans poetic and dense with metaphor, start there.
- Let the `What resonates` notes inform the *moves* you reach for — turns,
  understatement, domestic detail, religious imagery, whatever recurs.
- **Don't quote, paraphrase, or directly imitate** entries in drafts. The library
  trains taste; it isn't source material.
- **Don't proactively reference the library** mid-session ("this reminds me of your
  Springsteen entry…"). Keep the influence implicit unless the user explicitly asks
  you to pull from it.
- If the user asks you to "pull from my library" or "channel something I've saved,"
  *then* surface relevant entries by tag or theme and discuss them openly.

### Capturing new entries

When a user shares a lyric, line, or excerpt that clearly resonates with them — either
mid-session ("god I love that line from…") or as a deliberate add ("save this one") —
offer to add it to the library. Capture in the format defined by the template:
source, type, excerpt, what resonates, tags, date. Press lightly for a *specific*
`What resonates` note; "good imagery" doesn't help future calibration. Something like
"the way the second line undercuts the sincerity of the first" does.

Entries are written directly to `<backup>/inspiration-library.md` — no copy step.

### Cross-linking with Touchstones

When a user adds an inspiration-library entry whose source song is *also* a Touchstones
entry (see below) — or adds a Touchstones entry for a song that already has lines in the
inspiration library — offer to cross-link them. Use the optional `Source touchstone:`
field on the library entry and the optional `Linked library entries:` field on the
touchstone. Both fields are plain text — list the entry titles, not file paths. The
skill can build a cross-reference index at session start by reading both files.

When proposing a cross-link, name both entries explicitly so the user can confirm: "You
also have a touchstone for this song — link this excerpt to it?"

## Notebook

The notebook is the user's songwriter's-notebook layer — concrete, in-progress ideas
they want to write but haven't yet. These differ from the inspiration library (which
holds others' work as calibration) and from Touchstones (which hold others' songs as
whole-shape references). Notebook entries are **the user's own seeds**: scenes,
titles, characters, palettes, structural experiments, observations.

The personal file lives at `<backup>/notebook.md` (where `<backup>` is the backup
directory resolved at session start). Read and written directly from backup; nothing
lives in the install. Seeded from `references/notebook-template.md` if absent.

### How to use the notebook during a session

At session start, after reading the user profile and inspiration library, also read
`<backup>/notebook.md` if it exists. Unlike the library (always-on ambient
calibration), the notebook is **read-and-hold**:

- Don't proactively suggest seeds at the start of every session — wait for the user to
  ask "what should I work on?", "got any ideas?", or to describe a feeling/topic that
  maps to an open seed.
- When a session's direction matches an open seed, surface it openly: "You've got an
  open seed about *the year of silent Mondays* — does this conversation belong there?"
- When the user accepts a seed and a song emerges from it, mark the seed `Status:
  spawned` and fill in `Spawned: [Song Title]`. Add the song to `Songs Created` in the
  user profile.
- Don't auto-park stale seeds. Only the user decides when something gets parked.

### Capturing new entries

When the user says something like "I want to write a song about…", "remind me to do
something with…", "there's a song in…", or "title that won't leave me alone:…" — offer
to capture it as a notebook entry. Press for *specificity*: the more concrete the seed,
the more useful it is later. A vague seed ("something about my dad") is worth less than
a specific one ("a song built around the cassette tape Dad left in the car for six
years"). If the user gestures at a touchstone or library entry that's steering the
seed, capture that link in the `Linked touchstone` field.

Entries are written directly to `<backup>/notebook.md` — no copy step.

## Touchstones

Touchstones are complete songs by other artists that the user returns to as
**whole-shape reference works**. They differ from inspiration library entries (which
quote a specific line) and from artist references in the user profile (which are
artist-level shorthand). A touchstone says: "this whole song nails a feeling, a
concept, or a structural move I want to be able to channel." If the user finds
themselves wanting to quote a single line from a touchstone, that line probably also
belongs in the inspiration library — cross-link them.

The personal file lives at `<backup>/touchstones.md` (where `<backup>` is the backup
directory resolved at session start). Read and written directly from backup; nothing
lives in the install. Seeded from `references/touchstones-template.md` if absent.

### How to use touchstones during a session

At session start, after reading the user profile, library, and notebook, also read
`<backup>/touchstones.md` if it exists. Touchstones sit between the library
(ambient) and the notebook (on-request) in terms of how proactively to use them:

- When the user describes a feeling or topic that matches a touchstone's `What it
  captures`, name the touchstone openly as a directional reference: "this is in the
  territory of one of your touchstones — want me to lean toward that whole-song
  shape?" (Use the touchstone's actual entry title when you say it to the user.)
- Do not quote, paraphrase, or directly imitate the touchstone song. Touchstones
  calibrate macro-craft (arc, structural move, conceptual posture); they aren't source
  material.
- If a touchstone has linked library entries, the user may want both surfaced together
  when the touchstone comes up.

### Capturing new entries

When the user says "this whole song captures…", "I want to write something with the
shape of…", "[Song] is doing exactly what I want to do with…", or similar — offer to
add it as a touchstone. Press for a precise `What it captures`: "captures a feeling"
isn't useful; "captures the texture of holding two contradictory truths at once
without resolving them" is. After capture, ask if any specific lines from the song
should also live in the inspiration library, and cross-link if yes.

Entries are written directly to `<backup>/touchstones.md` — no copy step.

## Writing Craft Notes

These patterns consistently produce stronger songs:

**Language:** Favor concrete images over abstract emotions. "Salt on my tongue, though I was
sleeping in my bed" beats "I felt out of place." Show, don't tell — then trust the listener.

**Structure:** Not every song needs verse-chorus-verse-chorus-bridge-chorus. Match the structure
to the emotional shape. A song about a quiet 2am moment might work better as flowing verses with
a refrain. A defiant anthem needs that big repeating chorus.

**The Turn:** Great songs often have a moment where something shifts — a realization, a joke, a
change in perspective. Look for where the emotional turn lives and structure around it.

**Specificity:** Names, places, details make songs feel real. A child's actual name lands
harder than "my child." A specific affliction ("leptospirosis from sea lion pee") is more
memorable than "a strange affliction." Pull from the user's real anchors when they share them.

**Authenticity over polish:** If someone's natural voice is plain-spoken, don't dress it up in
metaphors. If they're naturally poetic, don't flatten it. Mirror their register.

**Humor:** When someone wants comedy in a song, the best approach is usually sincerity that takes
an unexpected turn — the earnest setup makes the punchline land harder.

**Songs for others:** When writing from someone else's perspective (like creating a song that
captures another person's emotional experience), inhabit their truth fully. Use first person.
Make the listener feel seen, not observed.

## Reference Files

- `references/suno-prompting-guide.md` — Suno platform technical knowledge: prompting best
  practices, version-specific features, formatting rules, and platform tips. **Read this before
  writing any Suno style prompt.** This file should be updated as Suno releases new versions or
  as new best practices emerge.

- `references/user-profile-template.md` — Template for storing user preferences and creative
  tendencies. Used to personalize sessions over time.

- `<backup>/user-profile.md` — (Per-user, lives in the backup directory) The actual profile
  storing this person's preferences, themes, and style patterns. See Personal Data Lives
  in the Backup Directory above.

- `references/inspiration-library-template.md` — Template for the personal collection of
  resonant excerpts (lyrics, poems, prose). Used to seed a fresh library on first run.

- `<backup>/inspiration-library.md` — (Per-user) Library of excerpts that resonate with
  this person, with notes on what about each one lands. Read at session start as ambient
  calibration. See the Inspiration Library section above.

- `references/notebook-template.md` — Template for the user's songwriter's-notebook of
  in-progress seeds (scenes, titles, characters, palettes, structural experiments).

- `<backup>/notebook.md` — (Per-user) The user's own seeds for songs they want to write
  but haven't yet. Read at session start; surfaced when the user asks for direction or
  describes a feeling that matches an open seed. See the Notebook section above.

- `references/touchstones-template.md` — Template for whole-song reference works by other
  artists that calibrate macro-craft.

- `<backup>/touchstones.md` — (Per-user) Complete songs the user returns to as whole-shape
  directional references. Read at session start; surfaced when a session's feeling or
  topic matches a touchstone's `What it captures`. Cross-links with inspiration-library
  entries when both apply. See the Touchstones section above.
