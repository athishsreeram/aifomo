# AI FOMO management — agentic skill pack

4 skills that chain into a fully automated weekly pipeline. Built for Claude Code on your Mac mini.

---

## Setup (one time)

```bash
# 1. Copy skill files into your Claude Code project
mkdir -p ~/.claude/skills/ai-fomo
cp SKILL_*.md ~/.claude/skills/ai-fomo/

# 2. Create slash commands (optional but recommended)
mkdir -p ~/.claude/commands
cat > ~/.claude/commands/scan-inbox.md << 'EOF'
Read /Users/athish/.claude/skills/ai-fomo/SKILL_1_scan.md and execute it. Write output to inbox.md in the current directory.
EOF

cat > ~/.claude/commands/triage-inbox.md << 'EOF'
Read /Users/athish/.claude/skills/ai-fomo/SKILL_2_triage.md and execute it against inbox.md in the current directory. Write output to triage.md.
EOF

cat > ~/.claude/commands/run-spike.md << 'EOF'
Read /Users/athish/.claude/skills/ai-fomo/SKILL_3_spike.md and execute it. Read triage.md for the top spike pick.
EOF

cat > ~/.claude/commands/publish-spike.md << 'EOF'
Read /Users/athish/.claude/skills/ai-fomo/SKILL_4_publish.md and execute it. Read spike-log.md for the latest entry.
EOF

# 3. Create your working directory
mkdir -p ~/ai-fomo-pipeline
cd ~/ai-fomo-pipeline
touch spike-log.md weekly-log.md
```

---

## Weekly execution

### Option A — Full pipeline (one command)
```bash
cd ~/ai-fomo-pipeline
claude "
Execute the AI FOMO pipeline in order:
1. Read ~/.claude/skills/ai-fomo/SKILL_1_scan.md and run it. Write inbox.md.
2. Read ~/.claude/skills/ai-fomo/SKILL_2_triage.md and run it against inbox.md. Write triage.md.
3. Read ~/.claude/skills/ai-fomo/SKILL_3_spike.md and run it. Use top spike pick from triage.md.
4. Read ~/.claude/skills/ai-fomo/SKILL_4_publish.md and run it.
Confirm completion of each step before proceeding to the next.
"
```

### Option B — Slash commands (step by step)
```bash
cd ~/ai-fomo-pipeline
/scan-inbox        # Monday: collect the week's signal
/triage-inbox      # Monday: sort and score
/run-spike         # Wednesday: 90-min build session
/publish-spike     # Wednesday: draft post + update log
```

### Option C — Triage only (skip spike when busy)
```bash
/scan-inbox
/triage-inbox
# Review triage.md manually, defer spike to next week
```

---

## File outputs per week

| File | Created by | Purpose |
|---|---|---|
| `inbox.md` | SKILL_1 | Raw collected items |
| `triage.md` | SKILL_2 | Scored + bucketed items |
| `spike-[slug].html` | SKILL_3 | The actual built demo |
| `spike-log.md` | SKILL_3 | Running log of all spikes |
| `post-draft.md` | SKILL_4 | LinkedIn post ready to review |
| `weekly-log.md` | SKILL_4 | Notion-pasteable weekly summary |

---

## After the pipeline runs

**Deploy the spike (2 min):**
```bash
cp ~/ai-fomo-pipeline/spike-*.html ~/github/athishsreeram.github.io/spikes/
cd ~/github/athishsreeram.github.io
git add spikes/ && git commit -m "spike: $(date +%Y-%m-%d)" && git push
```

**Review and post (5 min):**
```bash
cat ~/ai-fomo-pipeline/post-draft.md
# Edit if needed, paste into LinkedIn
```

**Paste weekly log into Notion (1 min):**
```bash
cat ~/ai-fomo-pipeline/weekly-log.md
# Copy latest week block into your Notion AI Inbox database
```

---

## Cadence

| Day | Action | Time |
|---|---|---|
| Monday | Run SKILL_1 + SKILL_2 | 30 min |
| Wednesday | Run SKILL_3 + SKILL_4 | 90 min spike + 10 min publish |
| Friday (optional) | Review WATCH items | 5 min |

---

## Skills at a glance

| Skill | Input | Output | Time |
|---|---|---|---|
| SKILL_1_scan.md | 4 live URLs | inbox.md | ~5 min |
| SKILL_2_triage.md | inbox.md | triage.md | ~5 min |
| SKILL_3_spike.md | triage.md top pick | spike-[slug].html + log | 90 min |
| SKILL_4_publish.md | spike-log.md | post-draft.md + weekly-log.md | ~5 min |


<img width="1440" height="2094" alt="image" src="https://github.com/user-attachments/assets/ed8b48f8-4475-4f8c-bbde-ecd0d46ed10b" />

<img width="1440" height="1930" alt="image" src="https://github.com/user-attachments/assets/3cd53954-42e4-4420-b681-6bfa5a98dd31" />


