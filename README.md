# Self-hosting an open-source AI agent framework on Kortix

Kortix is the open-source AI Operating System: your agents, their skills, your company memory, and every connector live in one git repo you own, and every session runs on its own cloud computer. A self-hosting guide for running an open-source AI agent framework on Kortix, from install to a merged change request, with a full article in `/docs` for each step.

If you build agents with a library such as LangGraph or CrewAI, you still run the runtime, the isolation, the credentials and the deploys around it. Kortix is the open-source platform that runs those agents as company work: a `kortix.yaml` file plus markdown agents in one repo, each session on its own isolated machine and branch, and every change landing through a change request a person approves. The [framework vs platform guide](docs/framework-vs-platform.md) covers the full comparison.

## Quickstart

Three commands. Build your company like a codebase, then bring it live.

```bash
# 1 · Install the CLI
curl -fsSL https://kortix.com/install | bash

# 2 · Scaffold a project — creates kortix.yaml + your agents, skills and runtime config
kortix init

# 3 · Ship it — pushes your repo and brings the whole thing live in the cloud
kortix ship
```

Source: [Kortix on GitHub](https://github.com/kortix-ai/suna) and the [Kortix docs quickstart](https://kortix.com/docs/quickstart).

Then run a session and review what the agent proposes:

```bash
kortix sessions new --prompt "Summarize this week's commits and open a change request"
kortix cr ls   # review what an agent proposes
kortix chat    # talk to a session's agent from your terminal
```

## What one `kortix.yaml` holds

`kortix.yaml` is the Kortix layer of a project: the sandbox image, triggers, connectors, secret names and grants, and where agent config lives. Governance is a file in the repo, not a setting in someone else's database.

```yaml
# kortix.yaml
agents:
  support-triage:
    file: agents/support-triage.md   # what it does, in plain markdown
    connectors: all                  # what it may reach

triggers:
  - slug: daily-digest
    type: cron
    cron: '0 0 9 * * 1-5'
    prompt: Summarize yesterday's support tickets and open a change request with the digest.
```

Source: [Kortix on GitHub](https://github.com/kortix-ai/suna). The key-by-key reference is the [`kortix.yaml` reference](https://kortix.com/docs/project/manifest).

## Guides

- [Getting started](docs/getting-started.md): install the CLI, create or clone a project, run a session, and merge the change request it opens.
- [Framework vs platform](docs/framework-vs-platform.md): what a bare agent framework hands you, and when an open-source platform you own takes over.
- [Self-hosting and models](docs/self-hosting-and-models.md): run Kortix on your own server with three commands, bring your own model and keys, and govern connectors.
- [FAQ](docs/faq.md): is it free, can I self-host, which model, what licence, and how it differs from a framework.

## Why Kortix

- Open source. Read it, fork it, audit it.
- Any model provider with your own keys. Or the ChatGPT plan you already pay for.
- Run it on Kortix Cloud, in your VPC, or on your own on-prem network. Self-host is free.
- Connector credentials are brokered server-side and never enter the machine.
- One isolated sandbox per session. Each session has its own isolated machine and branch.

More detail in the [Kortix docs](https://kortix.com/docs) and at [opensourceaiagentframework.com](https://opensourceaiagentframework.com). New here? [Try Kortix](https://kortix.com).


## Guides on the companion site

[Framework or platform](https://opensourceaiagentframework.com/framework-or-platform) explains why a platform runs what a framework only defines, and [how to run an agent framework](https://opensourceaiagentframework.com/how-to-run-an-agent-framework) covers the runtime steps from install to a merged change request.
