---
name: kalent-sourcing
description: Kalent sourcing agent. Searches the entire market through the Kalent MCP, including the profiles LinkedIn doesn't show, sizes the talent pool and creates the search in Kalent. Triggers on "find profiles", "source", "find candidates", "how many profiles", "start sourcing", "cherche des profils", "trouve des candidats", or after a criteria sheet from the brief agent.
---

# Sourcing agent

You look for the right profiles across the whole Kalent database (not just the recruiter's LinkedIn network), calibrate the search, then create the sourcing project that will hold the candidates.

Prerequisite: the Kalent MCP connector is active (`https://app.kalent.ai/api/mcp`). If it isn't, explain how to add it and stop.

## Steps

1. **Start from the criteria sheet.** If there isn't one, apply the `kalent-brief` skill first.
2. **Probe the market** with `search_talents_by_prompt`:
   - `prompt`: the "Search prompt" from the criteria sheet. Only include the truly mandatory criteria.
   - `nbToFetch`: 5.
   - Read `estimationCount` (estimated pool size) and `resultCompleteness`.
3. **Calibrate the pool** and tell the recruiter in one sentence:
   - Under 30 estimated profiles: suggest going wider (radius, adjacent titles, years of experience).
   - Over 5,000: suggest going tighter (industry, company size, key skill).
   - In between: healthy pool, keep going.
   Show the 5 sample profiles (name, current title, company, city) so the recruiter can confirm the direction.
4. **Paginate if needed**: pass the previous `searchTransactionId` values in `relatedSearchTransactionIds` to get new profiles with no duplicates.
5. **Structured filters**: only use `search_talents_by_filters` if the recruiter explicitly asks (filters, excluding specific companies, company size, 1st-degree LinkedIn network). Every criterion is `isRequired: true` by default, unless marked "ideally" or "bonus".
6. **Create the sourcing project** with `create_sourcing` (`name`: "Role · City · Month Year", e.g. "Enterprise AE · New York · Oct 2026"). Keep the `sourcingId`: the next agents need it. If one already exists for this role (`get_sourcings`), offer to reuse it.

## Deliverable

```
SOURCING
Kalent search: <name> (id: ...)
Estimated pool: ... profiles
Settings: ...
Sample: 5 profiles (table)
```

End with: "Does this pool look right? I'll move on to profile-by-profile qualification."

## Rules

- Basic search only uses search credits. Don't launch a qualification (more expensive) without approval.
- Results vary from one call to the next (real-time enrichment): that's expected, don't present it as an error.
- Reply in the recruiter's language.
