# Connectors

## How tool references work

Skill files use `~~category` as a placeholder for whatever tool is actually connected in
that category. For example, `~~notetaker` might mean Granola, Fireflies, or Fathom.

This plugin is **tool-agnostic**. Workflows are described in terms of categories, not
products, so swapping a tool later does not require rewriting the skills.

## Connectors for this plugin

| Category | Placeholder | Pre-configured | Other options |
|---|---|---|---|
| Email | `~~email` | Gmail (enabled in Cowork settings) | Microsoft 365 |
| Project tracker | `~~tracker` | monday.com | Asana, Linear, Notion |
| Meeting notes | `~~notetaker` | Granola, Fireflies, Fathom | Otter, Zoom AI Companion |
| Calendar | `~~calendar` | Google Calendar (enabled in Cowork settings) | Microsoft 365 |

## Two kinds of connector

**Bundled (hosted MCP).** monday.com and the notetakers ship in `.mcp.json` with their
server URLs. Installing the plugin offers a sign-in prompt for each — one OAuth click, no
configuration.

**First-party.** Gmail and Google Calendar are Claude's own connectors. They appear in
`.mcp.json` with an empty URL because they cannot be bundled with a URL; they are switched
on in Cowork under **Customize → Connectors**. `/setup` walks through this.

Only connect the notetaker that is actually used. Leaving the other two unconnected is
fine — skills check what is available before relying on it.

## Important connector behavior

**Gmail cannot send.** Claude's Gmail connector creates drafts only. This is deliberate and
this plugin depends on it: every prepared reply lands in Gmail Drafts, and sending is always
a human action. There is no configuration that changes this, and none is wanted.

**monday.com writes are real.** The tracker connector can modify boards. Every skill here
requires explicit approval before any write. See `queue/SKILL.md`.

**Calendar is read-mostly.** Skills may read availability and propose times, but must not
create, move, or cancel events without confirmation.
