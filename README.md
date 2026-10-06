# Kalent sourcing agents

**English** · [Français](README.fr.md)

Six AI agents that run your sourcing end to end, from job description to follow-ups, inside Claude, ChatGPT or Grok. They're powered by the Kalent MCP (`https://app.kalent.ai/api/mcp`). You approve, they execute: nothing goes out without your OK.

Full install guide: https://kalent.ai/guide-installation-agents

| Agent | Skill | What it does |
|---|---|---|
| Brief | `kalent-brief` | Reads the job description, turns it into hard criteria, asks for what's missing (quota, account size...) |
| Sourcing | `kalent-sourcing` | Searches the entire market, including the profiles LinkedIn never shows you |
| Qualification | `kalent-qualification` | Checks every criterion on every profile and explains why each one made the cut |
| Enrichment | `kalent-enrichissement` | Finds personal mobile and email |
| Outreach | `kalent-outreach` | Writes a personal first message based on each candidate's track record |
| Follow-up | `kalent-relance` | Follows up on WhatsApp or email when LinkedIn goes quiet |
| Full flow | `kalent-flow` | Chains all six agents, with your approval at every step |

The agents reply in the recruiter's language (English, French, or whatever you write in).

## Requirements

A Kalent account (sign up at app.kalent.ai). Qualified search, saved searches and sequences require an eligible Kalent plan. For LinkedIn sequences, your LinkedIn account must be synced in Kalent (Recruiter or Sales Navigator for InMail).

## Claude Code

```bash
claude mcp add --transport http kalent https://app.kalent.ai/api/mcp
```

Then, inside Claude Code:

```
/plugin marketplace add edouard-kalent/sourcing-agents
/plugin install kalent-sourcing@kalent
```

## Claude (web and desktop)

1. Settings > Connectors > Add custom connector > URL `https://app.kalent.ai/api/mcp`, then sign in to Kalent.
2. Settings > Capabilities > Skills > Upload: one .zip per folder in `skills/` (start with `kalent-flow`). Ready-made zips are attached to the [latest release](https://github.com/edouard-kalent/sourcing-agents/releases/latest).

## ChatGPT

1. Settings > Connectors > Advanced > turn on developer mode.
2. Create an app with the URL `https://app.kalent.ai/api/mcp`, then sign in to Kalent.
3. Create a "Kalent Sourcing" project and paste `chatgpt/project-instructions.md` into its instructions.

## Grok

Open the Kalent Grok (full flow): https://x.ai/bot/EmQ0HDJ_-lZ_98KbbNVa2

Or paste `grok/grok-instructions.md` into your own Grok project's instructions.

## Structure

```
.claude-plugin/     Claude Code plugin and marketplace
skills/<agent>/     one SKILL.md per agent
chatgpt/            ChatGPT project instructions (full flow), EN and FR
grok/               Grok instructions (full flow), EN and FR
```

MCP docs: https://docs.kalent.ai/mcp/overview
