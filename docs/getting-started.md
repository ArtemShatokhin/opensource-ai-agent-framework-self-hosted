# Getting started with an open-source AI agent framework on Kortix

Kortix is the open-source AI Operating System. The path runs from a fresh terminal to a merged change request: install the CLI, create or clone a project, run a session, and merge the change request the agent opens. Every step is a git operation on your project's repository.

Kortix is open source, so the code that runs your agents is yours to read, fork and audit. The commands come from the [Kortix docs quickstart](https://kortix.com/docs/quickstart).

## Install the CLI

```bash
curl -fsSL https://kortix.com/install | bash
```

The script supports macOS and Linux on x86_64 and arm64. On Windows, run it inside WSL. It installs two commands: `kortix` and `kortixt` (the TUI). To pin a version or use the dev channel, see the CLI install page in the [Kortix docs](https://kortix.com/docs).

## Sign in

```bash
kortix login
```

Your browser opens and you approve the CLI there; the token comes back on a local callback. If you belong to several accounts, the CLI asks which one, and `--account <slug>` skips the question. On a machine without a browser, sign in with a token: `kortix login --token kortix_pat_…`.

## Create or clone a project

A project is a git repository with a `kortix.yaml` at its root. A new account has no projects, so create one:

```bash
kortix init my-agent
cd my-agent
kortix ship
```

`kortix init` creates the `my-agent` directory from the starter, runs `git init -b main`, and asks which local coding tools to wire up. `kortix ship` validates `kortix.yaml`, commits, asks for missing secrets, pushes the current branch, and asks you to connect declared connectors. With no origin it creates a Kortix-managed repository; with a GitHub origin it links that repository.

To work on a project a teammate made, clone it through Kortix:

```bash
kortix projects clone <project_id> my-agent
cd my-agent
```

## Run a session

A session is one unit of agent work. It runs in its own sandbox, on its own branch, so the agent can install, run and break anything and only what it commits survives.

```bash
kortix sessions new --prompt "Add a skill that drafts the weekly status report"
```

The session runs the project's default agent. Pass `--agent <name>` or `--model <id>` to override the defaults.

| Task | Command |
| --- | --- |
| Talk to the agent | `kortix chat` (picks your most recent running session) |
| Send one prompt | `kortix chat <session_id> --prompt "…"` |
| Attach the OpenCode TUI | `kortix connect <session_id>` |
| List sessions | `kortix sessions ls` |

## Review and merge the change request

The agent commits, pushes the session branch, and opens a change request against the project's default branch (`main` in a new project). The managed `kortix-system` skill that Kortix adds to every session tells the agent to open it and stop.

```bash
kortix cr ls      # open change requests
kortix cr diff 1  # the unified diff
kortix cr merge 1 # merge into the base branch
git pull          # bring the merged work into your clone
```

An agent merges a change request only when its `kortix_permissions` include `project.gitops.merge`. The starter `kortix` agent has `kortix_permissions: all`, but the managed skill still tells it to stop after it opens one, so a person merges. To require a person, give the agent a `kortix_permissions` list without `project.gitops.merge`. To send the work back instead, run `kortix cr request-changes 1 --message "…"`.

## Check your setup

```bash
kortix doctor
```

The command checks your login and the project, starts a throwaway session, waits up to 180 seconds for its sandbox to start, sends a ping, waits for the agent's reply, and deletes the session. It exits `0` when every check passes and `1` when one fails.

## The git operation behind each step

| Step | Command | What happens in git |
| --- | --- | --- |
| Create a project | `kortix init`, `kortix ship` | `git init`, commit, push to the project's repository |
| Get a project | `kortix projects clone` | `git clone` through Kortix |
| Run a session | `kortix sessions new` | The agent works on its own branch, named `<session_id>` |
| Review and merge | `kortix cr merge` | The session branch merges into the default branch |

Next: run it on your own server with the [self-hosting and models guide](self-hosting-and-models.md), or read [what a framework leaves to you](framework-vs-platform.md).
