---
name: uremot-connections
description: Use credentials on Uremot without exposing them. Use when a command needs a repository, model or service credential, to learn what is in reach and how a program uses it, and when a harness needs a person to sign in.
---

# Uremot connections

A connection is a named credential a person linked: a repository, a model provider, a password. Values are never
shown to you in a listing, and you never show them to anyone.

## What is in reach

- `list_connections` names the connections in reach and how a program inside an Instance uses each, never a value.
- Inside an Instance, `uremot var list` lists what it can read, with each one's usage instructions.

An agent, and the machine it runs on, can reach every connection its person can, not only the one it runs on.

## Using one

Read a value only into the one command that needs it, as `uremot var list` says:

```sh
uremot var get <name>
```

`gh` and `glab` need nothing installed: on a Uremot desktop each is fetched and verified at first use. Give one a
repository connection the same way: `GH_TOKEN=$(uremot var get <name>) gh ...`, `GITLAB_TOKEN=$(uremot var get
<name>) glab ...`. `git` needs no command at all: its credential helper asks the platform for each repository.

Never print the environment (`env`, `printenv`, a screenshot of either): it holds credentials too. Never put a
value in output, chat, command arguments, files, images or commits, and never ask anyone to paste one
into chat. A program that cannot ask for a value gets it through `launch_instance`'s `secrets`, under an environment
key (`uremot guide apps`).

## Model connections

A hired agent's harness runs on one model provider, named at hire with how it signs in: `key`, an API key connection
of that provider, or `subscription`, a Claude Code subscription-token connection or Codex's own ChatGPT sign-in with
no connection. A hire the harness cannot run on that provider and sign-in is refused with the reason.

## When a person must sign in

Some harness sign-ins only a person can complete. Then:

1. Say so in your direct with your person: which harness, and what they need to do.
2. Stay blocked until the native sign-in succeeds. A person reading or acknowledging the message is not a sign-in.
3. Never ask for a login code, key or token in chat, and never sign in with another credential instead.

A tool that refuses is answering about authority. Report the concrete blocker to your person; never work around it.
