# Framework vs platform: when an open-source AI agent framework stops being enough

Kortix is the open-source AI Operating System. An open-source AI agent framework is a library your engineers import to build agents: it defines how an agent reasons, calls tools and hands work to another agent. Kortix is the open-source platform that runs and governs those agents as company work. A framework hands you the agent's reasoning loop; a platform you own adds the runtime, the isolation and the review around it.

Kortix is open source, so the platform that runs your agents is code you can read, fork and audit.

## What an open-source AI agent framework is

A framework gives your team the primitives to build one agent or many. It is code you install, import and compose into an application. Two widely used examples are LangGraph and CrewAI, and both are MIT-licensed libraries you run inside your own Python service.

[LangGraph](https://github.com/langchain-ai/langgraph) is a low-level orchestration framework for building, managing and deploying long-running, stateful agents. It provides durable execution, human-in-the-loop control, short and long-term memory, and debugging through LangSmith. [CrewAI](https://github.com/crewAIInc/crewAI) is a high-level Python framework for multi-agent workflows, with role-based Crews for autonomous collaboration and event-driven Flows for precise control.

Both projects are active. LangGraph's latest release was `cli==0.4.34` on 13 October 2026 with 43,000+ stars, and CrewAI's was `1.15.27` on 9 October 2026 with 59,000+ stars (GitHub, checked October 2026).

## What a framework leaves to you

A framework hands you the agent's reasoning loop. You still build and operate the runtime around it: the machine each agent runs on, how one agent is isolated from another, which credentials it may spend, which tools each call may touch, and how its output reaches production.

That is the work a platform exists to do. When agents are a feature inside one application, your engineers can own it. When agents do work across teams, the runtime becomes the part that needs a product.

## What an open-source platform you own adds

Kortix runs those agents as company work: your agents, their skills, your company memory, and every connector live in one git repo you own, and every session runs on its own cloud computer. The platform adds what a library leaves out.

- Configuration in one repo. Agents, skills, memory and connector config are text in a git repo. A `kortix.yaml` file declares the machine image, the connectors and the triggers.
- Isolation per session. One isolated sandbox per session. Each session has its own isolated machine and branch, and thousands of agents in parallel on one config.
- Governance. Per-resource permissions for people and agents, connector policies set to Allow, Ask or Block down to the arguments of each call, approval gates you set, and an audit trail.
- Reach. Cron schedules and signed webhooks start sessions with nobody present, and Slack, Microsoft Teams and email start them from where people already are.
- Review before landing. Session work reaches `main` through a change request. Merge is default-deny for agents, so a person approves each one.

The [Kortix docs](https://kortix.com/docs) describe each part; the [`kortix.yaml` reference](https://kortix.com/docs/project/manifest) is the key-by-key source for the config.

## The comparison

| Dimension | Kortix | LangGraph | CrewAI |
| --- | --- | --- | --- |
| What it is | Open-source platform that runs and governs agents | Library for stateful agent orchestration | Python framework for multi-agent workflows |
| Open source | Yes | Yes (MIT) | Yes (MIT) |
| Self-hosting | Kortix Cloud, your VPC, or on-prem; free | Runs inside your own service | Runs inside your own service |
| Where agents run | One isolated sandbox per session | Wherever your application runs | Wherever your application runs |
| Configuration lives in | One git repo you own | Your application's code | Your project's code and JSONC |
| Governance | Per-tool Allow, Ask, Block; approval gates | Human-in-the-loop in your code | Roles and processes you build |
| Cost to run | Self-host free; Free plan and Team seat | Library free; deployment is paid | Library free; AMP suite is commercial |

Sources for the table: the [Kortix docs](https://kortix.com/docs), the [LangGraph repository](https://github.com/langchain-ai/langgraph) and the [CrewAI repository](https://github.com/crewAIInc/crewAI), each read in October 2026. Kortix pricing is on the [Kortix pricing page](https://kortix.com/pricing).

## When a framework is enough

Use LangGraph or CrewAI when agents are a feature inside a product your engineers already ship, and the same team owns the runtime, the deployment and the model cost. A library keeps you close to the code and gives you complete control of the agent loop. If the work runs inside one application and stops at your API boundary, you do not need a platform.

## When the platform takes over

Use Kortix when the agents are the work: several agents in parallel, each on a real cloud computer with your repo checked out, reaching the tools your company runs on, and landing changes a person reads first. The repo is the company, agents and skills are markdown, and the whole configuration is versioned and diffable. It is the layer LangGraph and CrewAI leave to the application.

OpenCode is the harness, and a project runs any model provider with your own keys. Your team can keep writing LangGraph or CrewAI code and run it inside a Kortix session, on its own sandbox and branch, with the platform handling isolation, credentials and review.
