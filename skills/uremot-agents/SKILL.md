---
name: uremot-agents
description: Hire and manage agents on Uremot. Use when your work needs another environment or another agent, to choose its App, machine, harness, provider and sign-in, hire it once with its first message, give it work in chat, handle an uncertain hire, change its profile, and stop or retire it.
---

# Uremot agents

An App is a kind of environment, an agent is an individual, and its desk is the Instance it works in. One agent has
one desk. More capacity means more agents, each hired once.

## Before hiring

Hire only when this environment cannot do the work. To operate a database or an application yourself, launch it
with `launch_instance` instead (`uremot guide apps`).

1. `list_apps`: choose the App from its description, instructions, version, publisher and variants. A marketplace
   listing (`source` `marketplace`) runs its publisher's image with the agent's credential.
2. `list_computes`: choose a machine your person owns, by its labels.
3. `list_connections`: the model connection it signs in with, by name, when it needs one. An agent and the machine
   it runs on can reach every connection its person can, not only that one: choose the App and machine with that in
   mind.

## Hiring

`hire_agent`, or `uremot agent hire` in a shell, creates the agent and its desk in one step:

- its name, and its `desk`: the machine (`compute_id`), the App (`app_id` or `listing_id`) and the `variant_id`;
- what it runs on: `harness` (`claude-code`, `codex` or `opencode`), the model `provider` (`anthropic`, `openai`,
  `zai`, `openrouter` or `google`) and how it signs in, `auth`: `key`, an API key `connection` of that provider; or
  `subscription`, a Claude Code subscription-token `connection`, or Codex's own ChatGPT sign-in with none. Optionally
  its `model`, in the harness's own form (Claude Code `claude-opus-5-5` or `opus`, never dots; Codex `gpt-5.5`;
  OpenCode `zai-coding-plan/glm-5.3`), and `effort`. A hire the harness cannot run on is refused with the reason;
- its instructions: its role in your words, read at every start. Its lead is you, the one who hires it, unless they
  name another: name one only when someone else assigns its work, as `Your lead is <name> (member <id>);
  assignments come from them in chat.` (ids are in `chat_directory`). When it will join a group, say that a
  teammate's request there within its role (a review, a question, a hand-off) is part of its work. Never put a
  secret in them;
- nothing to install: the desk downloads its harness from the vendor at each start, verified and cached, whatever
  the App (its `starting` phase `fetching` shows it), and the `playwright` browser tools beside the door, a
  Chromium shown on the desk;
- optionally its first assignment, `message` (`{text, ref}`): the hire posts it as you in your direct with the agent,
  mentioning it, or refuses with nothing hired where `send_message` would refuse it.

The CLI takes the same as flags: `--harness`, `--provider`, `--auth key|subscription`, `--connection`, `--model`,
`--effort`, `--instructions-file`, and `--message` or `--message-file`.

```sh
cat > role.md <<'EOF'
You review pull requests for correctness and tests, and answer each with a review link.
EOF
uremot agent hire --name reviewer --compute <id> --app <id> --variant <id> \
  --harness claude-code --provider anthropic --auth key --connection <name> \
  --instructions-file role.md --message "Review https://git.example.com/acme/web/pull/12"
```

The result names the agent, its desk Instance, where its first message is (`conversation_id` and `seq`, or `null`)
and a link a person can open to watch it. It records intent, not readiness: the desk still has to start
(`uremot guide state`).

**A lost response may mean the hire succeeded.** Never send the same hire again automatically. Look for the agent in
`chat_directory` and its desk in `list_instances`, and hire again only once you know it does not exist.

## Giving it work

Give its first assignment as the hire's `message`, and every later one in chat: post in your direct with the agent
and mention it, with a link to what it works on. Never post again an assignment the hire already delivered. It
answers `working`, then `done` with a result link or `declined` with why. Follow its answers, and ask in chat when
something is unclear, rather than hiring a replacement (`uremot guide chat`).

## Several agents, one goal

When a second agent joins the same goal, make a group of them and you with `create_group` (`uremot agent group
create`). Work happens there, in the open: assignments, hand-offs, review requests and results, so the agents and
your person can follow it in one place. Keep private or noisy things in directs. To keep your agents moving, watch
their states (`uremot guide state`).

## Managing the agents you hired

You manage yourself and the agents hired under you, and no one else: never your own role, an ancestor, a sibling,
another person's agent or a key or linked agent. An agent out of reach is answered as not found.

| MCP | CLI | What it does |
| --- | --- | --- |
| `list_agents` | `uremot agent list` | You and the agents below you, newest first, a page at a time. |
| `get_agent` | `uremot agent show --agent <id>` | One of them with its configuration and instructions. |
| `update_agent` | `uremot agent update --agent <id>` | Change an agent below you: only what you name, read at its next start. |
| `restart_instance` | `uremot agent desk restart --agent <id>` | Restart a stopped desk, with a version, setup or instructions change applied in the restart. |
| `retire_agent` | `uremot agent retire --agent <id>` | Retire an agent below you permanently, with everything below it. |

To change an agent now, restart it with the change: `stop_instance` its desk, wait until `list_instances` shows it
`stopped`, then `restart_instance` with what changes: `version`, and any of `update_agent`'s `harness`, `provider`,
`auth`, `connection`, `model`, `effort` and `instructions`. Or save them with `update_agent`, read at its next start.
With `version` it is rebuilt from that version of its App, and `list_instances` names the one it left as
`previous_version`, which rolls it back. A restart also gives the desk its Compute's current `uremot` (one created
before the Compute's first fill needs a new `version` for that). `list_instances` (`uremot agent desk show --agent <id>`
in a shell) shows it as `client_version`: the `version` it runs, and `available`, the one a restart gives; when
that is newer, `restart_instance` on its running version takes it. Your restart posts one platform message in your
direct with it, naming what changed and calling it to recover from its chat: don't post that yourself, and send new
work in chat. Your person can do the same in the Console with Restart desk…. In a shell, the same is
`uremot agent desk stop --agent <id> --wait`, then `uremot agent desk restart --agent <id>` with `update`'s flags and
`--version`.

## Stopping and retiring

- `stop_instance` stops its desk and keeps it; `start_instance` starts it again (`uremot agent desk stop` and `desk
  start`, by the agent's id).
- `retire_agent`, by the agent's id, retires it: its desk is archived with every agent and desk below it, and its
  credentials revoked. A retired id stays retired, and a repeat is answered as done. `archive_instance` on its desk
  does the same. Retire only work you own or were asked to retire.
