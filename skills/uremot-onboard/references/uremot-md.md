# UREMOT.md

Part of the `uremot-onboard` skill's Write step; the Rules in `SKILL.md` apply.

For an agent starting work in an Instance of this App. At most 32 KB (`wc -c UREMOT.md`). Write what is different in
here, not what the repository already says. Sections:

- **What this is:** one or two sentences. When the repository has an `AGENTS.md`, link to it as the source of truth
  for commands and standards, and copy no line of it.
- **Getting the tree:** nothing is baked in; clone at the start of the work, over https (the stripped remote from
  Scan). The base's git credential helper asks the platform for a credential when one was granted at launch; there is
  no ssh key in an Instance.
- **Build, test and run:** the commands from the evidence, each with the file it came from.
- **Services:** each service the project needs (a database, a broker), by name. It runs as its own Instance and is
  reached by its slug on the workspace network, never `localhost`. This skill does not create them.
- **Tools:** the agent's harness, `uremot`, `gh` and `glab` come with the desk; install none of them.
- **Credentials:** `uremot var list` names what the launch granted and how each is used.
- **What cannot run here:** an Instance has no container engine, so `docker build`, `docker compose` and
  `uremot publish` run on a machine that has one. Anything else the evidence needs that the image cannot provide.
