# The Dockerfile

Part of the `uremot-onboard` skill's Write step; the Rules in `SKILL.md` apply.

The shape, in this order:

```dockerfile
# Written by the Uremot onboarding skill (uremot-onboard); Rebuild re-runs it.
# uremot-onboard: app=<name> version=<version>
FROM registry.uremot.com/uremot/uremot-os:<base version>

USER root

# <one RUN per toolchain or group of system packages, each with a comment
#  naming the finding it answers and the file the finding came from>

USER root
```

The rules, each of them checked below:

- The first line is exactly the marker above, and the second records the App and the version being published;
  Rebuild reads both.
- One `FROM`, by tag, with nothing after the version (`uremot publish` resolves the tag to a digest itself). No
  second stage.
- Install as `USER root` and end with `USER root`: the base's entrypoint starts as root and drops to the desktop user
  `uremot` (home `/home/uremot`) itself.
- The base is Debian (trixie) with `curl`, `jq` and `git`. Take a system package from `apt-get` when its version
  fits the evidence; end that `RUN` with `apt-get clean -y` and by deleting what is under `/var/lib/apt/lists/`. A
  package a CI file installs on Ubuntu can have another name in Debian, or none (`libappindicator3-dev` is
  `libayatana-appindicator3-dev` in trixie): check each name at `https://packages.debian.org/trixie/<name>` and say
  in the comment above the `RUN` which ones you renamed.
- Every other download names an exact version and is checked against the checksum its publisher ships where it ships
  one: Node against the release's `SHASUMS256.txt`, Go against `https://go.dev/dl/?mode=json`, `uv` against the
  `.sha256` beside its tarball, `rustup-init` against the `.sha256` beside it, a GitHub release asset against the
  `digest` the GitHub releases API lists for it. Write the checksum into the Dockerfile as an `ARG` where the
  publisher has no checksum file to fetch at build. Pick the build per architecture from `ARG TARGETARCH` (`amd64`,
  `arm64`) and fail on any other value.
- Each download lands in a directory from `mktemp -d`, removed in the same `RUN`, never at a fixed name under `/tmp`.
- After each install, check that the tool answers with the pinned version (`test "$(node --version)" = "v${NODE_VERSION}"`),
  so a wrong download fails the build.
- A tool installed from outside the distribution puts its license under `/usr/share/doc/<tool>/`.
- Nothing self-updates. Disable an updater the tool has; the image is republished instead.
- A toolchain the desktop user runs installs where that user can use it: system-wide under `/usr/local`, or for
  per-user installers (`rustup`), `RUSTUP_HOME`/`CARGO_HOME` under `/usr/local` with `ENV PATH` extended and the
  directories made writable by `uremot`.
- No checkout is baked in, and nothing from the repository is copied in: no `COPY` or `ADD`.
- No harness, `uremot` client or provider CLI: the desk brings them (above).
- No `ARG` or `ENV` holds a secret. Credentials are connections granted when an Instance is launched.

Check the written Dockerfile; each command prints nothing when it complies:

```sh
cd <output directory>
head -n 1 Dockerfile | grep -vxF '# Written by the Uremot onboarding skill (uremot-onboard); Rebuild re-runs it.'
grep -nE '^(FROM|ARG BASE_IMAGE=)' Dockerfile \
  | grep -vE 'registry\.uremot\.com/uremot/uremot-os:[0-9]+\.[0-9]+\.[0-9]+$'
grep -inE '^(ARG|ENV)[[:space:]]+[A-Z0-9_]*(TOKEN|SECRET|PASSWORD|API_KEY|PRIVATE)' Dockerfile
grep -nE '^(COPY|ADD)[[:space:]]' Dockerfile
grep -n '{{' Dockerfile
tail -n 1 Dockerfile | grep -vx 'USER root'
```
