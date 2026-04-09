# SKILL 4 — publish & log
**Trigger:** Run immediately after SKILL_3 regardless of SHIPPED or DROPPED status.
**Input:** Latest entry in `spike-log.md` + the spike HTML file
**Output:** `post-draft.md` (LinkedIn post) + updated Notion-ready `weekly-log.md`

---

## What this skill does
Turns every spike — win or loss — into LinkedIn content and a structured weekly log entry. The post is drafted, not published. You review and post manually (keeps authenticity).

---

## Agent instructions

```
You are running SKILL_4 of the AI FOMO management pipeline.

INPUT: Read the latest spike verdict from spike-log.md.

STEP 1 — Determine post type based on spike status:
  - SHIPPED → "I built X" format (show the thing, share the learning)
  - DROPPED → "I tried X and stopped" format (more honest, often higher engagement)

STEP 2 — Write a LinkedIn post draft. Rules:

FORMAT:
  - Line 1: Hook — one punchy sentence, no "I'm excited to share" opener
  - Lines 2–4: What you built/tried and why (3 short lines max)
  - Lines 5–8: The actual learning — what worked, what didn't, what surprised you
  - Line 9: Call to action or open question to the audience
  - Line 10: GitHub Pages URL if SHIPPED (proof of work)
  - Final 3 lines: hashtags (#buildinpublic #edtech #ai — match your niche)

TONE: Direct, builder voice. No corporate speak. First person. Honest about failures.
LENGTH: 150–250 words. Enough to be substantial, short enough to be read.

DO NOT:
  - Use "excited to share" or "thrilled to announce"
  - Use "game-changing" or "revolutionary"  
  - Pad with context that adds no value
  - Add a summary line that repeats what you already said

STEP 3 — Write post-draft.md with:
  - The full post text
  - A "suggested image" line: what screenshot or visual to attach
  - A "best time to post" line: Tuesday–Thursday 8–10am or 5–7pm EST

STEP 4 — Append to weekly-log.md (Notion-paste-ready markdown):

---
## week of {{ISO_DATE}}

**Scan:** N items collected  
**Triage:** N try-now / N watch / N archive  
**Spike:** [topic] — [SHIPPED|DROPPED]  
**Score:** [1-5]  
**Key learning:** [one sentence]  
**Post:** drafted — [first 10 words of hook]...  
**Notion triage update needed:** [list any WATCH items hitting 2-week mark]

---

STEP 5 — Run monthly prune check:
  Read triage.md. Find any WATCH items with revisit date older than today.
  List them as: "WATCH items to prune: [titles]" — these should move to ARCHIVE.
  If none: "No pruning needed this week."

STEP 6 — Print final pipeline summary:
"Pipeline complete for week of [date].
  Scan: N items → Triage: N TRY NOW, N WATCH, N ARCHIVE → Spike: [topic] [STATUS] → Post: drafted
  Next action: Review post-draft.md and push spike to GitHub Pages."
```

---

## How to invoke
```bash
claude "Read SKILL_4_publish.md and execute it."
```

Slash command:
```bash
/publish-spike
```

---

## post-draft.md example output
```markdown
# LinkedIn post draft — 2026-04-07

---
Tried running a language model entirely in the browser inside ChessKids. It actually works.

The model loads from Hugging Face, runs via WebLLM, and generates chess hints with zero server calls.
Took 85 of my 90-minute spike budget to get it working.

The catch: first load is 480MB. That's not okay for a kids' app on mobile.

So I shipped it anyway — as a "what's possible" demo — and documented what would need to change to make it production-ready (quantized model, lazy load, cache after first visit).

Sometimes the spike is the lesson, not the product.

Live demo: https://athishsreeram.github.io/spikes/spike-webllm-chesshints.html

What would you want to see in a browser-native chess tutor? Curious what's on your wishlist.

#buildinpublic #edtech #chessforkids #webllm #ai
---

Suggested image: Screenshot of chess board with AI hint bubble appearing
Best time to post: Tuesday 8–10am EST
```

---

## Full pipeline chaining

To run the entire pipeline end-to-end in one Claude Code session:

```bash
claude "
1. Read SKILL_1_scan.md and execute it. Write inbox.md.
2. Read SKILL_2_triage.md and execute it against inbox.md. Write triage.md.
3. Read SKILL_3_spike.md and execute it using the top spike pick from triage.md.
4. Read SKILL_4_publish.md and execute it.
Report status after each step before continuing.
"
```

Or chain as separate slash commands run sequentially:
```bash
/scan-inbox && /triage-inbox && /run-spike && /publish-spike
```
