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
<your-github-org>/pkuklis-assistant
```

**2. Install**

Find **Client Operations** in the list and click **Install**. You will be offered sign-in
prompts for monday.com and your meeting notetaker — accept the ones you use, skip the rest.

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
/setup
```

It takes about fifteen minutes. It will connect the remaining pieces, read your sent mail to
work out how you write, find your clients, and ask a handful of questions. It finishes by
showing you real output so you can judge it.

**6. Use it**

```
/queue
```

Run it two or three times a day, or let setup schedule it for you. Scheduled runs happen in
the cloud, so the queue is ready even if your laptop was closed.

---

## Daily use

| Command | What it does |
|---|---|
| `/queue` | The main review. What needs attention across every client. |
| `/client-status <name>` | Full picture of one client. Good five minutes before a call. |
| `/meeting-followup` | Turn a recorded call into decisions, commitments, and deadlines. |
| `/setup style` | Redo the writing-style profile. |
| `/setup clients` | Add or remove clients. |

**The assistant gets better when you correct it.** If a draft is wrong, say what was wrong —
it is written into the style profile and applied from then on. A few corrections in the first
week are worth more than the whole automated setup.

---

## What it will not do

By design, not by limitation:

- **It cannot send email.** Drafts only.
- **It will not change monday.com** without you approving each change.
- **It will not touch your calendar.** It suggests; you decide.
- **It hands back anything sensitive** — pricing, contracts, complaints, conflict, anything
  about a specific person — rather than drafting it.

---

## For whoever maintains this

```
.claude-plugin/marketplace.json   makes this repo installable as a marketplace
client-ops/
  .claude-plugin/plugin.json      manifest
  .mcp.json                       bundled connectors (hosted MCP + first-party placeholders)
  CONNECTORS.md                   the ~~category placeholder convention
  skills/                         one directory per skill, each with SKILL.md
```

Everything is markdown and JSON — no build step, no server, no custom MCP. Edit a `SKILL.md`,
push, and users get it on the next marketplace **Update**.

Skills are invoked as slash commands (`/queue`) and are also picked up automatically when
relevant. `queue` is the product; the others support it.

**Per-user state never lives in this repo.** The style profile, client files, and preferences
are written into the user's own Cowork project folder by `/setup`. That separation is what
makes the plugin distributable to more than one person.

### Known things to verify

- **Confirm the exact slash-command strings on the first real install.** Anthropic's own
  plugins document namespaced commands (`/finance:reconciliation`), so these may surface as
  `/client-ops:queue` rather than `/queue`. If so, correct the user-facing instructions above
  before handing over — the first command a non-technical user types must work.
- Scheduled tasks run in the cloud against connectors and files saved to your Claude account.
  Confirm a scheduled `/queue` can reach the local project folder; if it cannot, move
  `style-profile.md` and `clients/` to a location the cloud run can read.
- Cowork plugins were still in beta at the time of writing and install locally per user. On
  Team/Enterprise an admin can push the plugin to everyone instead of the paste-a-URL flow.
