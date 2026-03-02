# DispatchNow

AI agent orchestration + operational delegation in one skill.

**Part 1:** Rules for how agents should plan, delegate, and evaluate multi-step work.
**Part 2:** [Dispatch Protocol](https://github.com/stringztechnologies/dispatch-protocol) CLI integration — discover infrastructure, plan ownership, deploy configs, compile runbooks.

## Install

```bash
openclaw skills add stringztechnologies/dispatchnow
```

## What It Does

- Teaches agents structured planning before delegation
- Minimal-context prompting (each sub-agent gets only what it needs)
- Error recovery with diagnose → reframe → retry → escalate
- Quality evaluation with confidence ratings
- Infrastructure discovery across systemd, n8n, and OpenClaw
- One-owner-per-task conflict resolution
- Compiled ORCHESTRATOR.md runbooks for zero-context autonomous agents

## Requires

- [dispatch-protocol](https://github.com/stringztechnologies/dispatch-protocol) for Part 2 CLI commands

## Author

[Stringz Technologies](https://github.com/stringztechnologies)
