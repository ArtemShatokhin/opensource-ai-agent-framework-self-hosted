# Open-source AI agent framework: frequently asked questions

Kortix is the open-source AI Operating System. These are the questions engineers and buyers ask when they choose an open-source AI agent framework to run agents on, answered from the [Kortix docs](https://kortix.com/docs) and each project's own repository.

## Is an open-source AI agent framework free?

The framework code itself is free to install. LangGraph and CrewAI are MIT-licensed libraries, so you can use them at no cost (GitHub, checked October 2026). What costs money is the runtime around them: the machines agents run on, the model tokens, and the engineers who operate it. Kortix is priced separately. Self-host is free, and Kortix Cloud has a Free plan that includes 200 credits each month and 1 project, and a Team seat at $40/seat/month that includes 2,500 pooled credits per seat ([Kortix pricing](https://kortix.com/pricing)).

## Can I self-host Kortix?

Yes. Run it on Kortix Cloud, in your VPC, or on your own on-prem network, and self-host is free. Self-hosting installs the full platform with the `kortix self-host` commands: `curl -fsSL https://kortix.com/install | bash`, then `kortix self-host init` and `kortix self-host start`. The instance is one Docker Compose stack on a server you control. Sessions still run on a cloud sandbox provider, so the server needs a provider key and must be reachable so the sandboxes can call back. The [Kortix self-hosting docs](https://kortix.com/docs/host) cover the requirements.

## Which models can the agents use?

Any model provider with your own keys. Or the ChatGPT plan you already pay for. Or sign in with your OpenCode Console account for OpenCode Zen and Go. You can also point Kortix at any OpenAI-compatible endpoint behind your own URL. Kortix charges nothing for a request that runs on your own key or subscription; your provider bills you. A session that names no model uses the platform default, or the project or account default you set. See the [Kortix models docs](https://kortix.com/docs/project/models).

## How is Kortix different from a framework like LangGraph or CrewAI?

LangGraph and CrewAI are libraries your engineers compose into an application. Kortix is the open-source platform that runs and governs the agents: one git repo holds the agents, skills, memory and connectors, each session gets its own isolated sandbox and branch, and a person approves each change request before it lands. The [framework vs platform guide](framework-vs-platform.md) has the full comparison.

## What licence is Kortix under?

Kortix is open source. You can read the code, fork it, and audit it in the public repository at [Kortix on GitHub](https://github.com/kortix-ai/suna). For teams that need to self-run it, self-hosting installs the full platform on your own infrastructure. Kortix keeps the code available so no vendor owns your company's configuration or its agents.

## Do I need a cloud account to run Kortix?

No. You can self-host the whole platform. A self-hosted instance still needs a sandbox provider key (Daytona, Platinum or E2B), because the platform runs on your server while each session runs on a cloud sandbox provider. The instance must be reachable from the internet so those sandboxes can call back. On Kortix Cloud you create a project and start a session with nothing to install.

## How do I keep my keys and my team's data under control?

Connector credentials are brokered server-side and never enter the machine. Secrets are encrypted at rest with a key per project, and each one has an audience under "Who can use it": everyone in the project, only you, or specific people, groups or agents. Connector calls follow policies you set to Allow, Ask or Block, down to the arguments of each call. Approval gates are off until you set them. See the [Kortix connectors docs](https://kortix.com/docs/connect/connectors).

## What can I connect?

3,000+ apps in a click, plus MCP, OpenAPI, Postman, GraphQL and raw HTTP. Agents can also start from Slack, Microsoft Teams, email, a cron schedule, or a signed webhook. Email is experimental. Connector credentials are stored by Kortix and spent server-side, and every call is recorded in the audit log.

## Does Kortix replace a framework, or work with one?

Both. Kortix runs and governs agents as company work, and it can run the code your team already writes. OpenCode is the harness, and a project can use any model provider with your own keys, so a LangGraph or CrewAI service can run inside a Kortix session on its own cloud computer with the platform handling isolation, credentials and review.
