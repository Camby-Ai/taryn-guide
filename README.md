# Taryn Setup Guide

Taryn is a manager for DJs. Most DJs lose gigs in the admin, not in the booth: the pitch that never got a follow up, the press kit that is two years old, the promoter a friend could have introduced. Taryn keeps one pitch tracker of every venue, promoter and label, drafts the emails, gets each music submission ready, keeps the press kit current, runs the follow ups, and checks who you already know before you pitch cold. You make the music and press send. This guide sets her up on Hermes Agent, a free and open source agent from Nous Research that runs on your own machine.

The software is free. The model behind it is not always free, and the real work is writing down the facts about your own career that only you know.

**Checked on 2026-11-21.** Commands and limits change. Sources are at the bottom.

**How this guide was made:** every command below was copied from the Hermes Agent documentation on that date. We have not test-run them on a fresh machine. If a step does not behave the way it says, see "If a step breaks" near the end: your AI can patch it for you in a minute.

## What Taryn does

- Keeps one pitch tracker: every venue, promoter and label, what you sent, when, and what is due next.
- Drafts each pitch email from your own facts, so nothing in it is made up.
- Gets each music submission ready: the right track, the link, and the label's own submission rules.
- Keeps your press kit current: bio in three lengths, links, photos list and past gigs.
- Checks who you already know at each room before you pitch cold, and drafts the ask for an intro.
- Turns your footage notes into bio lines and content ideas.
- Tells you each weekday which follow ups are due, with a draft for each.
- Does not send, submit, post or book. You do.

## What you need before you start

| You need | Why | Cost | Checked |
|---|---|---|---|
| A computer that stays on: macOS, Linux, Windows or WSL2 | The follow up check only runs while Hermes' gateway is running | Yours already | 2026-11-21 |
| Git and curl | The installer uses them | Free | 2026-11-21 |
| Hermes Agent | Runs Taryn | Free, MIT licence | 2026-11-21 |
| A model with at least 64,000 tokens of context | Hermes rejects smaller models at startup | Depends on the provider you pick. Not measured here | 2026-11-21 |
| Your links and facts | Music links, past gigs, releases, photos, the bio you use now | Yours already | 2026-11-21 |
| A list of people you know in the scene | So she can look for a warm intro before a cold pitch | Yours already | 2026-11-21 |
| Telegram (optional) | To get the follow up check on your phone | Free | 2026-11-21 |

On a brand new install the setup wizard may offer a "Free, start now" choice that needs no account. The Hermes docs say it is shown only when it is available, so do not plan around it.

## The honest timeline

| When | What | Your time |
|---|---|---|
| Day 1 | Install Hermes and get one normal chat working | The Hermes docs say under 5 minutes. Allow 30 |
| Day 1 | Create Taryn and give her a desk folder | 20 minutes |
| Day 1 to 2 | Write your artist file and your first 20 targets | 1 to 2 hours. This is the part that decides everything |
| Day 2 | Turn on the follow up check | 20 minutes |
| Week 1 | Read the check, send what you approve, tell her what happened | 10 minutes a day |

The slow part is not the install. It is the facts. Taryn can only pitch what is true about you, so the artist file has to be right before the first email is drafted. Start with 20 targets you would be proud to play or sign with, not every club in the country.

Two limits to know before you start:

- She drafts and you send. A pitch from your own inbox, in your own name, gets read. Automated bulk email to venues and labels gets you blocked.
- Every label and promoter has its own rules for demos. Paste those rules into the tracker and she will follow them. She cannot guess them.

## Setup, in this order

### 1. Install Hermes and get one chat working

Do not add anything else until a plain conversation works. The Hermes quickstart is blunt about this, and it is right.

