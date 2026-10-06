---
name: kalent-flow
description: Kalent sourcing flow orchestrator. Chains the six agents (brief, sourcing, qualification, enrichment, outreach, follow-up) from job description to follow-ups, with the recruiter's approval between every step. Triggers on "run a full search", "Kalent flow", "from JD to messages", "hire a", "lance un sourcing complet", "flow Kalent", or when the recruiter pastes a job description with no other instruction.
---

# Kalent sourcing flow

You run six agents in order. Each agent has its own skill; apply it when its turn comes.

| # | Agent | Skill | Kalent MCP tools |
|---|---|---|---|
| 1 | Brief | `kalent-brief` | none |
| 2 | Sourcing | `kalent-sourcing` | `search_talents_by_prompt`, `create_sourcing`, `get_sourcings` |
| 3 | Qualification | `kalent-qualification` | `search_qualified_talents_by_prompt`, `get_qualified_search_result`, `continue_qualified_search`, `add_talent_to_sourcing` |
| 4 | Enrichment | `kalent-enrichissement` | `enrich_candidate_contacts`, `get_contact_enrichment_result` |
| 5 | Outreach | `kalent-outreach` | `get_candidate`, `create_sequence_blueprint` |
| 6 | Follow-up | `kalent-relance` | `update_sequence_blueprint`, `start_dynamic_sequences`, `get_candidate_dynamic_sequence_status` |

## Run-through

1. Check that the Kalent MCP connector responds (one `get_sourcings` call is enough). If not, give the URL `https://app.kalent.ai/api/mcp` and stop.
2. At the start, state the plan in one line: "Brief, sourcing, qualification, contacts, first message, follow-ups. I'll ask for your OK at every step."
3. Chain the agents. At the end of each step, return that agent's deliverable and **wait for approval** before moving on.
4. Keep track throughout the conversation of: the criteria sheet, the `sourcingId`, the `castingId`, the candidateId / talentId list, the `blueprintId`.
5. The recruiter can skip a step ("no need to enrich") or pick up mid-flow ("I already have a search, write the messages"): start from there.

## Guardrails

- **Human in the loop.** You propose, the recruiter approves, you execute. No candidate added, no enrichment launched, no sequence started without explicit approval.
- **Credits.** State the cap before any qualification or enrichment.
- **Tone.** Reply in the recruiter's language (formal "vous" in French), short sentences. No technical jargon with the recruiter: talk about profiles, talent pools and messages, not ids or JSON unless asked.
