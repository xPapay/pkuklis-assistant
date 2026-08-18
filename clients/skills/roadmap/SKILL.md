---
name: roadmap
description: Compare what was agreed with a client against what has actually happened, and identify what is slipping. Use inside the queue, when reviewing a single client, or when the user asks whether an onboarding is on track.
argument-hint: "<client name> or omit when called from /clients:queue"
---

# /clients:roadmap

> If you see unfamiliar placeholders like `~~tracker`, see [CONNECTORS.md](../../CONNECTORS.md).

Detect deviation between the agreed plan and reality.

The output to aim for is:

> **"We expected X by now, Y is still missing, so I recommend Z."**

Not "task overdue". The user can already see overdue tasks in the tracker. What they cannot
see cheaply is the *consequence* — what it blocks, who is holding it, and what to do.

## Step 1 — Establish what was agreed

Reconstruct the plan from the systems that hold it, in this order of authority:

1. **`~~tracker`** — the board, milestones, due dates, owners. The nominal plan.
2. **Meetings** — what was actually said and agreed on calls, which is frequently newer than
   the board and often contradicts it.
3. **Email** — dates and commitments made in writing between calls.
4. **`~~chat`** — commitments the team made internally that never became a task. Usually the
   only place these exist.
5. **The client file** — context that no system records.

Where these disagree, **the most recent explicit agreement wins**, whatever system it lives
in. A date agreed on Tuesday's call supersedes a board field last edited in June. Say
explicitly when you are relying on something the tracker does not know — that gap is itself
worth reporting, and usually means the board needs updating.

## Step 2 — Find the deviations

Look for each of these:

| Pattern | What it looks like |
|---|---|
| **Milestone overdue** | Past its date, not done |
| **Waiting on client** | We asked; no answer. How long? |
| **Waiting on us** | They asked, or we promised. Is anyone actually doing it? |
| **Commitment unfulfilled** | Said on a call, never happened, no task exists |
| **Silence** | No contact for materially longer than normal *for this client* |
| **Sequence broken** | Step 4 is moving while step 2 never finished |
| **Next step undefined** | Current phase ending, nothing planned after it |
| **Stale tracker** | Board says one thing, email and calls say another |

Two that are easy to miss and matter most:

**Silence.** Compare against this client's own rhythm, not a fixed threshold. Ten days of
quiet is normal for one client and an early churn signal for another. A client who replied
within the hour for two months and has now gone quiet for five days is a stronger signal than
one who always takes a fortnight.

**Our own unfulfilled commitments.** These are the expensive ones. They damage trust, the
client often will not chase them, and they are invisible in the tracker precisely because
nobody created the task. Meeting transcripts and `~~chat` are the main sources — see
[meeting](../meeting/SKILL.md). A promise made to a colleague in Slack is still a promise to
the client, and it is the least likely of all to be tracked anywhere.

## Step 3 — Assess and recommend

For each deviation, establish:

- **How late, in days.** Always give the number.
- **Who is holding it** — us, them, or a third party.
- **What it blocks.** A late item on the critical path is a different problem from a late
  item nothing depends on, and the recommendation differs accordingly.
- **Whether it has been raised.** Chasing something the client already apologized for
  yesterday is worse than saying nothing.

Then recommend one specific action. "Follow up with Acme" is not a recommendation. "Ask Jana
directly for the SSO metadata — third time asked, blocking the 25 Aug pilot" is.

## Output

```markdown
**<Client> — <what was expected>**
Expected: <what> by <date> — agreed <where: board / call of <date> / email of <date>>
Actual: <what has and hasn't happened>
Impact: <what this blocks, and by when>
Recommend: <one specific action, with who to contact about what>
```

Report only genuine deviations. If a client is on track, say so in one line and move on —
the value of this skill collapses the moment the user starts skimming past it. Never pad the
list to look thorough.

Flag **inconsistency between systems** as its own finding, even when nothing is late. A board
that disagrees with reality will quietly produce wrong recommendations later.
