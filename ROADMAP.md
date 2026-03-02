# Growth Roadmap: DispatchNow + Agent-Handoff

## Phase 1: Foundation Improvements (Now)

### DispatchNow
- [ ] **Dependency graph visualization** — Auto-generate Mermaid diagrams of subtask dependencies
- [ ] **Token budget tracking** — Track context tokens per sub-agent, warn when approaching limits
- [ ] **Prompt templates library** — Pre-built ROLE/CONTEXT/TASK templates for common patterns (code review, content writing, data analysis)
- [ ] **Retry memory** — Store what failed and why, so reframed prompts reference past failure modes
- [ ] **Parallel dispatch batching** — Group independent subtasks and dispatch in optimized batches

### Agent-Handoff
- [ ] **ORCHESTRATOR.md linter** — Validate runbooks for completeness (paths exist, commands executable, no relative paths)
- [ ] **Diff-aware updates** — When pipeline changes, update only affected sections of ORCHESTRATOR.md
- [ ] **Multi-agent platform support** — Templates for Devin, Cursor background agents, Windsurf, not just OpenClaw
- [ ] **Credential rotation hooks** — Detect expired tokens in ORCHESTRATOR.md, prompt for refresh

## Phase 2: Intelligence Layer (Month 2-3)

### DispatchNow
- [ ] **Adaptive model selection** — Auto-pick cheapest model per subtask complexity (codex for scripts, flash for summaries, sonnet for reasoning)
- [ ] **Subtask result caching** — If the same subtask runs again, check cache before re-dispatching
- [ ] **Confidence-based routing** — Low-confidence results auto-route to a more capable model for verification
- [ ] **Plan learning** — Store successful plans and reuse patterns for similar requests
- [ ] **Cross-session delegation** — Dispatch subtasks to persistent sessions that survive beyond the parent conversation

### Agent-Handoff
- [ ] **Self-healing orchestrator** — ORCHESTRATOR.md includes decision trees: "if X fails, try Y before alerting"
- [ ] **Gradual autonomy** — Start with human-in-the-loop approvals, auto-escalate trust as success rate proves out
- [ ] **Metrics dashboard** — Track task success rates, execution times, failure patterns per delegated task
- [ ] **Hot-swap agents** — If OpenClaw is down, auto-failover to n8n or systemd for critical tasks

## Phase 3: Ecosystem (Month 4-6)

### Combined
- [ ] **Skill composition protocol** — DispatchNow auto-detects when Agent-Handoff should run (build complete → delegation needed)
- [ ] **Pipeline-as-code** — Define entire orchestration + delegation in a single YAML/MD file
- [ ] **Community plan library** — Publish and share successful orchestration plans on ClawHub
- [ ] **Multi-VPS coordination** — DispatchNow orchestrates across multiple servers, Agent-Handoff packages per-server runbooks
- [ ] **Observability integration** — Pipe health check data to Sentry, Grafana, or a custom dashboard
- [ ] **Rollback protocol** — If a delegated pipeline breaks production, auto-rollback to last known good ORCHESTRATOR.md

## Phase 4: Platform Play (Month 6+)

- [ ] **DispatchNow API** — Expose orchestration as a service other skills can call
- [ ] **Agent marketplace integration** — Auto-select the best agent for each subtask from a registry
- [ ] **Natural language pipeline builder** — "I want daily SEO audits, weekly content, and real-time error alerts" → full plan + delegation in one shot
- [ ] **Enterprise features** — Audit logs, approval workflows, cost allocation per team/project
- [ ] **Billing awareness** — Track API costs per subtask, optimize for budget constraints

## How to Contribute

### For DispatchNow
```bash
git clone https://github.com/stringztechnologies/dispatchnow
# Edit SKILL.md → add new patterns, fix edge cases
# Submit PR with before/after examples
```

### For Agent-Handoff
```bash
git clone https://github.com/stringztechnologies/openclaw-skills
# Edit skills/agent-handoff/SKILL.md
# Add platform templates in references/
```

### Testing Improvements
The best way to improve these skills is real usage:
1. Run a complex multi-step task with DispatchNow active
2. Note where planning was weak, prompts were unclear, or recovery failed
3. Update the skill with the fix
4. Hand off the result with Agent-Handoff, note gaps in the runbook
5. Feed back into the skill

---

*Each phase builds on the last. Start with Phase 1 — the foundation improvements pay for themselves in the first week of use.*
