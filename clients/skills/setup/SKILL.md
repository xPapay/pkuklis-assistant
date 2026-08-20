---
name: setup
description: First-run setup for the client operations assistant. Connects accounts, learns the user's writing style from their sent mail, discovers clients and tracker boards, and sets approval preferences. Use on first install, or when the user wants to redo onboarding, add a client, or correct their style profile.
argument-hint: "(no arguments) or 'style' | 'clients' | 'preferences' to redo one part"
---

# /clients:setup

> If you see unfamiliar placeholders like `~~email`, see [CONNECTORS.md](../../CONNECTORS.md).

Set up the assistant for a new user. Runs once after install; individual sections can be
re-run later.

**Assume the user is not technical.** No file editing, no configuration syntax, no MCP
vocabulary. Ask only questions that cannot be answered by looking. Prefer discovering a fact
and asking the user to confirm it over asking them to supply it.

Work through the steps in order. Show progress so the user knows how much is left.

## Where setup output goes

Everything this skill produces is written into the **current Cowork project folder**:

```
clients/<client-slug>.md     one working file per client
style-profile.md             how the user writes
preferences.md               approval rules and working rhythm
```

These files are working context, not a source of truth. Roadmaps, deadlines, and status
live in `~~tracker` and `~~email`. These files hold only what those systems cannot express:
the user's judgment, their voice, and the mapping between systems.

> **Before writing anything, confirm the session is inside a Cowork project.** If it is not,
> tell the user plainly: "I need a project to keep your settings in — create one from
> **Projects → +** in the sidebar, then run `/clients:setup` again." Do not write to a scratch
> directory; the files would be lost.

## Step 1 — Check connections

Check which of `~~email`, `~~tracker`, `~~notetaker`, `~~calendar`, `~~chat`, `~~product`
respond.

Report the result as a simple checklist. For anything missing, give the exact click path
rather than an explanation:

- Gmail / Google Calendar → **Customize → Connectors**, find it, **Connect**, sign in.
- monday.com / notetaker → these were offered during install. If skipped, they are under
  **Customize → Plugins → Client Operations → Connectors**.

If `~~product` is missing and `.mcp.json` has an empty url for it, it has to be added as a
custom connector: **Customize → Connectors → Add custom connector**, then paste the URL and
sign in. Ask the user for the URL if it is not in `preferences.md`, and record it there once
it works so nobody has to find it again.

It is optional but high value — without it, product questions cannot be answered from source
and will be handed back rather than drafted. Say so plainly rather than quietly degrading.

`~~email` and `~~tracker` are required — stop and wait if either is missing. A notetaker is
strongly recommended but optional; note what will be missing without it (commitments made
verbally on calls). Calendar and chat are optional.

## Step 2 — Learn the communication style

This is the step that determines whether the user trusts the assistant, so do it properly.

Read roughly **60–100 of the user's own sent messages** from `~~email`, favouring replies to
external client addresses over internal ones, and spread across several months rather than
one busy week.

Then follow [style](../style/SKILL.md) to build the profile and
write `style-profile.md`.

Present the profile back **as prose the user can react to**, not as a table of scores. Give
them 3–4 real quotes from their own sent mail as evidence, and ask directly: *"Is this how
you would describe yourself? What did I get wrong?"* Apply their corrections before saving.

People often disagree with an accurate profile because it describes a habit they dislike in
themselves. If they push back on something the evidence clearly supports, say so once and
ask which they want: how they *do* write, or how they *want* to write. Record the answer —
it is a legitimate instruction either way.

## Step 3 — Discover clients

Do not ask "who are your clients?" Find them, then confirm.

1. List boards and workspaces in `~~tracker`. Identify which represent clients or
   onboardings rather than internal work.
2. Pull the most frequent external email domains from the last ~90 days of `~~email`.
3. List recent meetings from `~~notetaker` and their attendee domains.

