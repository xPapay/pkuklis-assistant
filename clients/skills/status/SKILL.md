---
name: status
description: Assemble the full current picture for one client — what is happening, what was agreed, what each side is waiting for, and what should happen next. Use before a call, when the user asks about a specific client, or when a queue item needs deeper context.
argument-hint: "<client name>"
---

# /clients:status

> If you see unfamiliar placeholders like `~~tracker`, see [CONNECTORS.md](../../CONNECTORS.md).

Answer, for one client, the questions the user would otherwise reconstruct by hand:

- What is currently happening?
- What did we agree?
- What are we waiting for from them?
- What are they waiting for from us?
- What is the next milestone, and are we on track for it?
- Is anything slipping?
- Should we contact them, and what should we say?
- Does anything need updating in `~~tracker`?

Typical use is five minutes before a call. Write for that reader: someone who needs to walk
in informed, not someone doing a review.

## Gather

Read the client file first — it holds context no system records. Then, in parallel:

- **`~~email`** — the last ~30 days of threads both ways, plus any older thread still open.
- **`~~tracker`** — board state, milestones, dates, owners, recent activity.
- **`~~notetaker`** — recent meetings. Run [meeting](../meeting/SKILL.md)
  on any not yet processed.
- **`~~calendar`** — what is scheduled, and when they were last met.

Then run [roadmap](../roadmap/SKILL.md).

## Output

```markdown
# <Client> — <date>

**Stage:** <onboarding / live / at risk>  ·  **Last contact:** <when, how>
**Next milestone:** <what> — <date> — <on track / at risk / late>

## Where things stand
<3–5 sentences. What is actually going on, in plain language. If someone asked the user
"how's Acme doing?" in a corridor, this is the answer.>

## Agreed plan
<milestones and dates, with where each was agreed>

## Waiting on them
<what — asked when — how long — does it block anything>

## Waiting on us
<what — promised when — by whom — is it tracked>

## Slipping
<from /clients:roadmap, or "nothing">

## People
<name — role — what they care about — address form>

## Recommended next actions
1. <specific action, and why now>

## Tracker inconsistencies
<where the board disagrees with reality>
```

## Rules

**Cite everything.** Every fact gets a source — a thread, a call and its date, a tracker
item. The user must be able to trust this without re-reading the history themselves, and a
single uncited claim that turns out wrong costs more trust than ten cited ones earn.

**Separate what you know from what you infer.** "No reply for 9 days" is a fact. "They may be
deprioritizing this" is an inference, and should be visibly marked as one.

**Say when you do not know.** A gap stated plainly is useful. A gap filled with a plausible
guess is a liability, and it is invisible to the reader precisely because it reads well.

**Do not write to `~~tracker`.** Propose; let the user approve.
