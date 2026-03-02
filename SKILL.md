---
name: dispatchnow
description: Sub-agent orchestration framework for multi-step task delegation. Use when spawning sub-agents, breaking complex tasks into subtasks, or coordinating parallel/sequential agent work. Provides structured planning, minimal-context prompting, error recovery, and quality evaluation. Triggers on multi-agent delegation, task decomposition, sub-agent coordination, or complex multi-step workflows requiring parallel execution.
---

# DispatchNow — Sub-Agent Orchestration Framework

## Before Delegating: Plan

Output a plan block before any delegation:

```
## Plan
1. **Goal**: Restate the user's actual goal (1 sentence)
2. **Subtasks**: List each subtask
3. **Dependencies**: Which must finish before others start?
4. **Assignment**: Which agent handles each subtask?
5. **Failure mode**: What if an agent fails?
```

Execute the plan step by step. Independent subtasks run in parallel.

## Prompting Sub-Agents

Construct each sub-agent prompt using this template:

1. **ROLE** — Assign a specific expert identity
2. **CONTEXT** — Only what's needed (summarize, don't dump)
3. **TASK** — One clear, specific instruction
4. **FORMAT** — Exact output structure expected
5. **CONSTRAINTS** — Boundaries (length, style, what NOT to do)
6. **EXAMPLES** — If ambiguous, include 1 input/output example

Self-rate prompt clarity 1–5 before sending. If below 4, revise.

## Context Isolation

For each sub-agent call, include:

- ONLY the context relevant to that specific subtask
- A summary of prior subtask results it depends on
- The original user intent (1 sentence, for grounding)

Never pass full conversation history to every agent.

## Error Recovery

When a sub-agent returns an error or low-confidence result:

1. **DIAGNOSE** — Was the prompt unclear? Context missing? Task outside scope?
2. **REFRAME** — Rewrite the sub-task with:
   - More specific instructions
   - Additional context from other completed subtasks
   - A simpler decomposition (break it down further)
3. **RETRY** — Send the reframed task (max 1 retry)
4. **ESCALATE** — If retry fails, inform the user with:
   - What was attempted
   - Why it failed
   - What the user can do to help

## Quality Evaluation

After synthesizing the final response, evaluate:

1. Does it fully address the user's original request?
2. Are there gaps where a subtask was skipped or failed?
3. Is the response internally consistent?
4. **Confidence**: high / medium / low

If medium or low, state what's uncertain and why.
