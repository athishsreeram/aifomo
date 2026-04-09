# SKILL 2 — triage & score
**Trigger:** Run immediately after SKILL_1, or manually against any `inbox.md`.
**Input:** `inbox.md` (produced by SKILL_1)
**Output:** `triage.md` with every item scored and bucketed.

---

## The 4-question filter (Athish-specific)

Score each item 0 or 1 per question. Max score = 4.

| Q | Question | Score 1 if... |
|---|---|---|
| Q1 | Ships to GitHub Pages? | No backend needed. Pure HTML/JS/static. |
| Q2 | Helps an active product? | Relevant to ChessKids, Veggie Attack, Phonics Hub, or WebAR menu |
| Q3 | Demoable in under 2 hours? | You could have a working HTML file in one spike session |
| Q4 | LinkedIn content potential? | Has a build-in-public angle for your audience |

---

## Bucket rules

| Score | Bucket | Meaning |
|---|---|---|
| 3–4 | **TRY NOW** | Add to spike backlog immediately |
| 2 | **WATCH** | Re-evaluate in 2 weeks |
| 0–1 | **ARCHIVE** | Log and forget |

**72-hour gate:** Any item less than 72 hours old at time of triage → auto-move to WATCH regardless of score. Mark with flag `[<72hr]`.

---

## Agent instructions

```
You are running SKILL_2 of the AI FOMO management pipeline.

INPUT: Read inbox.md from the current directory.

For each item in the inbox table:

STEP 1 — Check the 72-hour gate. If item.date is within 72 hours of today, assign bucket=WATCH, flag=[<72hr], skip scoring, move on.

STEP 2 — Score Q1 through Q4 (0 or 1 each). Be strict:
  - Q1: only score 1 if it's genuinely deployable to a static host with zero server code
  - Q2: only score 1 if it maps to ChessKids, Veggie Attack, Phonics Hub, or WebAR — not "could theoretically be used in any app"
  - Q3: only score 1 if a single HTML file prototype is achievable in 90 min
  - Q4: only score 1 if there's a concrete "I built X, here's what happened" post angle

STEP 3 — Sum scores, assign bucket per rules above.

STEP 4 — Write triage.md in this exact format:

---
## triage — week of {{ISO_DATE}}
Generated from: inbox.md (N items)

### TRY NOW (score 3-4)
| # | title | score | q1 | q2 | q3 | q4 | spike idea |
|---|---|---|---|---|---|---|---|
| 1 | ... | 4 | 1 | 1 | 1 | 1 | "Integrate X into ChessKids hint system" |

### WATCH (score 2 or <72hr)
| # | title | score | flag | revisit |
|---|---|---|---|---|
| 1 | ... | 2 | | 2 weeks |

### ARCHIVE
| # | title | score | reason |
|---|---|---|---|
| 1 | ... | 1 | No static deploy path |

---
## summary
- TRY NOW: N items
- WATCH: N items  
- ARCHIVE: N items
- Top spike pick: [title] — reason in one sentence
---

STEP 5 — Print: "Triage complete. N items sorted. Top spike: [title]"
```

---

## How to invoke
```bash
claude "Read SKILL_2_triage.md and execute it against inbox.md. Write output to triage.md"
```

Slash command:
```bash
/triage-inbox
```

---

## Notes
- The "spike idea" column is the most important output. Fill it concretely ("Add WebLLM to ChessKids hint engine"), not vaguely ("use in a project").
- If TRY NOW is empty, that's a valid week. Don't force items up just to fill the bucket.
- If TRY NOW has 4+ items, pick the highest-scoring one with the most concrete spike idea as "Top spike pick".
