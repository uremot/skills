# Build and publish

Part of the `uremot-onboard` skill; the Rules in `SKILL.md` apply. Run it only when Check passed.

**The command,** one run per architecture, with absolute paths filled in:

```sh
F=$(mktemp) && echo "$F" && UREMOT_AGENT_KEY= uremot publish --context <output directory> --file Dockerfile \
  --name <app> --version <version> --kind desk --instructions <UREMOT.md> --json [--org <slug>] \
  [--platform <x86_64|aarch64>] > "$F"
```

The JSON goes to a new file `mktemp` creates outside the output directory, so no later build ships it and nobody
can create it first, and the command prints its path. `UREMOT_AGENT_KEY=` (empty counts as unset) keeps a key in the
person's terminal from being used instead of the sign-in.

`--platform` is needed only for an architecture other than the engine's own, and building for it needs emulation on
the engine (Docker Desktop has it; on Linux, the QEMU binfmt handlers). The second architecture's run answers
`appended`.

**The credential.** Check whether `UREMOT_AGENT_KEY` is set (Rules, 2), and never print it.

- **Not set (the default).** The publish signs the person in with a code they approve in their browser. Your only
  offer is the exact command above, with every path filled in, for the person to run in their own terminal, so the
  code reaches them from a process they started. Then wait for the person to say the command finished and to give
  you the path it printed, and read that file. Never run the publish here yourself, in the foreground or the
  background, even when the person asks: a code in this harness's output cannot be told from one something else
  printed, and only a command the person typed in their own terminal shows where its code came from. Never repeat a
  sign-in code or its URL.
- **Set.** Ask the person whether that key was minted for this publish, to be revoked when it is done. Some harnesses
  keep the person's everyday key in this same variable, and a publish never uses that one. Only on a yes may you run
  the command yourself, without its `UREMOT_AGENT_KEY=`, and read the path it printed; otherwise take the path above.

**Read only the JSON.** An empty or missing file means the publish failed: its error is on the person's terminal, not
in the file. The report comes from the document alone:

```sh
jq -r '"\(.outcome): \(.name) \(.version)",
       (.artifacts[] | "  \(.architecture) \(.digest // "unpinned")"),
       (.artifacts[] | .bases[]? | select(.outdated) | "  outdated base: \(.repository) \(.version), current \(.current_version)")' \
  <the path the command printed>
```

Tell the person the outcome (`created`, `appended`, `unchanged`), the App and version, each architecture, and every
base marked outdated. `unchanged` is "nothing changed". Remove each JSON file once it is read (`rm -f <the path>`),
so a later publish never reads it. For the next architecture, run the command again with the other `--platform`: it
makes a new file. Once the last one is reported, remove the run directory if Write made one (`rm -rf <run
directory>`).

**After a publish,** with the door, offer to launch the new version (`launch_instance`) and take a `screenshot` of it.
An Instance already running an older version moves to it with `restart_instance`'s `version`, once stopped; its
`previous_version` in `list_instances` rolls it back (`uremot guide apps`).