On macOS and Windows the Hermes docs recommend the Hermes Desktop installer from https://hermes-agent.nousresearch.com/. For a command line only install on macOS, Linux or WSL2:

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
```

Reload your shell, pick a model, and start a chat:

```bash
source ~/.zshrc
hermes setup
hermes
```

You should see: a welcome banner naming your model, and a reply to a simple question. If replies are empty or broken, run `hermes model` again and confirm the provider, the model and the login. If anything else looks off, run `hermes doctor`.

### 2. Create Taryn

Every agent in Hermes gets its own profile: its own settings, memory and identity. Never point two agents at the same profile.

```bash
hermes profile create taryn --description "DJ manager. Keeps the pitch tracker for venues, promoters and labels, drafts pitch emails and music submissions, keeps the press kit current, runs follow ups, finds warm intros. Never invents a gig, a number or a quote. The owner sends everything."
```

Creating the profile also creates a `taryn` command. Check it:

```bash
taryn chat
```

You should see: the same kind of chat as step 1, now running as the `taryn` profile.

### 3. Give Taryn her identity

In Hermes, who an agent is lives in a file called `SOUL.md` inside that agent's home. For a profile named `taryn` that is:

```text
~/.hermes/profiles/taryn/SOUL.md
```

Hermes creates a starter file there. Replace its contents with the text in "Taryn's instructions" below. Keep it about who she is and how she talks. Your career facts and your rules go in different files, in the next step.

You should see: after starting a new `taryn chat`, she introduces herself as your manager when asked who she is.

### 4. Build her desk: the rules, your facts, the tracker

Hermes has a built-in memory, but the docs give it about 2,200 characters. That is a few paragraphs, not a career. So your facts and your pitches live in plain files that Taryn reads and updates.

Make a folder for her to work from:

```bash
mkdir -p ~/taryn-desk/press-kit ~/taryn-desk/drafts ~/taryn-desk/footage
```

Then add three files. Hermes loads `AGENTS.md` from the folder you start a chat in, and Taryn reads and updates the others as she works.

`~/taryn-desk/AGENTS.md`, the rules:

```text
# How Taryn works this desk

- artist.md holds every fact about me. Pitch only what is written there. If a fact is missing, ask me once and write my answer in the file. Never guess.
- tracker.md is the pitch tracker: one row per venue, promoter or label. Update it every time I tell you something was sent or answered.
- network.md lists people I know in the scene and where they are connected. Before any cold pitch, check it for a warm intro and tell me who to ask.
- press-kit/ holds the bio in three lengths (one line, short, full), my links, my photo list and my past gigs. Keep it matching artist.md.
- drafts/ holds every email and submission you write, one file each, named for the target.
- footage/ holds my notes on clips and photos I have. Use them for bio lines and content ideas. Do not describe footage I have not listed.
- A label's or promoter's own submission rules go in the Notes column. Follow them exactly.
- A follow up is due 7 days after a pitch with no answer. Stop after 2 follow ups and mark the row "resting".
- Never send, submit, post or book. Draft it, save it in drafts/, and show it to me.
- Never invent a gig, a release, a number, a quote or a name.
```

`~/taryn-desk/artist.md`, your facts:

```text
# {Artist name}

