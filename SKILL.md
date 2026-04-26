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

### First-Run Bootstrap (with Auto-Restore)

The personal `references/user-profile.md` file is **gitignored** — it never ships with the skill
and is never overwritten by reinstalls. On every session start, run this check before doing
anything else:

1. **Check whether `references/user-profile.md` exists.** If yes, read it and proceed normally.
2. **If it doesn't exist, resolve a backup location** (in this order, take the first that
   resolves to a real directory containing a `user-profile.md`):
   - `$SUNO_SKILL_BACKUP_DIR` environment variable
   - The path stored in `references/.backup-path` (a one-line text file with an absolute path,
     gitignored)
3. **If a backup is resolved, restore from it.** Copy `<backup>/user-profile.md` to
   `references/user-profile.md`, `<backup>/inspiration-library.md` to
   `references/inspiration-library.md` (if present), and any `<backup>/*_catalog.json` and
   `<backup>/*_playlists.json` files to `references/`. Tell the user: "Restored your personal
   profile from backup at [PATH]." Then proceed normally.
4. **If no backup is resolved, ask the user once** before falling back to the template:

   > "I don't see a personal profile yet. Before we start, would you like to set up a backup
   > location so your profile and song catalog survive future skill reinstalls?
   >
   > A: Yes, use `~/.suno-song-creator-backup/` (recommended generic default)
   > B: Yes, custom path — tell me where
   > C: Skip — bootstrap from template, no backup. (You can set this up later by writing the
   >    path to `references/.backup-path` or setting `$SUNO_SKILL_BACKUP_DIR`.)"

   If the user chooses A or B: create the backup directory if needed, write the absolute path
   to `references/.backup-path`, then check whether the backup already contains a profile — if
   yes, restore from it; if empty, fall through to template (step 5) and immediately back up
   the new profile to the chosen location.
5. **Fall back to the template.** Copy `references/user-profile-template.md` →
   `references/user-profile.md`. Tell the user: "I've created a fresh profile from the
   template. As we work together, I'll build it up with your style preferences, recurring
   themes, and song catalog."

The same gitignore + restore logic protects `references/*_catalog.json` files and
`references/inspiration-library.md` (see the Inspiration Library section below).

### Personal-Data Backup (Recommended)

Once the user has a backup location configured, **after any meaningful update to the profile or
catalog, copy the files to the backup location.** Either remind the user manually, or — if a
helper script like `sync-suno-personal.sh` is present in their environment — invoke or suggest
running it. Without a backup, a skill reinstall (Cowork or otherwise) will reset the profile to
the template and lose accumulated context.

If a user mentions losing their profile, moving machines, or seeing the "fresh from template"
message unexpectedly, check whether `references/.backup-path` is set and whether the backup
location actually contains files. Offer to restore manually if auto-restore didn't fire.

### How Preferences Work

Check for an existing user profile at `references/user-profile.md` before each session. If one
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

If the profile contains a **Suno handle** and a list of known published songs and playlists,
treat both as potentially stale. Before writing anything new:

1. Note the date the catalog was last synced (stated at the top of `user-profile.md`).
2. If it's been more than ~2 weeks, or if the user references a song or playlist you don't
   recognize from the profile, **ask once** at the start of the session:

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
   against `references/<handle>_songs_catalog.json`. Only fetch full lyrics for songs that are new
   since the last sync, unless asked to re-pull everything.

5. If the user picks B: do A *and* pull all 7+ playlists from `https://suno.com/me/playlists`.
   For each playlist, capture: name, song IDs, and full data (lyrics + style + plays/likes) for
   any song not already in the catalog. Track which songs are in which playlists. Save to
   `references/<handle>_playlists.json`.

6. After any pull, summarize what's new (titles, playlists they belong to, public vs private)
   and ask whether to update the profile before continuing to the songwriting jam.

7. Save updated data to:
   - `references/<handle>_songs_catalog.json` (public catalog)
   - `references/<handle>_playlists.json` (playlists + unpublished)

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

See `references/<handle>_songs_catalog.json` for an example of the resulting data structure.

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

The personal file lives at `references/inspiration-library.md`. It is **gitignored**
and follows the same bootstrap + backup logic as the user profile (see First-Run
Bootstrap above): restored from backup if present, otherwise seeded from
`references/inspiration-library-template.md`. When backing up personal data, include
this file alongside the profile and catalog.

### How to use the library during a session

At session start, after reading the user profile, also read
`references/inspiration-library.md` if it exists. Treat the contents as **ambient
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

After adding entries, copy `references/inspiration-library.md` to the backup location
along with the profile.

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

- `references/user-profile.md` — (Created per-user) The actual profile storing this person's
  preferences, themes, and style patterns.

- `references/inspiration-library-template.md` — Template for the personal collection of
  resonant excerpts (lyrics, poems, prose). Used to seed a fresh library on first run.

- `references/inspiration-library.md` — (Created per-user) The actual library of excerpts
  that resonate with this person, with notes on what about each one lands. Read at session
  start as ambient calibration for stylistic and thematic instincts. See the Inspiration
  Library section above.
