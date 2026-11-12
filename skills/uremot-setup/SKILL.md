---
name: uremot-setup
description: Set up Uremot from nothing to a first running Instance, or find out why it doesn't work. Use when a person asks to set up or start using Uremot, connect a harness to it, or install its client and skills, and when a command, a sign-in, a machine or a launch fails.
---

# Uremot setup

Go in order and skip what already works: each step says how to check it. The goal is a first agent at work on the
person's machine. Sign-ins and keys are the person's: they approve them in their own browser or terminal, and you
never ask for one in chat or put one in a command.

## 1. What the machine needs

Say this before anything is installed. On Linux, a Compute runs its desks in a container engine: `docker info` must
answer (Docker Engine, https://docs.docker.com/engine/install/). On macOS or Windows, the machine joins with the
Uremot desktop app (https://console.uremot.com/download) and runs desks with Docker Desktop.

## 2. An account

The person signs in at https://console.uremot.com with GitHub or Google. The first sign-in makes their organization
and a Default workspace, and asks them to accept the terms. `uremot compute enroll --sign-in` signs in from a terminal
and creates the account too, but answers `terms_not_accepted` until the terms are accepted in the Console, so send
the person there first.

## 3. The `uremot` client

Check: `uremot --version`. If it is missing, install it, yourself if your harness allows, else give the person the
line for their terminal (on Windows, Git Bash):

```sh
curl -fsSL https://get.uremot.com | sh
```

It installs to `~/.uremot/bin` (`/usr/local/bin` as root) and says when that directory is not on `PATH`: use
`~/.uremot/bin/uremot` until it is. An old client updates with `uremot update`.

## 4. The harness

**A hired agent** already has its door: its desk registers it, with the desk's own key, at each start of the agent.
Check with `claude mcp list` (or `codex mcp list`); a missing door there is a desk to restart, not one to add.

**A harness started by hand on a desk** gets no Uremot door and no Uremot tools: the desk's door is its hired
agent's. Nothing here adds one.

**On the person's own machine,** connect the hosted MCP server, `https://mcp.uremot.com/mcp`, for the person's user,
not one folder. Run the Add command yourself: none of them holds a secret. If your harness refuses it, give it to the
person. The sign-in is the person's: give them that command for their own terminal.

| Harness | Add | Sign in |
| --- | --- | --- |
| Claude Code | `claude mcp add --scope user --transport http uremot https://mcp.uremot.com/mcp` | `claude mcp login uremot` |
| Codex | `codex mcp add uremot --url https://mcp.uremot.com/mcp` | `codex mcp login uremot` |
| OpenCode (2 or later: `npm i -g @opencode/cli`; 1.x can't sign in) | `opencode mcp add uremot --url https://mcp.uremot.com/mcp --global` | `opencode mcp auth uremot` |

The sign-in opens the Console, where the person chooses the organization and workspace and allows the harness. Then
install the skills. It's safe to repeat, on a desk too: it writes where the harness reads, puts back any platform
skill that is missing, and leaves every other skill alone.

```sh
uremot skills install --harness claude-code    # or codex, opencode
```

Tools and skills load when a session starts. Ask the person to start a new session and tell you to continue the
Uremot setup. Check, in that session: `list_computes` answers.

## 5. A machine

A Compute runs the Instances: the desktop app on a machine the person sits at, or `uremot` on a Linux server, where
`uremot compute enroll --sign-in` adds it and `uremot compute run` (or the `--daemon` install's service) keeps it
online. `uremot guide computes` sets one up and checks it. Check: `list_computes` shows it `online`, with `containers`
`available`.

## 6. A first agent

Hire one with the person (`uremot guide agents`): `list_apps` for UremotOS, `list_computes` for the machine, and
`list_connections` for the model account it signs in with; with none, the person links one in the Console's
**Connections**. Then `hire_agent` with a first message that gives it a task the person chose. Give the person the
watch link; the agent reports back in their chat. To see a desk without an agent, `launch_instance` with UremotOS and
`screenshot`; `uremot guide onboarding` makes an App from a repository.

## When something fails

| Symptom | Look | Fix |
| --- | --- | --- |
| `uremot: command not found` | `ls ~/.uremot/bin` | add it to `PATH` as the installer said, or install it (step 3) |
| The harness has no Uremot tools | `claude mcp list` or `codex mcp list` | a hired agent: restart its desk; on the person's own machine: add the server (step 4), then start a new session; a harness started by hand on a desk has none, by design |
| `401`, or the tools stopped answering | — | the person signs in again (step 4); a revoked Console key needs a new one |
| `terms_not_accepted` | — | the person opens the Console and accepts the terms (step 2), then the command runs again |
| A machine offline, without a container engine, or with a broken identity | `list_computes`; on the machine itself, never inside an Instance, `uremot compute status` | `uremot guide computes` |
| `network.status` `unsupported`, or a command named here is missing | `uremot --version` | `uremot update` |
| `429 quota_exceeded` | its message names the window | wait for that window to pass; never retry in a loop |
| `409 plan_limit` | its message names the limit | archive what isn't needed; for more, the person writes to support@uremot.com |
| `409 try_again` | — | send the same request again: the platform gave up having written nothing |
| An Instance in `error` | `list_instances`: `error_reason`, `error_detail` | fix what it names, then `start_instance` (`uremot guide state`) |
| An agent `blocked` or not starting | `chat_directory`: its `state` | `uremot guide state` |

When a step needs something only the person can do, say so in one message: what to run or open, and why. Then wait
for them to say it's done, and check again.
