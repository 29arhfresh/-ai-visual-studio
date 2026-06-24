---
name: refine
description: >
  Run /refine to make Claude iteratively improve any content until it reaches peak quality on its own — no human feedback needed between rounds. Use this skill whenever the user types /refine, says "polish this", "make this better", "keep improving until it's done", "self-improve this", "iterate on this", "refine this until it's perfect", "loop until quality", or pastes content and asks Claude to max it out. Works universally: text, scripts, posts, emails, prompts, code, captions, strategies, plans. Claude generates its own quality criteria, critiques its own output, rewrites, and repeats — stopping only when improvement delta hits zero or the iteration cap is reached. Always trigger when the user wants autonomous multi-pass improvement without giving feedback each round.
---

# Refine Skill

Autonomous self-improvement loop. Claude generates, critiques, rewrites — round after round — until quality peaks or the iteration cap is hit.

**Core rule: no human input between iterations. The loop runs to completion. Then report.**

---

## Invocation

Trigger on `/refine` or any phrase from the description.

Input format (flexible — accept any of these):
- `/refine [content]` — improve existing content
- `/refine [task description]` — generate from scratch, then refine
- `/refine [task description] /// [content]` — task context + existing content
- `/refine max=[N] [content]` — custom iteration cap (default: 5)

---

## Phase 1 — Parse input

Extract:
1. **Task** — what is this content supposed to do? (infer from content if not stated)
2. **Content** — what to improve (or generate if missing)
3. **Max iterations** — from `max=N` if present, otherwise default to 5

If content is missing and task is too vague to generate from:
> "What should I generate or improve? Give me the content or describe the task."

One question only. Then proceed.

---

## Phase 2 — Derive quality criteria

Before iteration 1, internally determine criteria based on content type.

**Do not show criteria to the user. Use them silently as the scoring rubric.**

Examples by type (not exhaustive — adapt to what you're given):

| Content type | Key criteria |
|---|---|
| Instagram post / Reels script | Hook strength, retention, emotional pull, CTA clarity, natural language |
| Email | Subject line power, clarity, one clear ask, tone match |
| AI image/video prompt | Specificity, technical accuracy, visual logic, negative constraints |
| Code | Correctness, edge case handling, readability, minimal footprint |
| Strategy / plan | Logic, completeness, actionability, prioritization |
| Generic text | Clarity, concision, impact, structure |

Criteria must be **specific and measurable** — not "good writing" but "hook lands in first 5 words", "zero filler phrases", "single clear action per sentence".

---

## Phase 3 — Iteration loop

Run up to `max` iterations. Each iteration:

### Step A — Generate or rewrite
- Iteration 1: if content provided, use it as starting point. If not, generate from scratch.
- Iterations 2+: rewrite based on critique from previous step.

### Step B — Self-critique
Score against each criterion. Identify:
- What is failing or underperforming
- What specific change would fix it
- Whether the change would produce meaningful improvement

**If no criterion is failing and no change would produce meaningful improvement → stop early.**

State internally: `DELTA = 0. Stopping at iteration N.`

### Step C — Decide: continue or stop
Continue if:
- At least one meaningful improvement identified
- Iteration count < max

Stop if:
- No meaningful improvements found (DELTA = 0)
- Iteration cap reached

---

## Phase 4 — Output

Show nothing during the loop. Output only when done.

### Format:

```
## Result

[Final version of the content — clean, no markup unless content requires it]

---

## Refinement summary

Iterations: X of Y
Stopped: [early — quality peaked] / [at cap]

| Iteration | What changed |
|-----------|-------------|
| 1 | [What was weak, what was fixed] |
| 2 | [What was weak, what was fixed] |
| ... | ... |

Quality ceiling hit: [yes / not yet — more iterations may help]
```

---

## Rules

- Never ask for feedback mid-loop
- Never show intermediate versions unless user asks
- Never inflate iteration count — stop when genuinely done
- Never report vague improvements ("made it better") — be specific ("replaced passive verb in line 2, cut 3 filler words, sharpened hook")
- If content type is ambiguous, infer from context — do not ask
- If task is impossible to improve further on iteration 1, say so directly and explain why

---

## Failure modes to avoid

| Temptation | Rule |
|---|---|
| "I'll run all 5 iterations even if done at 2" | Stop when DELTA = 0 |
| "I'll show my work each round" | Silent loop, output at end only |
| "I'll ask what good looks like" | Derive criteria yourself |
| "I'll make minor cosmetic tweaks to seem busy" | Only count changes that move a criterion score |
| "I'll report vague summaries" | Every iteration entry must name the specific fix |