- Based in: {city}
- Sound: {two or three plain words, and what a set feels like}
- Links: {music, mixes, social, website}
- Releases: {title, label or self-released, date}
- Past gigs: {venue or event, city, date}
- Residencies or regular nights: {where, how often}
- Press and quotes: {who said it, where, date. Leave blank if none}
- What I want next: {the rooms, festivals and labels I am aiming at}
- What I will not do: {gigs, fees or terms I turn down}
- Booking contact: {the email pitches should come from}
```

`~/taryn-desk/tracker.md`: start it empty with one header line.

```text
| Target | Type (venue, promoter, label) | Contact | Warm intro | What was sent | Date sent | Status | Next step and date | Notes |
|---|---|---|---|---|---|---|---|---|
```

You should see: when you run `taryn chat` from inside `~/taryn-desk` and ask how she tracks a pitch, she answers from `AGENTS.md` and the tracker's columns.

### 5. Write your first 20 targets

This is the step everybody rushes, and it is the one that makes Taryn useful. Fill in `artist.md` yourself first. Then run `taryn chat` from inside `~/taryn-desk` and send her this:

```text
Read artist.md and tell me which facts are missing or thin. Ask me for them one at a time and write my answers into the file. Then ask me for 20 targets: the venues, promoters and labels I most want. For each one ask me three things: who the contact is if I know, their submission or booking rules if I know them, and anyone I know who is connected to them. Add one row per target to tracker.md and add the people to network.md. If I do not know something, write "unknown" and move on.
```

You should see: a complete `artist.md`, 20 rows in `tracker.md`, and a `network.md` with the people you named.

### 6. Schedule the follow up check

Hermes has a built-in scheduler. Give the job Taryn's folder so she reads your rules and your tracker when she runs. The folder must be written as a full path.

```bash
hermes -p taryn cron create "weekdays at 10am" \
  "Follow up check. Read tracker.md. List every row where a follow up is due today or overdue, oldest first, with what was sent and when. For each one draft a short follow up and save it in drafts/. Then list any row with no pitch sent yet that has a warm intro in network.md, and say who to ask. Send nothing. Keep it under 200 words." \
  --workdir /Users/you/taryn-desk \
  --name "Follow up check"
```

Schedules run from a gateway, and each profile has its own. Start Taryn's, and check that her scheduler is alive:

```bash
taryn gateway start
hermes -p taryn cron status
```

`taryn gateway install` keeps it running after a restart.

By default the check is saved on your machine. To get it on your phone, connect Telegram first with `hermes gateway setup`, then create the job with `--deliver telegram` added.

One thing to check on your machine: whether your times come out in your own timezone. Hermes reads the schedule in the profile's `timezone:` setting. Add `--timezone America/Los_Angeles` (or your zone) to pin it.

You should see: a follow up check waiting for you the next weekday morning.

### 7. Run your first pitch, by hand

Pick one real target from the tracker. From inside `~/taryn-desk` run `taryn chat` and send her this:

```text
Pick the row for {target} in tracker.md. First check network.md and tell me if anyone I know is connected to them. If someone is, draft the short message asking that person for an intro. If no one is, draft the pitch email using only facts from artist.md, and follow the rules in the Notes column. Save the draft in drafts/ and show it to me. Do not send anything.
```

You should see: one draft in `drafts/`. Every fact in it is in `artist.md`, and it follows that target's own rules.

## Taryn's instructions

Paste this into `~/.hermes/profiles/taryn/SOUL.md`. Change the parts in braces.

```text
# Who you are

You are Taryn. You manage {artist name}, a DJ and producer.

Your job is to work the rooms so your artist can make the music: every venue, promoter and label is in one pitch tracker, every pitch gets a follow up, the press kit is always current, and no cold pitch goes out when a warm intro was sitting in the network.

# How you work

- One tracker. Every pitch, answer and next step is written there the same day.
- You pitch only what is true. Every fact in an email comes from artist.md. You never invent a gig, a release, a number, a quote or a name.
- Before a cold pitch you check who your artist already knows. A warm intro beats a cold pitch.
- You follow each label's and promoter's own rules for demos and bookings, exactly.
- You keep pitches short: who the artist is, why this room or label, one link, one ask.
- You follow up twice, a week apart, then let it rest. You never nag.
- You draft. You never send, submit, post or book. Your artist does.
- If you do not know something, you ask once and write the answer down.

# How you talk

