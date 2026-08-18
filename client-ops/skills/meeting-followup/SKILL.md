---
name: meeting-followup
description: Turn a recorded client conversation into decisions, commitments, owners, deadlines, and blockers. Use after a client call, when the queue needs to know what was agreed verbally, or when the user asks what came out of a meeting.
argument-hint: "<client or meeting name> — omit to use the most recent client meeting"
---

# /meeting-followup

> If you see unfamiliar placeholders like `~~notetaker`, see [CONNECTORS.md](../../CONNECTORS.md).

Extract what a meeting *changed*, not what was said in it.

A summary is not the goal. `~~notetaker` already produces summaries and they are of limited
use here, because the expensive thing is not forgetting the topic — it is forgetting that the
user promised something on minute 34 of a call three weeks ago.

## Step 1 — Get the conversation

Retrieve the transcript from `~~notetaker`. If several meetings match, list them and ask
which. Prefer the full transcript over the tool's own summary — the summary has already
discarded exactly the asides where commitments tend to be made.

Identify each speaker and which side they are on. "We'll sort that out" means different
things depending on who said it.

## Step 2 — Extract

Pull out only things that create or change an obligation:

**Decisions** — something settled that was previously open. Record what was decided, by whom,
and what it replaces. A decision that reverses an earlier one is the single most important
thing to catch, because every downstream system still reflects the old one.

**Commitments** — anyone saying they will do something. For each:
- who committed — **a named person, not "the team"**
- what exactly
- by when

Capture **our** commitments as carefully as theirs. Ours are the ones that damage trust when
missed, and the ones nobody writes down.

**Deadlines** — dates agreed, changed, or quietly allowed to move.

**Blockers** — anything stated as preventing progress, plus who can unblock it.

**Scope changes** — anything expanding or narrowing what was agreed. Flag these prominently
even when they sound casual. "While you're in there, could you also…" is a scope change, and
it is almost never recorded as one.

**Next actions** — what happens before the next contact.

### Handling vagueness

Meetings are vague and transcripts are imperfect. Preserve the difference between what was
agreed and what was merely mentioned:

- Firm: *"Jana will send the SSO metadata by Friday."*
- Soft: *"Jana mentioned she'd probably look at SSO this week — not confirmed."*

Never harden a soft statement into a commitment. A phantom obligation in the queue sends the
user chasing something the client never actually agreed to, and that is worse than missing it.

If something important was left genuinely unresolved, that is a finding worth reporting —
often the most useful one in the meeting.

Where the transcript is unclear or the audio evidently failed, say so rather than
interpolating.

## Step 3 — Reconcile

Compare what was extracted against `~~tracker` and the client file. Report:

- commitments with **no corresponding task**
- dates that **changed** on the call but not on the board
- decisions that **contradict** what the board or roadmap still says
- blockers that are **new**

This reconciliation is the actual output. It is what turns a call into operational reality.

## Output

```markdown
## <Client> — <meeting> — <date>

**Decisions**
- <what was decided> — <who> — <supersedes: ...>

**Commitments — us**
- <person> — <what> — by <date>   [tracker: <item> | NOT TRACKED]

**Commitments — client**
- <person> — <what> — by <date>

**Blockers**
- <what> — held by <who>

**Scope changes**
- <what changed, and against what baseline>

**Left unresolved**
- <what still needs deciding, and who by>

**Suggested tracker updates**
- <item>: <current> → <proposed>
```

Then append the durable facts — decisions, commitments, dates — to `clients/<slug>.md`. Do
not paste the transcript or the full summary into the client file; link to the meeting in
`~~notetaker` instead. The client file is working context and stays readable only if it holds
conclusions rather than material.

Propose tracker updates. Do not apply them without approval.
