# Suno Song Creator

A Claude skill that turns the model into a co-writing partner for [Suno](https://suno.com).
You bring a feeling, a phrase, a person, a half-formed idea — Claude meets you where
you are and helps shape it into a complete package: lyrics with Suno section tags, a
style prompt tuned to the song's mood, arrangement notes, and a few variation hooks
you can swap in. The session is meant to feel like a jam, not an interview.

## What it does

- **Co-creative jam sessions.** Claude doesn't fire off a questionnaire. It reflects
  back what you've shared, throws out a few draft lines or a possible angle, and
  builds with you from there.
- **Real Suno output.** When the song clicks, you get lyrics formatted with Suno
  section tags (`[Verse]`, `[Pre-Chorus]`, `[Chorus]`, `[Bridge]`, `[Outro]`, etc.),
  a descriptive style prompt, an arrangement sketch, and optional variations
  (upbeat / stripped / dark / whatever's relevant).
- **Personal calibration over time.** The skill keeps four small files of personal
  data that build up across sessions and shape future drafts:
  - `user-profile.md` — your style DNA, recurring themes, named people/places, and
    a log of songs you've made together.
  - `inspiration-library.md` — excerpts (lyrics, poetry, prose) that resonate with
    you, used as ambient calibration during early drafting. Never quoted back at
    you; the point is to train taste, not to imitate.
  - `notebook.md` — your own seeds for songs you want to write but haven't yet.
    Surfaced when you ask "what should I work on?" or describe a feeling that
    matches an open seed.
  - `touchstones.md` — complete songs by other artists that you return to as
    whole-shape reference works.
- **Suno catalog sync.** If you give the skill your Suno handle, it can pull your
  published catalog (and optionally your playlists, including unpublished songs)
  via the Chrome extension, so future sessions know what you've already written
  and don't re-suggest territory you've covered.
- **Per-model prompt tuning.** Suno's models behave differently — v4.5 rewards
  comma-tag prompts; v5 and v5.5 reward narrative prose; v5.5 polishes
  aesthetics that v5 keeps raw. Pick a default model in your profile and the
  skill loads the matching guide before drafting any style prompt or lyrics.
  Override per-song whenever you want.

## Install

This skill follows the standard Claude skill layout — a directory with a `SKILL.md`
at the root.

### Claude Code / Claude Agent SDK

Clone or symlink into your skills directory:

```bash
git clone git@github.com:jayweiler/suno-song-creator.git ~/.claude/skills/suno-song-creator
```

The skill registers itself by being present. Restart Claude Code (or your agent)
so the skill loader picks it up.

### Cowork

Build a `.skill` bundle and install it via the Cowork app:

```bash
cd /path/to/parent && zip -r suno-song-creator.skill suno-song-creator/
```

Then attach the `.skill` file in a Cowork session — Claude will offer to install it.

## First run

The first time you load the skill, Claude will:

1. **Resolve a backup directory** for your personal data. Personal files (profile,
   library, notebook, touchstones, catalog JSON) live outside the skill install so
   that reinstalls and updates never wipe them. The skill checks two locations in
   order:
   - `$SUNO_SKILL_BACKUP_DIR` environment variable (recommended — survives
     reinstalls).
   - `references/.backup-path` — a one-line text file inside the install with an
     absolute path. Gets wiped on reinstall, so it's a fallback only.

   If neither resolves, Claude asks you where to put the data:
   - **A**: `~/.suno-song-creator-data/` (default)
   - **B**: a custom path you provide
   - **C**: skip — work from templates only, nothing persists this session

2. **Seed the four template files** at the backup location.

3. **Ask whether you already have a Suno catalog to import.** If you've been on
   Suno for a while, this is the moment to drop your `@handle` and let Claude
   pull your back catalog so it knows your existing work. Options:
   - Pull public catalog only.
   - Pull public catalog plus playlists (also captures unpublished songs you've
     shared with people directly; requires you to be signed in to Suno in your
     browser).
   - Skip — you're starting fresh.
   - Skip for now and ask again next session.

   If you choose to import, the pull happens through the Chrome extension and
   takes ~1–2 minutes.

4. **Ask which Suno model you usually use.** Options:
   - v5.5 (current latest, polished output, voice cloning, custom models)
   - v5 (best for raw / lo-fi / austere aesthetics that v5.5 over-polishes)
   - v4.5 / v4.5+ (predictable comma-tag prompts, Pro-only Add Vocals workflow)
   - Skip — decide per song

   Whatever you pick gets stored in your profile and tells the skill which
   per-model prompting guide to load when shaping style prompts and lyrics.
   You can override per song at any time ("use v5 for this one").

After that, Claude drops into the songwriting jam.

## Recommended setup

Add this to your shell config so the backup path survives reinstalls:

```bash
export SUNO_SKILL_BACKUP_DIR="$HOME/.suno-song-creator-data"
```

Then your data lives at `~/.suno-song-creator-data/` regardless of how many times
the skill itself gets updated or moved.

## Using it day to day

Just talk. Say things like:

- "Write a song for my partner about the way she always falls asleep with the
  light on."
- "I want something in the territory of late-period Bruce Springsteen — plain
  language, regret, a small turn at the end."
- "Save this lyric to my library: '…' — what resonates is the way the second
  line undercuts the first."
- "Got any open seeds I should work on?"
- "Refresh my Suno catalog — I published a few new ones this week."
- "Switch my default to v5 — I want rawer output going forward."
- "Use v5 for this one — it's meant to sound austere."

The skill triggers on a wide range of music-making language ("song," "lyrics,"
"track," "Suno," "verse," "chorus," etc.) and on emotional/experiential framings
that imply musical output. You don't need to invoke it explicitly.

## Picking a Suno model

The skill keeps a default model in your profile. Each Suno model rewards a
different prompt style:

- **v5.5** (Mar 2026, latest) — narrative prose works best; polished, stem-friendly
  output; Voices, Custom Models, My Taste, Replace Section.
- **v5** (Sept 2025) — narrative prose works; preserves raw / austere texture
  better than v5.5; pick this for lo-fi, dark ambient, raw black metal, austere
  folk, or anything where over-polish is a regression.
- **v4.5 / v4.5+** (May–Jul 2025) — comma-tag prompts work most reliably; cue
  control is less consistent than v5+; v4.5+ has Pro-only Add Vocals and Add
  Instrumentals workflows.

When the skill is about to draft a style prompt, it loads
`references/models/<your-default>.md` — the matching per-model guide tells
Claude the format the model rewards, character limit, cue reliability, and any
model-only features. Per-song overrides ("use v4.5 for this one") swap in a
different file just for that song without changing your default.

The full picker matrix is in `references/models/README.md`.

## Refreshing your Suno catalog

Once your handle is on file, Claude offers a refresh check at the start of any
session where the catalog is older than ~2 weeks (or where you mention a song or
playlist it doesn't recognize). The refresh uses the Chrome extension to scrape
your public profile and, optionally, your playlists.

Files written:

- `<backup>/<handle>_songs_catalog.json` — public catalog (titles, full style
  prompts, lyrics, plays/likes/comments).
- `<backup>/<handle>_playlists.json` — playlist memberships plus full data for
  any unpublished songs they contain.

The relevant sections of `user-profile.md` (`Songs Created`, `Playlist
Groupings`, sync date) get updated after each pull.

**Privacy note:** songs in playlists that aren't in the public catalog are
treated as private. The skill won't quote them externally, won't surface them in
public artifacts, and won't reference them outside the session unless you
explicitly ask.

## Repo layout

```
suno-song-creator/
├── SKILL.md                                  # the skill itself
├── README.md                                 # this file
├── LICENSE                                   # MIT
├── .gitignore                                # excludes personal data + dev cruft
└── references/
    ├── suno-prompting-guide.md               # cross-version songwriting craft
    ├── models/
    │   ├── README.md                         # picker matrix + decision rubric
    │   ├── v4-5.md                           # v4.5, v4.5+, v4.5-All
    │   ├── v5.md                             # v5
    │   └── v5-5.md                           # v5.5
    ├── user-profile-template.md              # seeded → <backup>/user-profile.md
    ├── inspiration-library-template.md       # seeded → <backup>/inspiration-library.md
    ├── notebook-template.md                  # seeded → <backup>/notebook.md
    └── touchstones-template.md               # seeded → <backup>/touchstones.md
```

Personal data files are deliberately *not* in the repo. They live at
`$SUNO_SKILL_BACKUP_DIR` (or whatever path you picked at first run) and the
skill reads/writes them directly.

## Updating the prompting guides

There are two layers to keep current:

- `references/suno-prompting-guide.md` — cross-version craft and shared
  vocabulary (genre / vocal / instrument / production tables, lyrics
  formatting, pitfalls, prompt templates by genre). Update when Suno-wide
  best practices shift.
- `references/models/<model>.md` — per-model guides (prompt format, cue
  reliability, character limits, model-only features). When Suno releases a
  new model, drop a new file here and update `references/models/README.md`
  (picker matrix + decision rubric).

Suno ships new versions and feature updates regularly; if you notice the
guides drifting from reality, edit them directly and open a PR. The skill
reads both layers before writing any style prompt, so keeping them current
matters.

## Contributing

Issues and PRs welcome. Useful directions:

- Better Suno catalog pulling (the current approach uses the RSC payload; if
  Suno changes their frontend, that may break).
- Additional reference docs for specific genres or production styles.
- Improvements to the first-run flow.

Please don't commit anything from your own backup directory — the `.gitignore`
covers the standard files but check before pushing.

## License

MIT.
