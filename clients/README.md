# personeo Client Operations — Cowork plugin

A customer-success co-pilot for Claude Cowork. It reviews client email, meetings, and
project state a few times a day and produces **one queue**: what needs a reply, what is
slipping, what to update, and what needs your personal judgment.

It prepares. You approve. Nothing is sent or changed without you.

---

## Install (for the end user)

You need a paid Claude plan (Pro, Max, Team, or Enterprise) and the Claude desktop app.

**1. Add the plugin source**

In Claude, open **Customize → Plugins → Add marketplace** and paste:

```
xPapay/pkuklis-assistant
```

**2. Install**

Find **Client Operations** in the list and click **Install**. You will be offered sign-in
prompts for **monday.com**, **tl;dv**, and **Slack** — accept all three.

If your team uses **Butterstaff**, add it under **Customize → Connectors → Add custom
connector** with the URL your team uses. It is optional, but it is what lets the assistant
answer product questions from the source rather than guessing — the largest single category
of client mail.

**3. Connect Gmail and Calendar**

These are Claude's own connectors and are switched on separately, under
**Customize → Connectors**. Connect **Gmail** and **Google Calendar**.

> Gmail here can only create drafts. It cannot send email. That is deliberate — every reply
> waits in your Drafts until you press send.

**4. Create a project**

Open **Projects → +** in the sidebar and create one called *Clients*. This is where the
assistant keeps what it learns about you. Start a session inside it.

**5. Run setup**

Type:

```
/clients:setup
```

It takes about fifteen minutes. It will connect the remaining pieces, read your sent mail to
work out how you write, find your clients, and ask a handful of questions. It finishes by
showing you real output so you can judge it.

**6. Use it**

```
/clients:queue
```

Run it two or three times a day, or let setup schedule it for you. Scheduled runs happen in
the cloud, so the queue is ready even if your laptop was closed.

---

## Daily use

| Command | What it does |
|---|---|
| `/clients:queue` | The main review. What needs attention across every client. |
| `/clients:status <name>` | Full picture of one client. Good five minutes before a call. |
| `/clients:meeting` | Turn a recorded call into decisions, commitments, and deadlines. |
| `/clients:roadmap` | Check one client against the agreed plan. |
| `/clients:setup style` | Redo the writing-style profile. |
| `/clients:setup clients` | Add or remove clients. |

**The assistant gets better when you correct it.** If a draft is wrong, say what was wrong —
it is written into the style profile and applied from then on. A few corrections in the first
week are worth more than the whole automated setup.

---

## What it will not do

By design, not by limitation:

- **It cannot send email.** Drafts only.
- **It will not change monday.com** without you approving each change.
- **It will not touch your calendar.** It suggests; you decide.
- **It never posts to Slack.** Slack is read-only — it is used to notice commitments your
  team made about a client, and shared client channels. If something needs saying there, it
  gives you the wording to paste.
- **It never reads your DMs.** Only the channels you name during setup.
- **It hands back anything sensitive** — pricing, contracts, complaints, conflict, anything
  about a specific person — rather than drafting it.

---

