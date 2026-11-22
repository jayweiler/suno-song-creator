# Personal Data: Profile, Catalog, Library, Notebook, Touchstones

This skill adapts to each person over time. What it learns lives in a few
plain files the user owns. Read this file at session start, before the first
personal-data read or write.

## Contents

1. [Where personal data lives](#where-personal-data-lives)
2. [Resolving the backup path](#resolving-the-backup-path)
3. [First-run setup](#first-run-setup)
4. [Using the profile](#using-the-profile)
5. [Catalog and playlist refresh](#catalog-and-playlist-refresh)
6. [Inspiration library](#inspiration-library)
7. [Notebook](#notebook)
8. [Touchstones](#touchstones)
9. [Local notes](#local-notes)

## Where personal data lives

Personal files are **not stored in the skill folder**. They live in one backup
directory the user owns, called `<backup>` in this skill:

| File | Holds |
|---|---|
| `<backup>/user-profile.md` | Style preferences, themes, people and places, songs made, defaults |
| `<backup>/inspiration-library.md` | Excerpts by others that resonate, with notes on why |
| `<backup>/notebook.md` | The user's own song seeds |
| `<backup>/touchstones.md` | Whole songs by others used as shape references |
| `<backup>/<handle>_songs_catalog.json` | The user's Suno songs |
| `<backup>/<handle>_playlists.json` | Playlists and what is in them |
| `<backup>/local-notes.md` | Optional machine-specific workflow notes |

The skill reads and writes these files in place. The skill's `references/`
folder holds only public templates and guides, and is read-only at runtime.

Why: an edit in any session is visible to every later session with no copy
step, a reinstall cannot reset anything, and there is one source of truth.

**Never copy personal data into the skill folder**, and never write it into
any file that could be shared, zipped or committed with the skill.

## Resolving the backup path

At session start, take the first of these that resolves to a real directory:

1. The `$SUNO_SKILL_BACKUP_DIR` environment variable (recommended; survives
   reinstalls).
2. The path in `references/.backup-path`, a one-line text file with an
   absolute path. It is gitignored and is wiped by a reinstall.

If the variable points at a directory that does not exist but
`.backup-path` resolves, use `.backup-path` and tell the user once that the
variable is stale. If a path is set and nothing resolves, say so and ask. Do
not silently create a new empty directory somewhere else. That is how a
"fresh" profile appears on top of a real one.

If the user mentions lost data, a new machine, or an unexpected "seeded fresh"
message, the path resolution failed. Check both locations and offer to re-set
the path. Their data is still in the directory they first chose.

## First-run setup

If neither location resolves, ask once before anything else:

> "I don't see a personal-data location yet. Where should your profile,
> inspiration library, notebook, touchstones and song catalog live? Picking a
> place once means reinstalls keep everything.
>
> A: `~/.suno-song-creator-data/` (default)
> B: A custom path. Tell me where.
> C: Skip for this session. Templates only, nothing is saved."

For A or B: create the directory if needed. Suggest the user add
`export SUNO_SKILL_BACKUP_DIR="<path>"` to their shell config, and write
`references/.backup-path` as a fallback.

For C: work from the templates and warn that this session's additions will
not persist.

### Seeding missing files

For each file below, if `<backup>/<file>` exists, leave it alone. It is
authoritative. If it is missing, copy the template:

- `<backup>/user-profile.md` from `references/user-profile-template.md`
- `<backup>/inspiration-library.md` from `references/inspiration-library-template.md`
- `<backup>/notebook.md` from `references/notebook-template.md`
- `<backup>/touchstones.md` from `references/touchstones-template.md`

The catalog and playlist JSON files have no templates. They are absent until
the first catalog refresh.

After seeding, tell the user where the files are and that they will fill in
as you work together.

### First-run questions

Only on a true first run (the profile was just created from the template),
ask these two, one at a time. Keep it quick; the point is to get to writing.

**1. Existing songs.** Most people who reach for this skill already have
songs on Suno.

- If a Suno connector is present: "Want me to read your Suno library so I know
  what you've already written? It's free and stays in your private data
  folder." On yes, run the refresh below.
- If not: "Do you already have songs on Suno? Share your handle (the
  `@name` in `suno.com/@name`) and I can note it for later, or skip." Offer:
  public catalog only, public plus playlists (needs a browser tool and a
  signed-in session), starting fresh, or ask again next time.

Write the handle into the profile under `## Identity`. If the user is starting
fresh, write `none` so later sessions do not ask again. If they said "ask me
later", leave it blank.

**2. Default model.** See the Model selection section of `SKILL.md`. With a
connector, read `allowed_models` from `account-usage` and offer those labels.
Without one, offer the current family from `references/models/README.md`.
Write the choice under `## Identity > Default Suno model`, or leave it blank
if the user wants to decide per song.

## Using the profile

Read `<backup>/user-profile.md` before each session to learn the person's:

- preferred genres, sounds and style references
- recurring themes and subjects
- tonal preferences (plain-spoken or poetic, sincere or wry)
- structural habits (long songs, bridges, comedic turns)
- named people, pets and places that appear in their songs
- feedback patterns: what they keep asking to change
- Suno defaults: model, sliders, exclusions that work for them

If there is no profile, just write a good song, and afterwards offer: "Want me
to remember your style preferences for next time?"

When updating the profile, capture what worked (lines they loved, structures
that clicked), style patterns that emerged, references they lean on, themes
they return to, and language preferences ("more direct", "less cliche", "keep
it wry"). After a song is generated or finalized, add it to `Songs Created`
with the date, model, and the Suno link if there is one.

The profile format is in `references/user-profile-template.md`.

## Catalog and playlist refresh

The catalog tells the skill what the user has already written, so it does not
suggest territory they have covered.

### When to offer it

- No catalog file exists for a handle on file.
- The catalog is older than about 2 weeks. The sync date sits at the top of
  the profile.
- The user mentions a song or playlist you do not recognize.

Skip the check when the handle field is `none`, or when there is no handle and
no connector. Ask once, early, in one line, and move on if the user declines:
"Catalog last synced [DATE]. Refresh before we start, or use what we have?"
Do not grind through a full pull mid-session, and do not ask every session.

### With a Suno connector (preferred)

Follow "Library and playlist sync" in `references/connector.md`. It is free,
needs no browser, and covers private songs because the user is signed in.
Fetch lyrics only for songs that are new since the last sync unless asked to
re-pull everything.

### Without a connector

Two options, in this order:

1. **Let it build up.** Log each new song to the profile as you write it.
   This needs no tools and is fine for most people.
2. **Browser pull**, if the client has a browser-automation tool and the user
   wants their back catalog. Suno's pages are client-rendered, so plain page
   fetches return nothing useful. The approach that has worked:
   - Open `https://suno.com/@<handle>?page=songs` and scroll the song list to
     load every card.
   - Collect each `a[href*="/song/"]` link with its title, style, duration
     and counters.
   - For full lyrics and style, read each song page.
   - For private songs and playlists, the user must be signed in in that
     browser; open `https://suno.com/me/playlists`.

   This depends on Suno's page structure and will break when it changes. If
   it fails twice, stop and fall back to option 1.

### After any pull

1. Save to `<backup>/<handle>_songs_catalog.json` and, if playlists were
   pulled, `<backup>/<handle>_playlists.json`.
2. Summarize what is new: titles, playlists, public or private.
3. Ask whether to update the profile, then update `Songs Created`, the
   playlist groupings and the sync date.

**Privacy:** songs that are not public may be personal, for example gifts for
one person. Treat their content as sensitive. Never quote them, share them,
put them in public artifacts, or mention them outside the session unless the
user asks.

## Inspiration library

A personal collection of excerpts (lyrics, poetry, prose) that resonate with
the user. It calibrates the skill's instincts. It is not a quote bank.

File: `<backup>/inspiration-library.md`, seeded from
`references/inspiration-library-template.md`.

**During a session.** Read it at session start, after the profile. Treat it as
ambient calibration for early drafts:

- Let the entries shape the register of first drafts. If the library is full
  of plain-spoken second-person lyrics, lean that way unprompted. If it is
  dense with metaphor, start there.
- Let the `What resonates` notes inform the moves you reach for: turns,
  understatement, domestic detail, whatever recurs.
- **Do not quote, paraphrase or imitate entries.** The library trains taste.
  It is not source material.
- **Do not bring it up unprompted** ("this reminds me of your entry from...").
  Keep the influence implicit.
- If the user says "pull from my library" or "channel something I've saved",
  then surface relevant entries by tag or theme and discuss them openly.

**Capturing entries.** When the user shares a line that clearly lands for them
("god I love that line from...", "save this one"), offer to add it in the
template's format: source, type, excerpt, what resonates, tags, date. Press
lightly for a specific `What resonates`. "Good imagery" does not help later.
"The second line undercuts the sincerity of the first" does.

**Cross-linking.** If an entry's source song is also a touchstone, or the
other way round, offer to link them with the `Source touchstone:` field on the
library entry and `Linked library entries:` on the touchstone. Both are plain
text: entry titles, not file paths. Name both entries when you ask.

## Notebook

The user's own seeds: scenes, titles, characters, palettes, structural
experiments, observations. Things they want to write and have not yet.

File: `<backup>/notebook.md`, seeded from `references/notebook-template.md`.

**During a session.** Read it at session start and hold it:

- Do not open every session by suggesting seeds. Wait for "what should I work
  on?", "got any ideas?", or a feeling or topic that maps to an open seed.
- When the session's direction matches a seed, say so: "You've got an open
  seed about *the year of silent Mondays*. Does this belong there?"
- When a seed becomes a song, set `Status: spawned`, fill in `Spawned:`, and
  add the song to `Songs Created`.
- Never park a stale seed yourself. Only the user parks things.

**Capturing entries.** On "I want to write a song about...", "remind me to do
something with...", "there's a song in...", or "title that won't leave me
alone", offer to capture it. Press for specifics. "Something about my dad" is
worth less than "a song built around the grocery list still stuck to the
fridge." If a touchstone or library entry is steering the seed, record that
in `Linked touchstone`.

## Touchstones

Complete songs by other artists that the user returns to as whole-shape
references. A touchstone says: this whole song nails a feeling, a concept or a
structural move I want to be able to channel. A single great line belongs in
the inspiration library instead; cross-link the two.

File: `<backup>/touchstones.md`, seeded from
`references/touchstones-template.md`.

**During a session.** Read it at session start. Touchstones sit between the
library (ambient) and the notebook (on request):

- When the user describes a feeling or topic that matches a touchstone's
  `What it captures`, name it as a direction: "this is in the territory of one
  of your touchstones. Want me to lean toward that whole-song shape?" Use the
  entry's actual title.
- Do not quote, paraphrase or imitate the song. Touchstones calibrate
  macro-craft: arc, structural move, conceptual posture.
- Never put the artist or song name into a Suno style prompt.
- If a touchstone has linked library entries, the user may want both surfaced
  together.

**Capturing entries.** On "this whole song captures...", "I want to write
something with the shape of...", or "[Song] is doing exactly what I want to do
with...", offer to add it. Press for a precise `What it captures`. "Captures a
feeling" is not useful. "Tells the whole story through what the narrator
refuses to say" is. Then ask whether any specific lines belong in the
inspiration library, and cross-link if so.

## Local notes

If `<backup>/local-notes.md` exists, read it at session start. It holds
workflow notes specific to this user or machine: where song files are saved,
a local audio-analysis pipeline for reference tracks, preferred file naming,
anything the user has asked the skill to remember that is not about taste.

Where a local note and this skill disagree on a workflow detail, the local
note wins. It never overrides the cost and safety rules in
`references/connector.md`.

Offer to add a note when the user states a standing preference about process
("always save the package to my songs folder as <slug>.md").
