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

## Two kinds of connector

**Bundled (hosted MCP).** monday.com and tl;dv ship in `.mcp.json` with their server URLs.
Installing the plugin offers a sign-in prompt for each — one OAuth click, no configuration.

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

## Important connector behavior

**Gmail cannot send.** Claude's Gmail connector creates drafts only. This is deliberate and
this plugin depends on it: every prepared reply lands in Gmail Drafts, and sending is always
a human action. There is no configuration that changes this, and none is wanted.

**monday.com writes are real.** The tracker connector can modify boards. Every skill here
requires explicit approval before any write. See `queue/SKILL.md`.

**Calendar is read-mostly.** Skills may read availability and propose times, but must not
create, move, or cancel events without confirmation.
