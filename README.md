# Give your agents hands, eyes and ears.

Uremot gives your AI agents real computers. An agent gets a desk, a full desktop with a screen, a keyboard and a
mouse, on a machine you choose. It works there the way a person would, and you can watch it live in your browser.
[uremot.com](https://uremot.com) · [Docs](https://uremot.com/docs)

This repository connects your agents to Uremot: a plugin that adds Uremot's hosted MCP server, and eight skills that
teach them how to use it.

## Install

**Claude Code**

```sh
claude plugin marketplace add uremot/skills
claude plugin install uremot@uremot
```

**Codex**

```sh
codex plugin marketplace add uremot/skills
codex plugin add uremot@uremot
```

**Gemini CLI:** `gemini extensions install https://github.com/uremot/skills`

**OpenCode** (2 or later), the skills through skills.sh and the MCP server by its own command:

```sh
npx skills add uremot/skills -a opencode -g
opencode mcp add uremot --url https://mcp.uremot.com/mcp --global
```

**Any other agent, through skills.sh:** `npx skills add uremot/skills`

Then sign in to Uremot when your agents ask; in Claude Code, `/mcp`.

## Next steps

- **New to Uremot?** [Set up with your AI](https://uremot.com/docs/set-up-with-your-ai): paste one text into your
  harness, and it sets everything up with you.
- [Getting started](https://uremot.com/docs/getting-started), to do each step yourself.
- [Install Uremot skills](https://uremot.com/docs/skills#install-them), for setup and sign-in steps for your harness.
- [Connect a harness](https://uremot.com/docs/connect-a-harness), to add the MCP server by hand.

## What's inside

- **The MCP server**, `https://mcp.uremot.com/mcp`: launch desktops and services, see and use their screens, hire
  more agents and talk to them in chat.
- **Eight skills:**

| Skill | What it's for |
| --- | --- |
| `uremot-agents` | Hire and manage agents on Uremot. |
| `uremot-apps` | Find, launch and publish Uremot Apps. |
| `uremot-chat` | Work through Uremot chat. |
| `uremot-computes` | Set up, check and repair a Uremot Compute, the machine Instances run on. |
| `uremot-connections` | Use connections and variables on Uremot without exposing them. |
| `uremot-onboard` | Turn a repository into a Uremot App, or rebuild one. |
| `uremot-setup` | Set up Uremot from nothing to a first running Instance, or find out why it doesn't work. |
| `uremot-state` | Read what Uremot says about agents and Instances, and recover. |

## About this repository

Version 1.3.2, generated from Uremot's own repository at each release: edit nothing here, as the next release
replaces it. For a question or a problem, write to us at [uremot.com/contact](https://uremot.com/contact).

## License

Apache-2.0: see [LICENSE](LICENSE).
