# SKILL 1 — scan & fetch
**Trigger:** Run this every Monday morning. Takes ~5 min. Feeds into SKILL_2_triage.

---

## What this skill does
Fetches the week's AI signal from your 4 sources, deduplicates, and writes a raw `inbox.md` file ready for triage. No decisions made here — pure collection.

---

## Sources to hit (in order)

| Source | URL | How to fetch |
|---|---|---|
| TLDR AI | `https://tldr.tech/ai` | Web fetch latest issue |
| Simon Willison | `https://simonwillison.net/` | Web fetch, grab last 7 days of posts |
| Anthropic changelog | `https://www.anthropic.com/changelog` | Web fetch |
| OpenAI changelog | `https://platform.openai.com/docs/changelog` | Web fetch |

---

## Agent instructions

```
You are running SKILL_1 of the AI FOMO management pipeline.

STEP 1 — Fetch each source URL above using web search or web fetch.
STEP 2 — Extract only items published in the last 7 days.
STEP 3 — For each item extract exactly: {title, source, url, date, one_line_summary}.
STEP 4 — Deduplicate: if the same announcement appears across multiple sources, keep one entry (prefer the original source).
STEP 5 — Write the result as structured markdown to inbox.md using this exact format:

---
## inbox — week of {{ISO_DATE}}

| # | title | source | date | summary |
|---|---|---|---|---|
| 1 | ... | TLDR AI | ... | ... |
| 2 | ... | Simon Willison | ... | ... |

Total items: N
---

STEP 6 — Print a one-line summary: "Scan complete. N items collected from 4 sources."

Do NOT triage, score, or filter anything in this skill. That is SKILL_2's job.
```

---

## Output
File: `inbox.md` in your working directory.
Expected item count: 8–20 per week. If > 30, the sources have noise — note it.

---

## How to invoke (Claude Code)
```bash
# In your terminal with Claude Code active:
claude "Read SKILL_1_scan.md and execute it. Write output to inbox.md"
```

Or as a slash command if you add to `.claude/commands/`:
```bash
/scan-inbox
```

---

## Failure handling
- If a source returns no content or 404 → note it as "unavailable this week" and continue with remaining sources.
- If fewer than 3 sources succeed → stop and report: "Scan incomplete. Manual check needed."
- Never hallucinate items. If you can't fetch, say so.
