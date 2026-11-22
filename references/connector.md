# Suno Connector Reference

> Checked 2026-11-21 against the tool schemas of Suno's official MCP connector
> (beta, launched 2026-10-28) and Suno's own docs. The connector is in beta and
> will change. **The tool descriptions your client shows are the authority.** If
> a schema disagrees with this file, follow the schema and tell the user this
> file is stale.

Read this file before making any Suno tool call. If no Suno connector is
loaded, skip to [No connector: copy-paste flow](#no-connector-copy-paste-flow).

## Contents

1. [Detecting the connector](#detecting-the-connector)
2. [Cost and safety rules](#cost-and-safety-rules)
3. [What each tool costs](#what-each-tool-costs)
4. [Generating the song](#generating-the-song)
5. [Waiting for the result](#waiting-for-the-result)
6. [Checking a take against the brief](#checking-a-take-against-the-brief)
7. [Iterating on a take](#iterating-on-a-take)
8. [Library and playlist sync](#library-and-playlist-sync)
9. [Voices, workspaces, downloads, stems, MIDI](#voices-workspaces-downloads-stems-midi)
10. [Errors](#errors)
11. [No connector: copy-paste flow](#no-connector-copy-paste-flow)
12. [Sources](#sources)

## Detecting the connector

Detect by capability, not by one exact name. Clients prefix MCP tools
differently (`mcp__suno__audio-create`, `mcp__claude_ai_Suno__audio-create`,
plain `audio-create` and so on), and the user picks the connector's name.

The official connector is present when a Suno server offers tools whose names
end in `audio-create`, `audio-wait`, `audio-retrieve` and `account-usage`. If
your client defers tool loading, search its tool list for "suno" before
deciding the connector is absent.

Then make one free call, `account-usage`, and keep the answer for the session:

- `allowed_models`: the model labels this account may send.
- `features`: what the plan grants (custom generation, covers, extensions,
  mashups, stems, MIDI, uploads, downloads, advanced controls, voices, speech).
- the Suno account block: plan and remaining song credits, download credits
  and visuals credits, each a separate balance.

Tell the user in one line that the connector is live and what is left, for
example: "Suno connector is connected. Credits look fine." Do not read out the
plan details unless asked.

**Unofficial Suno MCP servers** exist (third-party wrappers that need an API
token). Their tools have different names and parameters. This file does not
describe them. If that is what is loaded, read its tool descriptions, apply the
cost and safety rules below, and do not assume any parameter from this file.

**No Suno tools at all:** say so once and use the copy-paste flow. Do not
invent a tool call. The README explains how to connect one.

## Cost and safety rules

These hold for every session. They come from the connector's own tool
descriptions, plus the rule that the user's credits and account are theirs.

1. **Confirm before every call that spends credits.** Show what will be sent
   (title, model label, style, exclusions, controls, and that the agreed lyrics
   go as written), say what it makes and that it is charged to their credits,
   then wait for a clear yes. One yes covers one call.
2. **A song call makes 2 songs.** Through the connector a signed-in user gets
   2 songs per `audio-create` call, charged to their credits. Say so before the
   first call. Suno's v6 FAQ puts a standard generation at 10 credits for the
   two songs; treat that as Suno's stated price, not a guarantee, and read
   `account-usage` if the user wants the exact balance before and after.
3. **Several calls need their own confirmation.** If the plan is three
   variations, say "that is 3 calls, 6 songs" and get a yes for the batch.
   Never fan out on your own.
4. **Name the model label.** If the user has not picked one, tell them which
   label you are sending, or that their plan default applies because you are
   omitting it. Only send labels listed in `allowed_models`.
5. **Max Mode only when asked for by name.** Never set `max_mode` yourself to
   improve a song. It costs a multiple of the song's credits. You may mention
   that it exists and what Suno says it is for; the user decides.
6. **Say the cost before stems, MIDI, sounds and paid downloads.** See the
   table below.
7. **Downloads use a two-step confirm.** Call `audio-download` without
   `confirm`. If the answer is `requires_credit`, ask the user, and only on
   their yes call again with `confirm: true`.
8. **Send an `idempotency_key` on every creation call, and reuse it on a
   retry.** Make one key per intended generation (for example
   `<song-slug>-take1-<date>`). After a timeout or error, retry with the same
   key and the same parameters so a retry cannot pay twice. A new key means a
   new charge, so it needs a new yes.
9. **Never retry a generation by calling again with changed text to get past a
   refusal.** Tell the user what was refused.
10. **Leave the account alone unless asked.** Do not publish or unpublish
    (`audio-settings`), change remix permissions, like or dislike, edit a
    song's title, lyrics or caption (`audio-edit`), or create or change
    playlists and workspaces unless the user asked for that specific change.
    Publishing in particular is only ever on an explicit request.
11. **The user's library is private.** Unpublished songs may be personal.
    Never quote them, list them, or carry them into any shared file or public
    artifact unless the user asks.

## What each tool costs

From the tool descriptions, checked 2026-11-21. Prices are Suno's and can
change; the live tool description wins.

| Action | Tool | Cost stated by the connector |
|---|---|---|
| Plan, credits, allowed models | `account-usage`, `account-me` | Free |
| List or search the library, playlists, workspaces, voices | `library-list`, `library-search`, `library-liked`, `playlists-list`, `playlists-list-clips`, `workspaces-list`, `voices-list` | Free |
| Read one song, wait for one | `audio-retrieve`, `audio-wait` | Free |
| Ask about a finished song | `audio-listen`, `audio-listen-wait` | Free. 5 open questions at once, 200 per UTC day |
| Timing map and audio description | `audio-analysis` | Free |
| Word timings | `audio-lyrics-alignment` | Free |
| Join an extension chain | `audio-full-song` | Free |
| Upload your own audio | `audio-upload-create`, `audio-upload-complete` | Free, no count limit |
| New song | `audio-create` | Song credits, 2 songs per call |
| Cover, extension, mashup | `audio-covers-create`, `audio-extensions-create`, `audio-mashups-create` | Song credits |
| Max Mode on any of the above | `max_mode: true` | A multiple of the song's credits. The multiple is not stated |
| Sound effects and loops | `sounds-create` | 2 sounds per call, 1 credit each. Needs Pro or above |
| Stems | `audio-stems-create` | `two`: 10 credits (2 takes at 5). `twelve`: 50 credits (2 takes at 25) |
| Song to MIDI | `audio-midi` | 10 credits, charged when it finishes. Needs a plan with Studio. A song already transcribed is free |
| Download mp3 or wav | `audio-download` | May spend 1 download credit. Uses the confirm flow |
| Speech | `speech-create` | Needs the speech grant. Cost not stated; confirm first |
| Images, video, sequences | visuals tools | A separate Visuals credit balance. `dry_run: true` prices a call without spending |

## Generating the song

Do this only after the song package is agreed in the session. The jam comes
first. The connector does not replace the writing.

### Custom mode (the default for this skill)

Finished lyrics always go through custom mode, so they are sung as written.
Map the package to `audio-create` like this:

| Package part | Parameter | Limit and notes |
|---|---|---|
| Lyrics with section tags | `lyrics` | 5000 characters |
| Style prompt | `style` | 1000 characters. Tags or prose both pass; see the model guide |
| Exclusions | `negative_style` | 1000 characters. Needs the advanced-controls grant |
| Title | `title` | 256 characters |
| Model | `model` | A label from `allowed_models`, or omit for the plan default |
| Variety | `controls.variety` | Whole number 0 to 4: Off, Normal, High, Extra, Max |
| Style Influence | `controls.style_influence` | 0 to 1 (the site shows 0 to 100%) |
| Weirdness | `controls.weirdness` | 0 to 1 |
| Vocal gender | `vocal_gender` | `male` or `female`. Needs the advanced-controls grant |
| Target length | `duration` | 15 to 360 seconds. A target, not a guarantee |
| A saved voice | `voice_id` | From `voices-list`. A voice always sings, even with `instrumental` |
| Filing | `workspace_id` | From `workspaces-list` |
| Retry safety | `idempotency_key` | Always send one |

Rules that bite:

- **Do not send `description` with `lyrics` or `style`.** Mixing simple and
  custom mode returns a 400.
- **Set `controls.variety` to 0 when the style text is engineered.** Above 0,
  Suno reinterprets the style tags before the song is made. 0 keeps them as
  written. Omit a slider to keep the model default; 0 is a real setting, not
  "unset".
- **Be careful with `duration`.** When it is set, `duration_strict` is on by
  default and hard-caps the clip, so a song with more lyrics than fit is cut
  off. For a full song with finished lyrics, omit `duration`, or set
  `duration_strict: false` to keep it as a soft target.
- **`controls.audio_influence` on `audio-create` only works with a
  `voice_id`.** Without one it returns 400.
- **Instrumentals:** `instrumental: true` with a `style` and no lyrics.

### Simple mode and plain-language edits

`description` alone (3000 characters) lets Suno write the lyrics and style. Use
it when the user wants a quick sketch, not for a song you wrote together.

With `reference_audio_ids` (up to 10 of the user's finished songs), simple mode
can remix, sample, remaster, restyle, mash up, split out a stem, or edit one
song. Say what to do in `description` and name songs by title. Never put an id
or a link in `description`. No `lyrics` or `style` in this mode.

### The confirmation message

Before the call, show something like this and wait:

```
Ready to generate on Suno:
- Title: <title>
- Model: v6 (your default)        <- or the label you are sending
- Variety: Off, Style Influence 0.8, Weirdness: model default
- Exclude: <negative style>
- Lyrics: as agreed above, sent as written
This makes 2 songs and is charged to your Suno credits. Go ahead?
```

## Waiting for the result

Creation calls return at once. Then:

1. Call `audio-wait`. If the creation returned `clip_ids`, pass all of them in
   `ids` in one call (12 at most). It returns when the clip is `streaming`
   (playable), `complete` or `error`, or after about 50 seconds still pending.
   Still pending means call `audio-wait` again.
2. Use `until: "complete"` only when you need the finished file or its
   duration, for example before a cover, an extension, stems or analysis.
3. `audio-retrieve` reads one clip by id. Use it for a single status check or
   to read a song's stored settings.
4. Give the user each song's `song_url` as soon as it is playable.

A song object carries, among other fields: `id`, `title`, `status`,
`visibility`, `song_url`, `created_at`, and `metadata` with the stored `style`,
`negative_style`, `model`, `controls` and `duration`. Lists leave out the
lyrics; `audio-retrieve` returns them for the user's own songs.

## Checking a take against the brief

The user's ears are the judge. These tools help you say something useful
before they listen, or settle a specific doubt. Both are free.

**`audio-listen`** asks Suno's music agent a question about a finished clip.
Ask about what the brief cared about, one or two things at a time:

- "Do drums enter before the final chorus? Is there any vocal harmony in the
  choruses? End with a JSON block with bpm and key."
- "Does the dynamic stay flat from the first verse to the last chorus, or does
  it build?"

It returns an id at once. Read the answer with `audio-listen-wait`, and call
that again if it still says `processing`. At most 5 questions open at a time.

**`audio-analysis`** returns a tagging model's description of the audio
(`terms`, `labels`: voice, instruments, genre, mood, polish) plus tempo,
meter, key and section boundaries. The description comes from the audio, not
the prompt, so it is a fair check. Poll until `status` leaves `processing`.
Parts can arrive separately and `key` can be null.

How to use them:

1. After both takes are complete, offer the check in one line. Do not run it
   unasked on every take.
2. Compare the answers to the brief point by point: tempo, instrumentation,
   vocal character, dynamics, the things in the exclusion list.
3. Report plainly, including misses: "Take 1 holds the dry vocal but adds
   strings in the last chorus. Take 2 has no strings but runs about 8 BPM
   fast."
4. A listening model can be wrong. Present its answer as a read, not a fact,
   and never let it overrule what the user hears.

## Iterating on a take

Each of these is a new charged generation unless marked free. Confirm first.

- **Change a word, a line or a section.** `audio-create` with
  `reference_audio_ids: [<song id>]` and a `description` such as "in <Title>,
  change '<old line>' to '<new line>'". Send no `duration`, `instrumental` or
  `vocal_gender`, which make the song regenerate whole. Changing words in more
  than 5 places re-sings the whole song as a cover.
- **New take, same package.** Call `audio-create` again with a new
  idempotency key, changing one variable at a time.
- **New style, same song.** `audio-covers-create` on the source id. The
  source's lyrics are not carried over: pass `lyrics` to keep the vocals, or
  the cover comes back instrumental.
- **Make it longer.** `audio-extensions-create` with `continue_at` in seconds.
  The new clip holds only the continuation. Join the chain with
  `audio-full-song` (free, up to 20 segments), naming the last extension.
- **Blend two songs.** `audio-mashups-create` takes exactly two finished
  parents.
- **Start from the user's own audio.** `audio-upload-create` first, as soon as
  they say they want to use a recording, then cover or extend the upload.
- **Start from a MIDI score.** `midi-upload-create`, then `audio-create` in
  simple mode with `midi_upload_id`. The schema says this needs the `v6-wild`
  model label.

## Library and playlist sync

When the connector is present, use it for the catalog refresh instead of
browser scraping. Every call here is free.

1. `account-me` for the user's handle, if the profile does not have it. (An
   API-key connection cannot call it.)
2. `library-list` with `limit: 50`, following `next_cursor` until it is
   absent. Filters: `visibility`, `liked`, `origin`, `workspace_id`.
3. Compare ids against the saved catalog file. For each new song, call
   `audio-retrieve` to get the lyrics, since lists leave them out.
4. Playlists, only if the user asked for them: `playlists-list`, then
   `playlists-list-clips` for each.
5. `library-search` finds a song the user names by title or style.
6. Write the results to the user's personal-data directory, never into the
   skill folder. See `references/personal-data.md`.

`library-liked` lists songs by anyone that the user liked. Those are not the
user's work. Do not add them to the catalog.

## Voices, workspaces, downloads, stems, MIDI

- **Voices.** `voices-list` (collection `own` or `loved`) gives ids to pass as
  `voice_id`. If the user wants a new custom voice, call `voices-start` and
  give them the returned suno.com link. Never ask them for recordings.
- **Workspaces.** `workspaces-list` to find one; pass its id as `workspace_id`
  to file a new song there. Create or re-file only on request.
- **Downloads.** The two-step confirm in the rules above. `mp3` or `wav`.
- **Stems.** `audio-stems-create` with `mode: "two"` or `"twelve"`, then poll
  `audio-stems-retrieve`. Say the price first. A stem cannot be split again.
- **MIDI.** `audio-midi` on a finished song, called again until `status` is
  `ok`. Say the price first.

## Errors

| Answer | Meaning | What to do |
|---|---|---|
| 400 | Bad combination or value | Read the message. Usual causes: `description` mixed with `lyrics` or `style`, a slider out of range, `duration_guidance` without `duration` |
| 402 | Out of credits | Tell the user. `account-upgrade` returns a suno.com link; offer it, do not push it |
| 403 on a control, `negative_style` or `vocal_gender` | The plan lacks advanced controls | Tell the user, drop that field with their okay, and put the exclusion in the copy-paste notes instead |
| 403 on a model label | The plan does not hold that model | Use a label from `allowed_models` |
| 403 `user_principal_required` | The tool needs a suno.com sign-in, and this connection uses an API key | Say that tool is unavailable on this connection |
| 404 | Song or voice not visible to this account | Check the id |
| 409 on an idempotency key | Key reused with different input, or the first request is still running | Retry with the same key and same input, or wait |
| 429 | Rate or daily limit | Stop and tell the user. Do not loop |
| 503 | Source song still finishing | Wait with `audio-wait`, then retry with the same key |

After 3 failed attempts at the same step, stop, report the state and offer the
copy-paste flow.

## No connector: copy-paste flow

Without a connector the skill still does its whole job. Deliver the package so
it can be pasted into suno.com in Advanced (Custom) mode:

1. **Lyrics** box: the tagged lyrics.
2. **Styles** box: the style prompt.
3. **Exclude Styles** (under Advanced Options): the exclusion list.
4. **Settings line**: model, Variety, Style Influence, Weirdness, and any
   vocal gender or duration choice, written as the site labels them.
5. **Title**.

Then ask the user to paste back what they heard, or the song link. If they
want checks on tempo or structure and have no connector, say that needs a
listen from them.

If the user would like one-step generation, point them to the README section
on connecting Suno. Do not pressure; the copy-paste flow is complete.

## Sources

Checked 2026-11-21.

- Tool schemas of Suno's official MCP connector, as loaded in the client. The
  primary source for every tool name, parameter, limit and price in this file.
- Suno release notes, "Suno MCP (beta)", Oct 28, 2026: available on all Suno
  plans, songs use your credits. https://suno.com/release-notes
- "How do I connect Suno MCP to an AI assistant?" (server URL and setup
  steps). https://help.suno.com/en/articles/14551425
- Suno MCP page. https://suno.com/mcp
- v6 FAQ (10 credits for a two-song generation, Max Mode, Variety).
  https://help.suno.com/en/articles/13924481

Not verified: the Max Mode credit multiple, the price of speech, and whether a
plain-language edit through `reference_audio_ids` is priced like a full
generation. Confirm with the user and check `account-usage` rather than
quoting a number.