- Fast, confident, a little cheeky. Nightlife energy, never rude.
- Lead with what is due today.
- One line per target: who, what was sent, what you would do next.
- When you ask a question, ask exactly one.
```

This persona was written from Taryn's job description for this guide. It is not a copy of the instructions she runs on in our own team.

## First test

Run `taryn chat` from inside `~/taryn-desk` and send her this:

```text
Which rows in tracker.md have no pitch sent yet? For each one, tell me if there is a warm intro in network.md, and write a one line pitch angle using only facts from artist.md. Then tell me which facts in artist.md are still missing. Send nothing.
```

A good answer lists only targets that have a row in `tracker.md`, takes every fact from `artist.md`, names a real person from `network.md` or says there is none, sends nothing, and lists the missing facts instead of filling them in.

If she invents a gig, a number or a quote, tighten the "never invent" line in `SOUL.md` and add the real fact to `artist.md`.

## When it goes wrong

| What you see | Why | Fix |
|---|---|---|
| Empty or broken replies | Model or login is wrong | `hermes model`, then a plain chat before anything else |
| She does not know your career | The chat was not started in `~/taryn-desk` | Start `taryn chat` from that folder |
| She made up a gig, a number or a quote | The fact was missing and she guessed | Fix `artist.md`, and tighten the "never invent" line in `SOUL.md` |
| A pitch ignores a label's demo rules | The rules are not in the tracker | Paste them into that row's Notes column |
| She pitched cold when you knew someone | The person is not in `network.md` | Add them, with who they are connected to |
| No follow up check | Taryn's gateway is not running, or the job is missing its folder | `hermes -p taryn cron status`, `taryn gateway start`, and recreate the job with `--workdir` |
| The check arrives at the wrong hour | The schedule is running in another timezone | Recreate the job with `--timezone` set to your zone |
| Anything else | | `hermes doctor` |

## If a step breaks

Hermes changes fast, so a command here can drift from what your version expects. You do not need us to fix it. Paste this into Hermes itself, or into Claude or ChatGPT, with the error underneath:

```text
I am following a setup guide for a DJ manager agent on Hermes Agent. This step failed. Here is the step, the exact command I ran, and the full error. Check the current Hermes Agent docs at https://hermes-agent.nousresearch.com/docs, tell me what changed, and give me the corrected command.
```

`hermes doctor` is worth running first. It catches most setup problems on its own.

## What it costs to run

- Hermes Agent: free, MIT licence.
- The model: billed by whichever provider you choose in `hermes model`. Every follow up check and every draft is model usage, and each one reads your tracker. Not measured here, so no number is given.
- More targets means more to read on every run. That is one more reason to start with 20.

## Want it set up for you?

CAMBY Ai sets Taryn up and runs her for you: https://mikecamby.com

## Credit

Taryn runs on [Hermes Agent](https://github.com/NousResearch/hermes-agent), built by Nous Research and released under the MIT licence. Hermes is not ours. This guide is a walkthrough for using it to manage a DJ career. Not affiliated with or endorsed by Nous Research, Telegram, SoundCloud or any venue, promoter or label.

## Sources

All read on 2026-11-21 from the Hermes Agent repository and its documentation.

- https://github.com/NousResearch/hermes-agent : install command, first-run commands, licence.
- https://hermes-agent.nousresearch.com/docs/getting-started/quickstart : install paths, `hermes setup`, the 64,000 token minimum, the "Free, start now" option, `hermes doctor`.
- https://hermes-agent.nousresearch.com/docs/user-guide/profiles : `hermes profile create`, `--description`, one profile per agent, the per-profile command.
- https://hermes-agent.nousresearch.com/docs/user-guide/features/personality : `SOUL.md`, where it lives and what belongs in it.
- https://hermes-agent.nousresearch.com/docs/user-guide/which-file-does-what : `SOUL.md` versus `AGENTS.md`.
- https://hermes-agent.nousresearch.com/docs/user-guide/features/memory : the built-in memory and its size limit.
- https://hermes-agent.nousresearch.com/docs/user-guide/features/cron : `cron create`, "weekdays at", `--workdir`, `--deliver`, `--name`, `--timezone`.
- https://hermes-agent.nousresearch.com/docs/user-guide/messaging : `hermes gateway setup`, `start`, `install`, `status`.
- https://hermes-agent.nousresearch.com/docs/user-guide/multi-profile-gateways : one gateway per profile, `<profile> gateway start`, `hermes -p <profile> cron status`.
