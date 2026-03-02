# DispatchNow + Agent-Handoff: The AI Orchestration Stack

> **DispatchNow** is the brain. **Agent-Handoff** is the protocol. Together, they're a complete multi-agent operating system.

---

## What You Achieve

### The Problem
AI agents are powerful individually but chaotic in groups. Without structure:
- Sub-agents get full conversation dumps (wasted tokens, confused outputs)
- No retry logic — one failure kills the whole pipeline
- No clear ownership — three systems run the same task, all badly
- Handoffs between dev-time AI and runtime AI lose context entirely

### The Solution

| Layer | Skill | What It Does |
|-------|-------|-------------|
| **Planning** | DispatchNow | Decomposes complex tasks, maps dependencies, assigns agents |
| **Execution** | DispatchNow | Structured prompts (ROLE/CONTEXT/TASK/FORMAT), context isolation, parallel dispatch |
| **Recovery** | DispatchNow | Diagnose → Reframe → Retry (1x) → Escalate with full transparency |
| **Quality** | DispatchNow | Self-evaluation: completeness, consistency, confidence rating |
| **Delegation** | Agent-Handoff | Produces ORCHESTRATOR.md runbooks for autonomous agents |
| **Scheduling** | Agent-Handoff | Cron jobs with isolated sessions, timeouts, model selection |
| **Ownership** | Agent-Handoff | One owner per task — resolves systemd/n8n/agent-cron conflicts |
| **Monitoring** | Agent-Handoff | Health checks, mandatory reporting, failure alerts |

### What This Unlocks

1. **"Build it once, run it forever"** — You design a pipeline with Claude Code, DispatchNow coordinates the build across multiple agents, then Agent-Handoff packages it for autonomous 24/7 execution on OpenClaw.

2. **Zero-context-loss handoffs** — Agent-Handoff assumes every session starts fresh. ORCHESTRATOR.md is re-read every run. No "remember last time" failures.

3. **Cost-efficient agent orchestration** — DispatchNow's context isolation means sub-agents only get what they need. No 50k-token dumps to agents that need 2k of context.

4. **Self-healing pipelines** — DispatchNow retries with reframed prompts. Agent-Handoff's health checks catch infrastructure failures. Together: autonomous recovery at both the task and system level.

5. **Audit trail** — DispatchNow logs plans, assignments, and confidence. Agent-Handoff mandates reports after every action. Full visibility without manual monitoring.

---

## How They Work Together

```
┌─────────────────────────────────────────────────┐
│                  USER REQUEST                     │
│         "Build and deploy X, then run it 24/7"   │
└────────────────────┬────────────────────────────┘
                     │
              ┌──────▼──────┐
              │ DISPATCHNOW │  ← Plans the work
              │              │
              │ 1. Plan      │  Goal → Subtasks → Dependencies
              │ 2. Prompt    │  ROLE/CONTEXT/TASK per agent
              │ 3. Dispatch  │  Parallel where possible
              │ 4. Recover   │  Retry failed subtasks
              │ 5. Evaluate  │  Confidence check
              └──────┬──────┘
                     │
                     │ Build complete ✅
                     │
             ┌───────▼────────┐
             │ AGENT-HANDOFF  │  ← Packages for autonomy
             │                 │
             │ 1. Discover     │  Map all automation
             │ 2. Resolve      │  One owner per task
             │ 3. Author       │  ORCHESTRATOR.md runbook
             │ 4. Schedule     │  Cron jobs + timeouts
             │ 5. Monitor      │  Health checks + reporting
             │ 6. Verify       │  End-to-end test
             └───────┬────────┘
                     │
              ┌──────▼──────┐
              │  AUTONOMOUS  │  ← Runs forever
              │    AGENT     │
              │  (OpenClaw)  │
              └─────────────┘
```

---

## Real-World Scenarios

### Scenario 1: Music Promotion Pipeline
1. **DispatchNow** plans: curator enrichment → email warmup → outreach → follow-ups → reporting
2. Sub-agents build each piece in parallel (scraper, email templates, n8n workflows)
3. **Agent-Handoff** packages it: ORCHESTRATOR.md for OpenClaw, 4 cron jobs, health checks on Supabase queue, Telegram daily briefing

### Scenario 2: Multi-App Deployment
1. **DispatchNow** coordinates: frontend build → API deploy → DNS config → monitoring setup
2. Each subtask gets only its relevant context (frontend agent doesn't see API credentials)
3. **Agent-Handoff** sets up: systemd timers for health pings, agent cron for SSL renewal checks, n8n webhook for deploy notifications

### Scenario 3: Content Engine
1. **DispatchNow** orchestrates: research → write → SEO optimize → publish → distribute
2. Parallel dispatch: research + SEO audit run simultaneously
3. **Agent-Handoff** automates: weekly publishing schedule, LinkedIn cross-posting, engagement monitoring

---

## Comparison: Without vs. With

| Without | With DispatchNow + Agent-Handoff |
|---------|----------------------------------|
| Dump everything to one agent, hope it works | Structured plan, minimal context per agent |
| Task fails → start over manually | Auto-retry with reframed prompt, then escalate |
| Copy-paste instructions to VPS agent | ORCHESTRATOR.md runbook, auto-generated |
| 3 systems doing the same job | One owner per task, conflicts resolved |
| "Did it run last night?" → SSH in and check | Mandatory Telegram reports + health checks |
| Agent forgets context between sessions | ORCHESTRATOR.md re-read every session |

---

## Metrics You Can Expect

- **Token savings**: 40-70% reduction via context isolation (no full-history dumps)
- **Failure recovery**: ~80% of sub-agent failures recovered via reframe+retry
- **Handoff time**: Pipeline → autonomous in minutes, not hours of manual config
- **Zero drift**: ORCHESTRATOR.md is the single source of truth, version-controlled

---

## Getting Started

### Install Both Skills
```bash
openclaw skills add stringztechnologies/dispatchnow
openclaw skills add stringztechnologies/openclaw-skills --path skills/agent-handoff
```

### Quick Test
Ask your agent:
> "Build a daily SEO audit for towji.com, then hand it off to run autonomously on the VPS"

Watch DispatchNow plan the subtasks, coordinate the build, then Agent-Handoff produce the ORCHESTRATOR.md and cron config.

---

*Built by [@Gemvolta](https://t.me/Gemvolta) / Stringz Technologies*
