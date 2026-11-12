# Rebuild

Part of the `uremot-onboard` skill; the Rules in `SKILL.md` apply.

1. Run Check. Read the App and version from the Dockerfile's second line.
2. Run Scan again and compare the findings with what the Dockerfile and `UREMOT.md` say: a new toolchain or version,
   a changed command, a service added or gone.
3. When the current `uremot-os` version (Check, 3) is newer than the one in `FROM`, move `FROM` to it.
4. A Dockerfile an older version of this skill wrote names `harnesses=` on its second line and installs a harness
   or its launcher. Remove that line's `harnesses=` part, each harness's block (the comment above it says which),
   the files it copied (`claude-first-run.sh`, `managed-settings.json`, `claude.json`) and their `!` lines in
   `Dockerfile.dockerignore`, and the harness entry in `UREMOT.md`: the desk now brings its own. That is a change.
5. **Something changed:** bump the version's patch number (`1.0.3` to `1.0.4`), update the second line, show the
   diff of every file, and write after a yes. **Nothing changed:** say so, keep the version, and publish it again
   only if the person wants to confirm.
6. Run Build and publish. A publish whose JSON says `unchanged` is reported as "nothing changed". When the JSON file
   is empty after re-publishing a version, the reason is only on the person's terminal: ask them what it says. Bump
   the patch and publish again only when they say the version already holds a different image (an uncached rebuild
   rarely gives the same digest, so expect this when Scan found nothing new). Never guess the refusal from the empty
   file.
