---
name: dispatchnow
description: "AI agent orchestration and operational delegation. Use when: (1) spawning sub-agents for multi-step tasks, (2) delegating pipelines to run autonomously, (3) discovering/auditing infrastructure automation, (4) resolving ownership conflicts between platforms (systemd, n8n, OpenClaw), (5) generating ORCHESTRATOR.md runbooks for zero-context agents. Includes the Dispatch Protocol CLI for infrastructure management. Triggers on: multi-agent delegation, task decomposition, pipeline handoff, infrastructure audit, autonomous operations, cron/timer/workflow management."
---

# DispatchNow — Agent Orchestration + Operational Delegation

Two layers: **how to think** (orchestration rules) and **how to act** (Dispatch Protocol CLI).

---

## Part 1: Orchestration Rules

Follow these rules whenever delegating work to sub-agents.

### Before Delegating: Plan

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

### Prompting Sub-Agents

Construct each sub-agent prompt using this template:

1. **ROLE** — Assign a specific expert identity
2. **CONTEXT** — Only what's needed (summarize, don't dump)
3. **TASK** — One clear, specific instruction
4. **FORMAT** — Exact output structure expected
5. **CONSTRAINTS** — Boundaries (length, style, what NOT to do)
6. **EXAMPLES** — If ambiguous, include 1 input/output example

Self-rate prompt clarity 1–5 before sending. If below 4, revise.

### Context Isolation

For each sub-agent call, include:

- ONLY the context relevant to that specific subtask
- A summary of prior subtask results it depends on
- The original user intent (1 sentence, for grounding)

Never pass full conversation history to every agent.

### Error Recovery

When a sub-agent returns an error or low-confidence result:

1. **DIAGNOSE** — Was the prompt unclear? Context missing? Task outside scope?
2. **REFRAME** — Rewrite the sub-task with more specific instructions, additional context from completed subtasks, or a simpler decomposition
3. **RETRY** — Send the reframed task (max 1 retry)
4. **ESCALATE** — If retry fails, inform the user with what was attempted, why it failed, and what they can do

### Quality Evaluation

After synthesizing the final response, evaluate:

1. Does it fully address the user's original request?
2. Are there gaps where a subtask was skipped or failed?
3. Is the response internally consistent?
4. **Confidence**: high / medium / low

If medium or low, state what's uncertain and why.

---

## Part 2: Dispatch Protocol CLI

When the user needs to **hand off operational tasks to autonomous agents** or **audit their infrastructure**, use the Dispatch Protocol CLI.

### Install

```bash
pip install dispatch-protocol
# Or from source:
git clone https://github.com/stringztechnologies/dispatch-protocol
cd dispatch-protocol && pip install -e ".[dev]"
```

### Three Commands

```bash
dispatch discover    # What's running? Where? Who owns what?
dispatch plan        # Propose ownership. Resolve conflicts.
dispatch deploy      # Push configs. Compile runbook. Verify.
```

### When to Use Each Command

| User says | Run |
|-----------|-----|
| "What's running on my server?" | `dispatch discover --local` |
| "Audit my automation" | `dispatch discover --ssh user@host` |
| "Which system should own this task?" | `dispatch plan` |
| "Hand this off to run autonomously" | `dispatch plan` → `dispatch deploy` |
| "Generate an ORCHESTRATOR.md" | `dispatch compile --project-name "X"` |
| "Deploy but let me review first" | `dispatch deploy --dry-run` |
| "Set up OpenClaw to handle enrichment" | `dispatch plan` → `dispatch deploy` |
| "Make this pipeline autonomous" | Full pipeline: discover → plan → deploy |

### Full Pipeline Example

```bash
# 1. Discover infrastructure
dispatch discover --local -o ./infra
# Finds: systemd timers, n8n workflows, OpenClaw jobs, scripts
# Generates: agents.yaml, tasks/*.yaml, health.yaml

# 2. Plan ownership
dispatch plan -o plan.yaml
# Assigns one owner per task based on:
#   systemd → high-frequency polling (< 5min)
#   n8n → webhook-triggered flows
#   openclaw → tasks needing AI reasoning/reporting

# 3. Deploy (preview first)
dispatch deploy --plan plan.yaml --dry-run

# 4. Deploy for real
dispatch deploy --plan plan.yaml \
  --project-name "My Project" \
  --project-path "/root/workspace/my-project"
# Pushes configs, compiles ORCHESTRATOR.md, runs verification
```

### Core Principle

**One owner per task.** Every recurring task has exactly one automation system responsible. Not two. Not "both handle it." One. Dispatch Protocol detects and resolves conflicts.

### What Gets Generated

| File | Purpose |
|------|---------|
| `agents.yaml` | Who can do what (auto-discovered) |
| `tasks/*.yaml` | One contract per task — command, timeout, success metric, escalation |
| `health.yaml` | Health checks + escalation config |
| `plan.yaml` | Ownership plan (review before deploy) |
| `ORCHESTRATOR.md` | Compiled runbook — never hand-edited, regenerated from source of truth |

### Task Contract

A task is a **contract**, not a prompt:

```yaml
task:
  id: enrich-curators
  name: Curator Contact Enrichment
  owner: openclaw
  priority: critical
  execute:
    command: python3 scripts/enrich.py --limit 200
    timeout: 600s
  schedule:
    type: interval
    every: 6h
  failure:
    threshold: 3
    escalate:
      to: claude_code
      context: "Pipeline failing. Diagnose and fix."
```

### Platform Adapters

| Platform | Adapter does |
|----------|-------------|
| **OpenClaw** | Reads/writes jobs.json, creates cron jobs with model selection |
| **n8n** | REST API — activate, deactivate, verify workflows |
| **systemd** | Creates timer+service units, manages via systemctl |

All support local or SSH remote execution.

---

## Combining Both Layers

The orchestration rules (Part 1) govern **how you think** about delegation.
The Dispatch Protocol (Part 2) gives you **what to run** for infrastructure delegation.

**Typical flow:**

1. User asks for something complex
2. Follow Part 1: Plan → decompose → dispatch sub-agents
3. Sub-agents build the pipeline (scripts, workflows, configs)
4. Follow Part 2: `discover` → `plan` → `deploy` to hand it off
5. ORCHESTRATOR.md ensures the autonomous agent knows exactly what to do
6. Evaluate the result per Part 1's quality evaluation

The agent that **plans** the work and the agent that **runs** the work don't need to be the same. DispatchNow bridges them.
