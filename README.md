# AgentOS — a control plane for an AI agent team

AgentOS is a native macOS app (Swift 6, SwiftUI, macOS 14+) that I use to run a team of
autonomous coding/research agents (Claude, Codex, Devin and others). It is the operator's
console — not another chat wrapper. Agents do the work; AgentOS gives a human one place to
see, decide and stop.

> This repository is the public overview/spec. The working code lives in a private
> monorepo because it is wired to client projects.

## What exists today

- **Agent directory + live status** — every agent, its role, the terminal/session it runs in,
  and whether that session is alive.
- **"Needs you" inbox** — every decision an agent cannot make alone (login, approval, stop,
  conflict) lands in one queue; nothing proceeds without the operator.
- **Task and run tracking** — cards, runs, results and evidence per task; a portfolio view
  across projects.
- **Read-only by design** — the app only reads runtime state (registries, event logs,
  launchd, terminal lists). It writes only decision files into a dedicated folder; an
  external lead process applies them and reports back the result.
- **Safe process execution** — subprocesses run through a single runner: argv only (no
  shell), timeouts, capped output; atomic, no-replace file writes that refuse symlinks.

## Architecture direction (next milestones, built with a Devin Bot)

1. **Isolated agent runtimes** — each agent in its own sandbox with scoped permissions,
   short-lived credentials, resource limits and restricted network.
2. **Zero-trust inter-agent auth** — per-agent identity, credentials issued by a central
   broker, no shared secrets between runtimes.
3. **Kill switch** — revoke credentials, cut network, pause the task queue, terminate the
   runtime, preserve execution logs.
4. **Recovery** — quarantine, review activity, human approval before restart.

Principle: agents operate autonomously, but never outside the boundaries the operator defines.

## Day-1 brief for Devin

> Build AgentOS, a native macOS application using Swift and SwiftUI. Start by inspecting the
> existing project, then design a modular architecture for a centralized AI agent control
> plane: lifecycle monitoring, task orchestration and execution tracking, isolated runtimes
> with scoped permissions, zero-trust inter-agent authentication, and an emergency kill
> switch with credential revocation and audit logs. First deliver an architecture spec,
> threat model, component breakdown and roadmap. Do not fake completed functionality.
> Validate each decision before implementation; ship in verifiable milestones.

— Nam (Pham Duc Phuong Nam), Vietnam
