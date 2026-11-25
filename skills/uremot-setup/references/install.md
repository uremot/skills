# Install `uremot` 1.3.10

The client release these skills were published with, pinned by version and by each binary's SHA-256. Pick the
machine's platform (`uname -s` and `uname -m`), show the person the block, then run it. It installs only when the
checksum matches: on a mismatch the download is not the reviewed release, so stop and tell the person.

## Linux, x86_64

```sh
d=$(mktemp -d) && curl -fsSLo "$d/uremot" https://releases.uremot.com/client/releases/v1.3.10/uremot-1.3.10-linux-x86_64 &&
  echo "b29e7991fe8fd9607543813be2574e2af56094277bd9b4a0348ddac1c803ecfe  $d/uremot" | sha256sum -c - &&
  mkdir -p ~/.uremot/bin && cp "$d/uremot" ~/.uremot/bin/uremot && chmod 0755 ~/.uremot/bin/uremot
```

## Linux, aarch64 (arm64)

```sh
d=$(mktemp -d) && curl -fsSLo "$d/uremot" https://releases.uremot.com/client/releases/v1.3.10/uremot-1.3.10-linux-aarch64 &&
  echo "cfe44ebcebbf8c1a127e1755863c494b420b2206b0b6f759054e7854c60d0f0a  $d/uremot" | sha256sum -c - &&
  mkdir -p ~/.uremot/bin && cp "$d/uremot" ~/.uremot/bin/uremot && chmod 0755 ~/.uremot/bin/uremot
```

## macOS, Intel or Apple silicon

```sh
d=$(mktemp -d) && curl -fsSLo "$d/uremot" https://releases.uremot.com/client/releases/v1.3.10/uremot-1.3.10-darwin-universal &&
  echo "aeda44318d1f6844ddf6ca23466060213cad52405a7d59bf19add1f57da175bf  $d/uremot" | shasum -a 256 -c - &&
  mkdir -p ~/.uremot/bin && cp "$d/uremot" ~/.uremot/bin/uremot && chmod 0755 ~/.uremot/bin/uremot
```

## Windows, x86_64, in Git Bash

```sh
d=$(mktemp -d) && curl -fsSLo "$d/uremot.exe" https://releases.uremot.com/client/releases/v1.3.10/uremot-1.3.10-windows-x86_64.exe &&
  echo "aaf350cfc0050c7dcf8ba64f59d949cbe088c7b81a7767ec1cfe563a68a1554b  $d/uremot.exe" | sha256sum -c - &&
  mkdir -p ~/.uremot/bin && cp "$d/uremot.exe" ~/.uremot/bin/uremot.exe && chmod 0755 ~/.uremot/bin/uremot.exe
```

Then `~/.uremot/bin/uremot --version` answers `1.3.10`. `uremot update` keeps it current and checks each newer
release's signature.
