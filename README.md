# Suno Song Creator

An agent skill that turns your AI assistant into a co-writing partner for
[Suno](https://suno.com).

You bring a feeling, a phrase, a person, a half-formed idea. The assistant
helps shape it into a finished song: lyrics with Suno section tags, a style
prompt tuned to the current Suno models, an exclusion list, settings and
arrangement notes. The session is meant to feel like a jam, not an interview.

If your assistant has a Suno connector, it can then generate the song, wait
for it, check the take against what you asked for, and iterate, asking before
anything that spends credits. If it does not, you get a package to paste into
suno.com. Both paths are complete.

## Quick start

1. **Install the skill** for your assistant. See [Install](#install).
2. **Optional: connect Suno** so the assistant can generate directly. See
   [Connect Suno](#connect-suno-optional).
3. **Say what you want to write.** For example: "Write a song for my sister
   about the summer we shared a room."

On first use the assistant asks where to keep your personal files, whether to
read your existing Suno songs, and which model you usually use. After that it
just writes with you.

## What it does

- **Co-creative jam sessions.** No questionnaire. The assistant reflects back
  what you shared, offers a few draft lines or an angle, and builds with you.
- **A complete Suno package.** Tagged lyrics, a style prompt in the format the
  current models reward, exclusions, a settings line (model, Variety, Style
  Influence, Weirdness), arrangement notes and optional variations.
- **Direct generation with a Suno connector.** Generate, extend, cover, edit a
  single line, check a take's tempo, key and instrumentation, and read your
  library, from the chat. See the safety rules below.
- **Works without a connector.** Everything except the generating and
  checking steps is the same.
- **Per-model prompt tuning.** Guides for the current v6 family (v6, v6-wild,
  v6-mini), with sources and a clear list of what is unverified. Older guides
  are kept for reading old prompts.
- **Memory that stays yours.** A profile, an inspiration library, a notebook
  of song seeds and a list of touchstone songs build up across sessions. They
  live in a folder you choose, outside the skill.

### Safety rules when a connector is used

The skill is written so the assistant:

- asks before every call that spends credits, and says what it will make (a
  song call makes 2 songs)
- tells you which model label it is sending
- never turns on Max Mode unless you ask for it by name
- tells you the cost before stems, MIDI, sounds or a paid download
- sends an idempotency key so a retry cannot charge twice
- does not publish, rename, like or reorganize anything unless you ask

These are instructions to the assistant, not a technical lock. Your client's
own tool-approval prompts are the backstop; keep them on for Suno tools.

## Install

The skill is a folder with a `SKILL.md` at the root and a `references/`
folder. Download the release zip, or clone the repository for the newest
version (releases can lag behind `main`).

[**suno-song-creator.zip**](https://github.com/jayweiler/suno-song-creator/releases/latest/download/suno-song-creator.zip)

### Claude Code

```bash
unzip suno-song-creator.zip -d ~/.claude/skills/
```

or

```bash
git clone https://github.com/jayweiler/suno-song-creator.git ~/.claude/skills/suno-song-creator
```

Start a new session. The skill triggers when you mention songwriting, lyrics
or Suno.

### Claude (claude.ai and the desktop app)

Upload the zip as a custom skill: open **Customize**, then **Skills**, add a
skill and choose the upload option. In older builds the same screen is under
Settings, then Capabilities. The zip must contain the `suno-song-creator/`
folder at its top level. As of this writing custom skills need a paid Claude
plan with code execution turned on.

Personal files persist only where the assistant can write files between
sessions. Where it cannot, the skill still writes songs with you; the
cross-session memory may not persist.

### Codex CLI

Untested by the author. The format is the same Agent Skills format Codex
reads. OpenAI's docs give `~/.codex/skills/<skill-name>` for a user skill:

```bash
unzip suno-song-creator.zip -d ~/.codex/skills/
```

Check the current Codex skills docs if it does not load; the documented
location has moved between versions.

### Other assistants

Any assistant that reads the Agent Skills format (`SKILL.md` plus references)
should be able to load it. Put the folder wherever that tool looks for skills.
This has not been tested on Gemini CLI, ChatGPT, Cursor or others. If you get
it working, a note or PR is welcome.

### Compatibility

| Assistant | Status |
|---|---|
| Claude Code | Used by the author. Primary development target |
| Claude (claude.ai, desktop) | Used by the author. Memory depends on file access |
| Codex CLI | Untested. Format compatible |
| Gemini CLI, ChatGPT, Cursor, others | Untested |

## Connect Suno (optional)

Suno has an official MCP connector (beta, launched October 28, 2026). Per
Suno: it is available on all Suno plans, needs no API key, songs use the
credits on your plan, and every song lands in your Suno library.

The server URL is `https://api.suno.com/mcp`. You sign in with your Suno
account and approve the connection.

**Claude (claude.ai and desktop):** Customize, then Connectors, then add a
custom connector. Name it "Suno", paste the URL, connect and sign in. Some
Claude plans restrict custom connectors.

**Claude Code:**

```bash
claude mcp add --transport http suno https://api.suno.com/mcp
```

Then run `/mcp` inside a session to sign in.

**ChatGPT, Gemini, Cursor and others:** Suno's instructions are at
<https://suno.com/mcp> and <https://help.suno.com/en/articles/14551425>.
Follow those; they are the source of truth and the steps change.

The skill detects the connector by what it can do, not by its name, so it does
not matter what you call it. Third-party Suno MCP servers also exist. The
skill's connector guide is written for the official one and treats anything
else cautiously.

The setup steps above were checked against Suno's pages on 2026-11-21.

## Your personal files

The skill keeps what it learns about you in a folder you own, outside the
skill, so updates and reinstalls never touch it.

| File | What it holds |
|---|---|
| `user-profile.md` | Your style, themes, people and places, defaults, songs made |
| `inspiration-library.md` | Lines by others that land for you, and why |
| `notebook.md` | Your own seeds for songs you have not written yet |
| `touchstones.md` | Whole songs you return to as shape references |
| `<handle>_songs_catalog.json` | Your Suno songs, if you choose to sync them |
| `local-notes.md` | Optional workflow notes for your machine |

Set the location once so it survives reinstalls:

```bash
export SUNO_SKILL_BACKUP_DIR="$HOME/.suno-song-creator-data"
```

If the variable is not set, the assistant asks on first run and offers
`~/.suno-song-creator-data/` as the default.

The repository ships templates only. `references/user-profile-template.md`
shows the profile format with placeholders. Your filled-in copy is created in
your own folder. Do not put personal files in the skill folder, and do not
commit them. The `.gitignore` covers the standard names as a safety net.

Songs you have not published are treated as private. The skill is written not
to quote them or carry them into anything shared.

## Using it day to day

Just talk. For example:

- "Write a song for my partner about the way she always falls asleep with the
  light on."
- "Plain language, some regret, a small turn at the end."
- "Generate it." (With a connector. The assistant shows what it will send and
  asks first.)
- "Does take 2 have drums before the last chorus?"
- "Change the second line of the bridge to '...' and leave the rest."
- "Save this lyric to my library. What gets me is how the second line
  undercuts the first."
- "Got any open seeds I should work on?"
- "Use v6-wild for this one."

## Models

Since September 9, 2026 Suno runs on the v6 family, and earlier models are
retired for new generations:

- **v6**: the flagship. Polished, follows a brief. Pro and Premier.
- **v6-wild**: less predictable, for exploration. Pro and Premier.
- **v6-mini**: faster and lighter. All plans.

The skill loads `references/models/v6.md` before writing a style prompt. That
file lists its sources and marks what comes from Suno and what comes from
community reports. With a connector, the assistant reads which models your
account can use instead of guessing.

## Repository layout

```
suno-song-creator/
├── SKILL.md                              # the skill
├── README.md
├── LICENSE                               # MIT
├── .gitignore                            # safety net for personal files
└── references/
    ├── connector.md                      # Suno connector: tools, costs, safety rules
    ├── personal-data.md                  # where your files live and how they are used
    ├── suno-prompting-guide.md           # cross-version craft and vocabulary
    ├── models/
    │   ├── README.md                     # model picker
    │   ├── v6.md                         # v6, v6-wild, v6-mini (current)
    │   ├── v5-5.md                       # retired, reference only
    │   ├── v5.md                         # retired, reference only
    │   └── v4-5.md                       # retired, reference only
    ├── user-profile-template.md
    ├── inspiration-library-template.md
    ├── notebook-template.md
    └── touchstones-template.md
```

## Keeping the guides current

Suno ships changes often, and the connector is in beta.

- `references/connector.md` describes the connector's tools as of its "checked"
  date. The tool descriptions your assistant sees always win over the file.
- `references/models/<model>.md` holds per-model behavior. When Suno releases
  a model, add a file and update `references/models/README.md`.
- `references/suno-prompting-guide.md` holds craft that applies across models.

If a guide has drifted, edit it and open a PR. Please cite a Suno page for
facts about models, prices and limits, and label community reports as such.

## Contributing

Issues and PRs are welcome. Useful directions: notes from other assistants,
corrections to the model guides with sources, genre templates.

Never commit anything from your own personal-data folder.

## License

MIT.
