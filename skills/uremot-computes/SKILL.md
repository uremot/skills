---
name: uremot-computes
description: Set up, check and repair a Uremot Compute, the machine Instances run on. Use to install and enroll a machine, tell whether it can run containers and is online, read it from the platform and on the machine, and fix one that is offline, has no container engine, or lost its identity.
---

# Uremot computes

A Compute is a machine enrolled into a workspace. Its daemon connects it to the platform and runs Instances as
containers through a container engine (Docker, or any engine that serves the Docker socket). A desktop app Compute
also shares its own desktop, which needs no engine.

## Set up a headless Linux machine

1. **A container engine.** `docker info` must answer. On Linux install Docker Engine
   (https://docs.docker.com/engine/install/); on macOS or Windows, Docker Desktop. A Linux Compute that runs as its
   user, not as the service, needs that user in the `docker` group: `sudo usermod -aG docker $USER`, then a new login.
2. **The binary.** `curl -fsSL https://get.uremot.com | sh` installs `uremot` for the current user. As root with
   `--daemon` (`curl -fsSL https://get.uremot.com | sudo sh -s -- --daemon`) it also installs the `uremot-compute`
   systemd service. `--daemon` needs systemd, and stops when no container engine answers, having changed nothing:
   install the engine, or add `--no-containers` to install without one. Without systemd, install without
   `--daemon` and keep `uremot compute run` running under your init system.
3. **Enroll.** `uremot compute enroll --sign-in` prints a URL and a code that the person approves in their own
   browser; show them where it runs, and never relay the code in chat. It records the approving person as owner:
   only they and their agents can launch on it. `uremot compute enroll --grant-stdin`, with a grant from the Console's
   **Add a Compute** page (**Headless Linux server**, then **Create enrollment grant**), enrolls a **shared** machine
   instead: workspace members and their agents can launch on it, and whoever operates it can reach its workloads and
   credentials. On a `--daemon` install run either with `sudo`, since the service reads root's identity; enroll then
   starts the service.
4. **Run it.** When enroll ends with a `Next:` line, run that line. It is the one step left. Without the service,
   the machine is a Compute only while `uremot compute run` runs, and nothing starts it again after a closed shell, a
   crash or a reboot: keep it running in tmux, a supervisor or the init system.

An engine installed later is found within a few seconds, with no restart, except by `uremot compute run` on Windows:
restart it there once the engine runs.

**A machine a person sits at** (macOS, Windows, or a Linux desktop): they install the desktop app, sign in, name the
machine, choose the organization and workspace, and press **Join**. It also needs an engine to run Instances.

## Check from the platform

`list_computes`, or `uremot agent computes` in a shell, lists the machines you can launch on:
- `presence`: `online` while its daemon reports. An offline machine keeps intent and acts on it when it returns.
- `containers`: `available` (it can run Instances), `unsupported` (no engine answers there), `blocked` (its owner
  turned containers off on that machine, and only they can change it) or `unknown` (not reported yet).
- `client_version`: absent when it never connected.
- `network.status`: its place on the workspace network; `unsupported` means its client is too old for it.

## Check on the machine

- `uremot compute status` prints the identity, then `containers` (`available (container engine <version>)`, `unavailable: no
  container engine answers`, `unavailable: Docker is installed, but this user can't reach it…` or `blocked by local
  policy`) and `service` (`active`, `inactive`, `failed`, or `not installed`). It works before enroll too; `--json`
  carries `containers`, `containers_reason` (`no engine` or `no permission`) and `service`.
- `journalctl -u uremot-compute` shows the service's log; `docker info` shows whether the engine answers.

## Repair

- **Offline:** on a service install, `sudo systemctl enable --now uremot-compute`; otherwise `uremot compute run`.
  A service that stays `failed` explains itself in `journalctl -u uremot-compute`.
- **`containers` is `unsupported`:** install or start the engine. The daemon finds it within a few seconds; on Windows,
  restart `uremot compute run`.
- **Docker is installed, but this user can't reach it** (`containers_reason` `no permission`): the engine's socket
  refuses this user. `sudo usermod -aG docker $USER`, then log out and in, and start `uremot compute run` (or the
  desktop app) again in the new login. A service install runs as root and never meets this.
- **`blocked`:** only the machine's owner can allow containers again, on that machine.
- **Old client:** `uremot update` installs the latest release, after verifying it. A desk created since the
  machine first filled its runtime takes it at its next start (stop, then start or restart it); an older desk takes
  it with a new App version.
- **Broken identity:** `uremot compute recover --grant-stdin`, with a recovery grant a workspace admin issues from
  the machine's page in the Console, keeps its Compute record (`sudo` on a service install, which it stops and
  starts again). Anywhere else, stop the daemon first: recover refuses while it runs. Without a grant, stop the
  daemon (`sudo systemctl stop uremot-compute` on a service install), `uremot compute reset`, then enroll again.
  Reset refuses while the daemon runs.

On a machine you can't reach, ask its person in chat for the exact command to run, never a guess.