Match these into candidate clients. Present the merged list and ask the user to confirm,
remove, and add. Expect the systems to disagree — a client in email with no board, or a
board with no recent mail, is a normal and useful finding. Show those cases explicitly.

For each confirmed client, write `clients/<slug>.md`:

```markdown
# <Client name>

**Tracker:** <board name and URL, or "none">
**Email domains:** <domain(s)>
**Meeting attendees:** <names seen on calls>
**Stage:** <onboarding | live | at risk | winding down>
**Address form:** <see style-profile.md — per-client, confirm before first draft>

## Who's who
<name — role — what they care about>

## What we agreed
<roadmap, milestones, dates — link to the tracker item rather than copying it>

## Open threads
<what we owe them / what they owe us — with dates>

## Notes for me
<judgment calls, sensitivities, history that no system records>
```

Leave sections empty rather than inventing content. `/clients:queue` and `/client` will fill them in
over time.

## Step 4 — Set preferences

Ask a small number of questions with sensible defaults offered, so the user can accept
rather than compose:

1. **When do you want the queue?** (default: 8:30, 13:00, 16:30 on weekdays)
2. **Which clients are sensitive right now?** — anything commercially or personally
   delicate, where the assistant should hand back rather than draft.
3. **Anything you always want to write yourself?** (defaults offered: pricing, contract or
   scope changes, complaints, anything involving a named individual's performance)
4. **How much drafting?** — full drafts ready to send, or short bullet answers to expand?
5. **Which Slack channels are about clients?** If `~~chat` is connected, list the channels
   the user is in, mark which look client-related, and let them confirm. Flag any shared
   with a client (Slack Connect) separately — those carry actual client communication, not
   just internal chatter, and are triaged like email.

   Name the channels explicitly in `preferences.md`. **Do not offer to include DMs**, and if
   the user asks for them, explain why not: their DMs hold performance, pay, and
   interpersonal conversations that this assistant should never process. Chat is read-only
   in all cases — nothing is ever posted.

Write the answers to `preferences.md`. Keep the defaults visible in the file so the user can
see what they accepted without re-running setup.

## Step 5 — Prove it works

Do not end setup with a configuration summary. End it with output the user can judge.

Run `/clients:queue` on real current data and show the result. Then pick **two recent client emails
the user already answered themselves**, draft replies without looking at their actual
response, and show both side by side.

If `~~product` is connected, make one of the two a **product question** — the kind the user
answers over and over. That is the single biggest category of their mail, and the clearest
demonstration of whether this saves them real time.

This is the moment the user decides whether to keep using the assistant. Ask what is off
about the drafts, and write the answer into `style-profile.md` under **Corrections**. Two or
three of these are worth more than the whole automated profile.

## Step 6 — Set up the daily rhythm

Explain in one short paragraph how the assistant is meant to be used: it is a queue reviewed
two or three times a day, not a notification stream, and nothing is sent without approval.

Then set up the schedule — but **verify before committing to it**.

Scheduled tasks run in the cloud rather than on the user's machine, which is what makes the
queue ready even when the laptop is closed. It also means a scheduled run may not be able to
read the project folder this setup just wrote to. If that is the case, the scheduled queue
would run without the style profile and client list and quietly produce generic output — the
worst possible failure, because it still looks like it worked.

So:

1. Create **one** scheduled `/clients:queue` run, a few minutes out.
2. When it completes, check its output actually used the setup: are the clients from
   `clients/` named? Does the drafting reflect `style-profile.md`?
3. **If yes** — create the recurring tasks at the times from Step 4 and tell the user they
   are set.
4. **If no** — do not schedule the rest. Tell the user plainly that scheduled runs cannot see
   their settings yet, that `/clients:queue` works normally when they run it themselves, and that
   this needs fixing before automating. Then report which files were unreachable.

Finally, tell the user how to change their mind later: *"Ask me to redo any part — `/clients:setup
style`, `/clients:setup clients`, or `/clients:setup preferences`."*
