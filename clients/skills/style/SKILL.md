---
name: style
description: Learn and apply the user's own writing voice. Use when building the style profile during setup, when drafting any message on the user's behalf, or when the user corrects a draft and that correction should be remembered.
argument-hint: "'learn' to rebuild the profile, or omit when drafting"
---

# /clients:style

> If you see unfamiliar placeholders like `~~email`, see [CONNECTORS.md](../../CONNECTORS.md).

Two jobs: build a description of how the user writes, and apply it when drafting.

## Learning the style

Read 60–100 of the user's **own sent** messages. Replies to clients, not internal threads,
not forwards, not one-line acknowledgements. Spread across months, so you capture how they
write when calm and when under pressure.

Describe the following, each with a real quoted example as evidence:

**Shape**
- Length. Do they answer in two lines or fifteen?
- Do they open with pleasantries or go straight to the point?
- Paragraphs, bullets, or one dense block?
- How do they sign off? Is it consistent?

**Register**
- Formal or familiar, and does it shift by client?
- Warm or businesslike?
- Do they use humour? Where, and how dry?

**Substance**
- How do they explain a problem — lead with the conclusion, or build up to it?
- How do they ask for something? Direct request, or softened?
- How do they follow up on something late without straining the relationship?
- How do they deliver bad news or a delay?
- **How do they avoid over-committing?** Note the specific hedges they use. This is the
  habit most worth copying precisely, because getting it wrong creates real obligations.

**Tells**
- Recurring phrases that are genuinely theirs
- Words they never use
- Punctuation habits — dashes, ellipses, exclamation marks

### Language and register

Communication here is likely **Slovak and English mixed**, sometimes within one thread.
Profile each language separately. A person's English voice and Slovak voice are rarely the
same, and a profile that averages them produces drafts that sound like neither.

**Slovak requires an explicit decision that English does not: `ty` or `vy`.**

There is no neutral option and no way to avoid choosing. Getting it wrong is not a matter of
tone — it reads instantly as someone else writing, or as a small insult. It cannot be
inferred from the client's industry or seniority; it depends on the specific relationship and
often on a moment when one of them offered to switch.

So:

- Record the address form **per person**, not per client and not globally, in the client
  file. Two people at the same company can differ.
- Determine it from what the user has actually sent that person before. If there is no prior
  Slovak thread with that person, **ask** — this is a legitimate question for `/clients:setup` or for
  the queue, and much cheaper than getting it wrong once.
- Watch for a switch mid-history. If the user moved from `vy` to `ty` with someone, the
  later form wins.
- Carry it into every agreeing word — verbs, possessives, imperatives. A draft that mixes
  forms within one message is worse than one that picks the wrong form consistently.

Also note whether the user answers in the language they were written to in, or has a default.

### Writing the profile

Save to `style-profile.md` in the project folder. Write it as **instructions to a writer**,
not as an analysis of a person:

> *Opens with one line of context, never "I hope you're well". Answers the question in the
> first paragraph, then adds detail only if it changes what the reader should do. Uses "let
> me check and come back to you" rather than giving a date they haven't confirmed.*

That is usable. "Tone: professional, warm, concise" is not — it describes every business
writer alive and will produce generic drafts.

Include a **Corrections** section at the end, and keep appending to it. When the user edits a
draft, the difference between what was written and what they sent is the single highest-value
signal available. Record the pattern, not the sentence:

> *Cut my hedging in the opening — went straight to the answer. Third time. Stop softening
> openings.*

## Applying the style

When drafting:

1. Read `style-profile.md` and the client's file, including the address form for the
   recipient.
2. Read the thread properly — several messages back, not just the last one. Match the
   register that is already established in it.
3. Draft. Then check it against the profile and revise once.

**Do not copy phrasing from past emails.** The goal is a message the user would have written,
not a collage of ones they did. Reusing distinctive sentences produces drafts that feel
uncanny to the recipient — a phrase they have seen before in a different context — and they
stop being usable the moment the situation differs slightly from the one they came from.

Match structure, length, directness, and hedging. Let the words be new.

**One deliberate exception: explaining how the product works.** The user has said an AI-written
explanation was clearer than his own. Take that at face value — for product explanations,
optimize for the reader understanding it, not for sounding exactly like him. Keep his
register, greeting, and sign-off so the message is recognisably his; let the explanation
itself be as clear as it can be.

This applies only to explanation. Everywhere else — chasing, apologising, negotiating,
declining — match his voice as closely as the profile allows.

**Never invent facts to fill a gap.** No dates that were not agreed, no numbers, no names, no
"as discussed" for something you cannot find. If the draft needs a fact you do not have, that
is not a drafting problem — the item belongs in **Need context** in the queue.

Finally: read it once as the client. If it would prompt a confused reply or a follow-up
question, it is not finished.
