---
name: interview
description: Generate user interview scripts, analyze interview transcripts, and extract actionable product insights. Use when user says "create interview guide", "analyze interview", "user research", "interview questions", "research plan", or needs to understand user needs through qualitative research.
user-invokable: true
args:
  - name: target
    description: The topic, feature, or transcript to work with (optional)
    required: false
metadata:
  author: builder-skills
  version: "1.0.0"
---

# User Interviews

This skill operates in two modes. Pick the one that matches what you have.

- **GENERATE** — You have a topic or feature area. You need an interview script.
- **ANALYZE** — You have a transcript (or notes). You need structured insights.

---

## GENERATE Mode — Build an Interview Script

Structure every interview in three acts:

```
┌─────────────────────────────────────────┐
│  1. WARM-UP (5 min)                     │  Build rapport, set context
│  2. CORE QUESTIONS (30–40 min)          │  Explore the job, past behavior
│  3. WRAP-UP (5 min)                     │  Open floor, next steps
└─────────────────────────────────────────┘
```

### Warm-Up

Set the participant at ease. Explain there are no wrong answers, you're learning from them, and they can skip anything. Ask one or two easy background questions to get them talking.

### Core Questions

Use these question types, in roughly this order:

| Type | Purpose | Starter |
|------|---------|---------|
| **Context** | Understand their world | "Walk me through a typical day when you..." |
| **Behavioral** | Surface real past actions | "Tell me about the last time you..." |
| **JTBD** | Uncover the job and switching triggers | "What were you trying to accomplish when you decided to..." |
| **Process** | Map their current workflow | "What happened next? And after that?" |
| **Emotional** | Find pain and delight | "How did that make you feel? What was the hardest part?" |
| **Magnitude** | Gauge importance | "How big a deal is that for you? What's at stake?" |

Follow every answer with "Why?" or "Can you tell me more about that?" until you reach concrete specifics.

### Wrap-Up

Ask "Is there anything I should have asked but didn't?" — this regularly surfaces the most valuable insight of the session. Thank them and explain what happens next.

### Good vs Bad Questions

| Bad (avoid) | Why it fails | Good (use instead) |
|-------------|-------------|-------------------|
| "Would you use a feature that does X?" | Hypothetical — people can't predict future behavior | "Tell me about the last time you needed to do X." |
| "Don't you think this is frustrating?" | Leading — suggests the answer | "How do you feel about that part of the process?" |
| "Do you like A or B better?" | Binary — closes exploration | "What matters most to you when you're doing this?" |
| "How often would you use this?" | Speculative frequency | "How often did you do this in the past month?" |
| "Users like you typically want..." | Primes agreement | "What do you wish were different?" |

### Output

Produce the script as a numbered list of questions, grouped by section, with follow-up prompts indented beneath each question. Include a short interviewer note at the top covering the research goal, who to recruit, and session logistics.

---

## ANALYZE Mode — Extract Insights from a Transcript

Work through the transcript systematically. Pull out five things:

| Extract | What to look for |
|---------|-----------------|
| **Themes** | Recurring topics or patterns across answers |
| **Jobs** | What the participant was trying to accomplish (verb + object + clarifier) |
| **Needs** | How they measure success — stated or implied (direction + measure + object) |
| **Pain points** | Where the current process breaks down, causes frustration, or wastes time |
| **Opportunities** | Unmet needs, workarounds, or moments of high emotional energy |

### How to work through it

1. Read the full transcript before highlighting anything.
2. Tag each meaningful quote with one or more of the five categories above.
3. Group related quotes into clusters. Name each cluster with a short theme label.
4. For each cluster, write one insight statement: "[Who] needs to [job] but struggles with [pain] because [root cause]."
5. Rank opportunities by how often they appear and how strongly participants reacted.

### Output

Produce a structured summary with:
- **Research context** — who was interviewed, when, about what
- **Top themes** — ranked list with supporting quotes
- **Jobs identified** — written as proper job statements
- **Key pain points** — with severity and frequency indicators
- **Opportunities** — specific, actionable, tied back to evidence
