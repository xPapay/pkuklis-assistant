---
name: queue
description: The main review. Sweeps client email, meetings, and tracker state, then produces one prioritized queue — what needs a reply, what is slipping, what to update, and what needs the user's personal judgment. Use two or three times a day, when the user asks what needs attention, or when a scheduled task fires.
argument-hint: "(no arguments) or a client name to limit the sweep"
---

# /clients:queue

> If you see unfamiliar placeholders like `~~email`, see [CONNECTORS.md](../../CONNECTORS.md).

The core loop of this assistant. Everything else exists to make this output good.

## What this is for

The user is a Head of Customer Success carrying several clients at once. The cost is not
writing emails — it is **remembering**: what was agreed, what is late, who owes whom, which
client has gone quiet.

So the goal is not to answer as many emails as possible. It is:

> **The user opens this two or three times a day, understands what needs attention across
> every client, and approves the right next actions in a few minutes.**

A queue with three items and correct reasoning beats a queue with twelve drafts. If a draft
is not clearly right, do not include it — describe the situation and hand it back instead.
Volume is not the metric; not having to double-check is.

## Before you start

Read `style-profile.md`, `preferences.md`, and the `clients/` files from the project folder.
If they are missing, say so and suggest `/clients:setup` — do not guess at the user's voice.

Note the current date and time. "Overdue" and "gone quiet" are meaningless without it.

## Step 1 — Sweep

Cover **every** client in `clients/`, not only ones with new mail. Silence is a signal and
this is the step that catches it.

For each client, gather in parallel where possible:

- **`~~email`** — messages since the last run, in both directions. Include threads where the
  user sent the last message and no reply has come back.
- **`~~tracker`** — item status, due dates, assignees, and recent updates on the client's
  board.
- **`~~notetaker`** — meetings since the last run. Extract commitments via
  [meeting](../meeting/SKILL.md).
- **`~~calendar`** — what is scheduled with this client, and when they were last met.
- **`~~chat`** — only the channels named in `preferences.md`. Two things are being looked
  for, and nothing else:
  1. **Commitments the team made about this client** — "I'll send Acme the SSO docs
     tomorrow". These never reach the tracker or email, nobody writes them down, and they
     are the ones that quietly damage trust when missed.
  2. **Blockers surfaced internally** before the client was told.

  If a channel is **shared with the client** (Slack Connect), treat it as client
  communication and triage it like email in Step 2.

Then apply [roadmap](../roadmap/SKILL.md) to compare what was agreed against
what has actually happened.

If a connector fails or returns nothing, say so in the output. A silent gap reads as "all
clear" and that is the one failure mode that actively misleads.

## Step 2 — Classify each item

Every incoming message and every detected deviation resolves to exactly one of four:

| Outcome | When | What you do |
|---|---|---|
| **Draft** | Routine. You have the facts. The answer is not contentious. | Write a Gmail draft |
| **Need context** | The right answer depends on something you cannot see | Ask the user the specific question — never a vague "please review" |
| **Personal** | Judgment, relationship, money, or conflict | Summarize and hand back. Do not draft |
| **No action** | FYI, thanks, newsletters, already handled | Leave it out of the queue entirely |

**Route to Personal, not Draft, when the message involves:**

- pricing, discounts, contract terms, or scope changes
- a complaint, a frustration, or an apology that needs to mean something
- anything about a specific person's performance or behavior
- churn signals, escalation, or a threat to leave
- a decision the user has not already made somewhere you can see
- anything where being wrong costs more than being slow

Also check `preferences.md` — the user's own list of "always mine" topics wins over the
table above.

**When genuinely torn between Draft and Personal, choose Personal.** A hand-back costs the
user thirty seconds. A confident draft on a delicate thread costs trust that took months to
build, and this assistant's entire value rests on the user not having to double-check it.

## Step 3 — Prepare drafts

For each **Draft** item, follow [style](../style/SKILL.md) and
create the reply in `~~email` as a Gmail draft.

The connector cannot send. That is the safety model — nothing leaves without the user
pressing send in Gmail, so there is no approval step to build, and drafting freely is safe.

**This does not extend to `~~chat`.** Slack can post, so the same freedom would be unsafe
there. Never post to Slack, never reply in a thread, never DM — including in channels shared
with a client. When something needs saying in Slack, put the suggested wording in the queue
under **Needs you** and let the user paste it.

Do not commit the user to anything they have not already agreed to: no new dates, no new
scope, no "we'll have that to you by Thursday" unless Thursday is already written down in
`~~tracker` or a meeting. If a reply needs a commitment the user has not made, that item is
**Need context**, not **Draft**.

## Step 4 — Propose tracker updates

Where reality has moved past `~~tracker` — a milestone completed on a call, a date slipped,
a blocker cleared — propose the update. State it as a diff:

> *Acme — "Data migration" is In progress, due 12 Aug (6 days ago). On the 14 Aug call Jana
> said it finished Tuesday. → Set Done, completed 12 Aug.*

**Never write to `~~tracker` inside `/clients:queue`.** List the proposals and let the user pick.
Apply only what they approve, then confirm what changed.

## Step 5 — Output

Use this order. It is deliberate: the user's own judgment first, then things with a deadline,
then things that are merely useful.

```markdown
## Queue — <date, time>
<one sentence: how many clients swept, how many need you, anything unusual>

### Needs you (N)
**<Client> — <one-line situation>**
<why this is yours, in 1–2 sentences. What you need to decide.>
<link to the thread>

### Slipping (N)
**<Client> — <what was expected, and when>**
Expected: <X by <date>, from <where it was agreed>>
Actual: <Y>
Recommend: <specific next action>

### Drafts ready (N)
**<Client> — <subject>**
<one line on what they asked and what the draft says>
→ In Gmail Drafts

### Suggested tracker updates (N)
**<Client> — <item>**: <current> → <proposed>  ·  <evidence>

### Untracked commitments (N)
**<Client> — <who> promised <what>, <when>**  ·  said in <where>
<not in the tracker — create a task, or drop it?>

### Quiet (N)
<Client> — <last contact, next milestone, nothing needed>
```

Rules for the output:

- **Never omit the Quiet section.** It is how the user knows a client was checked rather than
  missed. Without it, an empty queue is indistinguishable from a broken one.
- Lead every item with the client name. The user thinks in clients, not in threads.
- Cite where each fact came from — a thread, a call, a tracker item. The user must be able to
  approve a recommendation without going and reconstructing the story themselves. If you
  cannot cite it, you are guessing, and you should say so.
- Keep it scannable. Reasoning belongs under the item, not in front of it.
- If nothing needs attention, say exactly that and show only Quiet. Do not manufacture work.

## Step 6 — Close the loop

Offer, but do not perform:

- apply approved tracker updates
- propose calendar actions — follow-ups, check-ins, a call for a client gone quiet.
  **Recommend times; never create, move, or cancel an event without confirmation.**

Then record what changed. Append to each client's `clients/<slug>.md` only durable facts —
commitments made, decisions taken, dates agreed. Do not append the queue itself; it is a
view, not a record, and duplicating it turns the client files into an unreadable log.

Note the run time so the next sweep knows where to resume.
