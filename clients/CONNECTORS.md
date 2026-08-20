# Connectors

## How tool references work

Skill files use `~~category` as a placeholder for whatever tool is actually connected in
that category. For example, `~~notetaker` means tl;dv here, but could be Granola, Fireflies,
or Fathom without any change to the skills.

This plugin is **tool-agnostic**. Workflows are described in terms of categories, not
products, so swapping a tool later does not require rewriting the skills.

## Connectors for this plugin

| Category | Placeholder | Pre-configured | Other options |
|---|---|---|---|
| Email | `~~email` | Gmail (enabled in Cowork settings) | Microsoft 365 |
| Project tracker | `~~tracker` | monday.com | Asana, Linear, Notion |
| Meeting notes | `~~notetaker` | tl;dv | Granola, Fireflies, Fathom, Otter |
| Calendar | `~~calendar` | Google Calendar (enabled in Cowork settings) | Microsoft 365 |
| Team chat | `~~chat` | Slack | Microsoft Teams |
| Product knowledge | `~~product` | Butterstaff | any docs/codebase Q&A source |
| Customer data | `~~customer data` | Butterstaff | data warehouse, admin API |

## Two kinds of connector

**Bundled (hosted MCP).** monday.com, tl;dv, Slack, and Butterstaff ship in `.mcp.json` with
their server URLs. Installing the plugin offers a sign-in prompt for each — one OAuth click, no
configuration.

Slack is the one exception to "no configuration": it does not support dynamic client
registration, so its `clientId` is pinned in `.mcp.json`. That is the same public client id
Anthropic pins across their own plugins. Leave it alone — without it, sign-in cannot complete.

**First-party.** Gmail and Google Calendar are Claude's own connectors. They appear in
`.mcp.json` with an empty URL because they cannot be bundled with a URL; they are switched
on in Cowork under **Customize → Connectors**. `/setup` walks through this.

## Swapping the notetaker

Skills never name a notetaker; they reference `~~notetaker`. To move to a different one,
change the entry in `.mcp.json` and the table above. No skill file changes.

## A note on tl;dv

Two ways to connect it, and the difference matters:

- **`https://mcp.tldv.io/mcp`, bundled here.** Hosted, OAuth with dynamic client
  registration, so install offers a sign-in button. This is the intended path.
- **The connector directory.** tl;dv is also listed in Anthropic's connector directory, so
  it can be enabled under **Customize → Connectors** like Gmail. Use this if the bundled
  sign-in fails. Connect it one way or the other, not both.

**Do not use `tldv-public/tldv-mcp-server` from GitHub.** It is a self-hosted stdio server
needing Docker or Node, a `TLDV_API_KEY`, and a tl;dv Business/Enterprise plan — three
things this plugin exists to avoid.

## A note on Butterstaff

Butterstaff answers two different kinds of question, so it fills two placeholders:

- **`~~product`** — how the product actually works, answered from the codebase. This is the
  one that matters most. The single largest category of client email is the same product
  questions asked repeatedly, and this turns answering them from *the model explains
  plausibly* into *the answer is retrieved from the source of truth*.
- **`~~customer data`** — account and usage data retrieved from the database. This is the
  only connector that knows whether a client is actually **using** the product, as opposed to
  what a project board claims about them.

It is not in Anthropic's connector directory, but it is bundled here by URL
(`https://butterstaff.com/mcp`), so installing the plugin offers a sign-in prompt like any
other connector. It supports dynamic client registration, so no client id is pinned.

### Two rules when using it

**Never put one client's data in another client's message.** `~~customer data` can retrieve
across the whole customer base. A draft to one client must contain only facts about that
client. Do not include comparisons, benchmarks, or "other customers typically…" figures
derived from live data — treat any cross-client number as disclosure until the user says
otherwise.

**Retrieved is not the same as verified.** Attribute product claims in a draft to what
`~~product` returned, and if the answer is ambiguous or looks out of date, treat the item as
**Need context** rather than drafting a confident explanation. Being wrong about how the
product works, in writing, to a client, is expensive to walk back.

## Important connector behavior

**Gmail cannot send.** Claude's Gmail connector creates drafts only. This is deliberate and
this plugin depends on it: every prepared reply lands in Gmail Drafts, and sending is always
a human action. There is no configuration that changes this, and none is wanted.

**monday.com writes are real.** The tracker connector can modify boards. Every skill here
requires explicit approval before any write. See `queue/SKILL.md`.

**Slack can post, and this plugin never does.** Unlike Gmail, the Slack connector holds
`chat:write` — it is technically able to send messages into channels and DMs. Nothing here
may use it. Slack is a **read-only context source**.

The reason is that Gmail's safety comes from the platform: it physically cannot send, so a
bad draft is harmless. Slack has no such protection. A posted message is instant, visible to
everyone in the channel, and cannot be unsent. So the restriction has to be a rule instead,
and rules only hold if they are absolute — "post only when it seems safe" would not survive
contact with a plausible-looking edge case.

If something needs saying in Slack, put the suggested text in the queue and let the user
paste it.

**Do not read DMs.** Only channels the user named during setup. A Head of Customer Success's
DMs carry performance conversations, compensation, and complaints about colleagues — exactly
the material this assistant is supposed to hand back rather than process. It should not be
swept at all.

**Calendar is read-mostly.** Skills may read availability and propose times, but must not
create, move, or cancel events without confirmation.
