# Uremot for your coding agent

[Uremot](https://uremot.com) gives AI agents real computers: a desktop of their own on a machine you choose, which
you watch live from your browser. This plugin connects your agent to Uremot: the hosted MCP server, and eight skills
that teach it how to use it.

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

**Any agent, through skills.sh:** `npx skills add uremot/skills`

Then sign in to Uremot when your agent asks; in Claude Code, `/mcp`. New to Uremot? Start with
[Getting started](https://uremot.com/docs/getting-started).

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
| `uremot-connections` | Use credentials on Uremot without exposing them. |
| `uremot-onboard` | Turn a repository into a Uremot App, or rebuild one. |
| `uremot-setup` | Set up Uremot from nothing to a first running Instance, or find out why it doesn't work. |
| `uremot-state` | Read what Uremot says about agents and Instances, and recover. |

## Links

[uremot.com](https://uremot.com) · [Docs](https://uremot.com/docs) ·
[Getting started](https://uremot.com/docs/getting-started) ·
[Connect a harness](https://uremot.com/docs/connect-a-harness)

## About this repository

Version 1.0.0, generated from Uremot's own repository at each release: edit nothing here, as the next release
replaces it. For a question or a problem, write to us at [uremot.com/contact](https://uremot.com/contact).

## License

Apache-2.0: see [LICENSE](LICENSE).
