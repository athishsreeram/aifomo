# SKILL 3 — spike & build
**Trigger:** Run against the "Top spike pick" from triage.md. One spike per week max.
**Input:** `triage.md` (specifically the Top spike pick item)
**Output:** A working single-file `spike-[slug].html` + `spike-log.md` entry

---

## The spike contract
- **Time box: 90 minutes.** If you can't produce a working demo, ship what exists and mark as DROPPED.
- **Single file only.** No npm, no build step, no backend. Pure HTML + vanilla JS + inline CSS.
- **Deploy target:** GitHub Pages (`athishsreeram.github.io`). Must work on static hosting.
- **Ship or drop decision:** At end of spike, it's either SHIPPED (has a live URL) or DROPPED (logged with reason). No "in progress."

---

## Agent instructions

```
You are running SKILL_3 of the AI FOMO management pipeline.

INPUT: Read triage.md. Extract the Top spike pick item and its spike idea.

STEP 1 — PLAN (5 min budget)
State in 3 bullet points:
- What exactly you will build
- What the "done" condition looks like
- What the single biggest technical risk is

STEP 2 — BUILD (75 min budget)
Generate a complete, working single-file HTML spike.

Rules for the spike file:
  - Filename: spike-[kebab-slug-of-topic].html
  - Must open directly in browser with no server
  - Must work on GitHub Pages (no server-side code, no private API keys hardcoded)
  - Include a visible header: "AI Spike: [topic] — [date]"
  - Include a visible "What I learned" section at the bottom of the page (rendered in the HTML itself)
  - No external dependencies except CDN links from cdnjs.cloudflare.com or unpkg.com
  - If the spike requires an API key (e.g. Claude API), use an input field — never hardcode

STEP 3 — VERDICT (10 min budget)
After generating the file, produce a spike verdict block:

---
SPIKE VERDICT
Topic: [title from triage]
Date: [ISO date]
Status: SHIPPED | DROPPED
File: spike-[slug].html
Deploy: https://athishsreeram.github.io/spikes/spike-[slug].html (update after push)
Score: [1-5] — how useful was this?
Key learning: [one sentence — what actually worked or failed]
LinkedIn angle: [one sentence — the post hook]
Time used: ~[N] min of 90min budget
---

STEP 4 — APPEND to spike-log.md
Append the verdict block above to spike-log.md (create file if it doesn't exist).

STEP 5 — Print: "Spike complete. Status: [SHIPPED|DROPPED]. File: spike-[slug].html"
```

---

## Drop conditions (stop immediately, log as DROPPED)
- The technology requires a paid API with no free tier
- The technology requires a backend — cannot be made static
- After 45 min you have nothing running — pivot to documenting the failure
- The spike idea was wrong: it doesn't actually demonstrate what triage expected

---

## How to invoke
```bash
claude "Read SKILL_3_spike.md and execute it. Read triage.md for the top spike pick."
```

Slash command:
```bash
/run-spike
```

---

## GitHub deploy (manual step after skill runs)
```bash
# After spike file is generated:
cp spike-*.html /path/to/athishsreeram.github.io/spikes/
cd /path/to/athishsreeram.github.io
git add spikes/
git commit -m "spike: [topic] — [date]"
git push
```

The deploy step is intentionally manual — it's the "ship or drop" decision point where you review the output before it goes live.

---

## spike-log.md format
```markdown
# spike log

| date | topic | status | score | key learning |
|---|---|---|---|---|
| 2026-04-07 | WebLLM in ChessKids | SHIPPED | 4 | Runs in-browser but 500MB model load is too slow for kids |
| 2026-04-14 | ... | DROPPED | — | Required Node.js server |
```
