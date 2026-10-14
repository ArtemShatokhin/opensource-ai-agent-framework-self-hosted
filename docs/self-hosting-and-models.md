# Self-hosting the open-source Kortix AI Operating System

Kortix is the open-source AI Operating System, and self-hosting runs the full platform on a server you control. Kortix is open source, so the platform you self-host is code you can read, fork and audit. It installs with three commands, runs any model with your own keys, and governs every connector call. Run it on Kortix Cloud, in your VPC, or on your own on-prem network, and self-host is free.

## Self-host with three commands

A self-hosted instance is the full Kortix platform on a server you control: one Docker Compose stack that the `kortix self-host` CLI installs, configures and updates. The platform runs on your server, and each session runs on a cloud sandbox provider. The instance must be reachable from the internet so those sandboxes can call back. The [Kortix self-hosting docs](https://kortix.com/docs/host) cover the whole operator path.

```bash
# 1 · Install the CLI
curl -fsSL https://kortix.com/install | bash

# 2 · Initialize the instance, with your domain and admin email
kortix self-host init --domain kortix.example.com --admin-email you@example.com

# 3 · Start the stack
kortix self-host start
```

The requirements are a Linux server with a public IP, `A` or `AAAA` records for your domain and its `api` subdomain plus a wildcard for sandbox previews, ports 80 and 443 open inbound, a sandbox provider key (Daytona, Platinum or E2B), and a model provider key you add in the web app after the first sign-in. A self-hosted instance has no managed models, so connect your own providers.

Sign up as the admin at `https://kortix.example.com` as soon as the stack starts. Until you set `EMAIL_URL` or `DISABLE_SIGNUP=true`, anyone who reaches the instance can create a confirmed account for any address, the admin address included. Then run `kortix self-host doctor` and fix every failing check.

## Bring your own model and keys

Kortix is model-agnostic. Pick the model per agent, per session or per message, and switch the day a better one lands. Every model request goes through the LLM gateway, which finds the credentials, applies the project's routing, access rules and budgets, and records the cost.

```bash
kortix providers set anthropic       # reads the key from stdin
kortix providers set openai sk-...   # key as an argument
kortix providers ls                  # what is connected
kortix providers rm anthropic        # remove the key and any login
kortix models default <model-id>     # set the project default
```

Three providers take a device-code login instead of a key:

```bash
kortix providers login openai       # ChatGPT Plus or Pro
kortix providers login opencode     # OpenCode Zen
kortix providers login opencode-go  # OpenCode Go
```

Any model provider with your own keys. Or the ChatGPT plan you already pay for. Or sign in with your OpenCode Console account for OpenCode Zen and Go. Kortix charges nothing for a request that runs on your own key or subscription; your provider bills you. For a self-hosted or unlisted endpoint, the Custom tab connects any OpenAI-compatible base URL. The [Kortix models docs](https://kortix.com/docs/project/models) list every provider and its secret name.

## Connector governance

A connector gives a project governed access to one external app, MCP server or API. Kortix stores the credential, picks the account, applies the project's policies, calls the provider and writes an audit event for every call. Connector credentials are brokered server-side and never enter the machine.

Set a policy per action in `kortix.yaml`: `always_run`, `require_approval` or `block`, shown in the product as Allow, Ask and Block. An Ask holds the call until a person approves it, then the agent resumes. Approval gates are off until you set them.

```yaml
# kortix.yaml
connectors:
  - slug: billing-api
    name: Billing API
    provider: openapi
    spec: https://api.example.com/openapi.json
    policies:
      - match: "get_*"
        action: always_run
      - match: "*"
        action: require_approval
```

The [Kortix connectors docs](https://kortix.com/docs/connect/connectors) cover providers, accounts and the full policy schema.

## Secrets and who can use them

A secret is an encrypted, per-project credential: an API key, a token, or a connection string that a session needs and that must not live in the repository. Kortix encrypts every value with a key per project, and no API, CLI command or page returns a stored value.

```bash
kortix secrets set STRIPE_API_KEY=sk_live_...   # everyone in the project
kortix secrets set PAYROLL_TOKEN=... --only-me  # only you
```

Environment is the default exposure. Egress-enforced is experimental: the sandbox holds a handle, and Kortix substitutes the real value outside the sandbox only on approved HTTPS hosts, so an echoed value comes back as `[REDACTED]`. Each secret also has an audience under "Who can use it": everyone in the project, only you, or specific people, groups or agents. A session receives a secret only when the person who started it, and the agent's grant, both allow it. The [Kortix secrets docs](https://kortix.com/docs/project/secrets) have the details.

Next: [get started](getting-started.md) with a project and a session, or read [what a framework leaves to you](framework-vs-platform.md).
