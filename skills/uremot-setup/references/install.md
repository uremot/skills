# Install `uremot` 1.3.8

The client release these skills were published with, pinned by version and by each binary's SHA-256. Pick the
machine's platform (`uname -s` and `uname -m`), show the person the block, then run it. It installs only when the
checksum matches: on a mismatch the download is not the reviewed release, so stop and tell the person.

## Linux, x86_64

```sh
d=$(mktemp -d) && curl -fsSLo "$d/uremot" https://releases.uremot.com/client/releases/v1.3.8/uremot-1.3.8-linux-x86_64 &&
  echo "9c7a017075f3e62697e3f8c41d52bd9ff7591c357f2b17c41c6d81fd0f091e47  $d/uremot" | sha256sum -c - &&
  mkdir -p ~/.uremot/bin && cp "$d/uremot" ~/.uremot/bin/uremot && chmod 0755 ~/.uremot/bin/uremot
```

## Linux, aarch64 (arm64)

```sh
d=$(mktemp -d) && curl -fsSLo "$d/uremot" https://releases.uremot.com/client/releases/v1.3.8/uremot-1.3.8-linux-aarch64 &&
  echo "99b4eab45ec799d513d3d29e7655ebedb38274e54dbc8b9aaa6851eb31aa19f3  $d/uremot" | sha256sum -c - &&
  mkdir -p ~/.uremot/bin && cp "$d/uremot" ~/.uremot/bin/uremot && chmod 0755 ~/.uremot/bin/uremot
```

## macOS, Intel or Apple silicon

```sh
d=$(mktemp -d) && curl -fsSLo "$d/uremot" https://releases.uremot.com/client/releases/v1.3.8/uremot-1.3.8-darwin-universal &&
  echo "5954b0feb23e517fa8841f2cad4739de38f7246467865803612c26813e8259b6  $d/uremot" | shasum -a 256 -c - &&
  mkdir -p ~/.uremot/bin && cp "$d/uremot" ~/.uremot/bin/uremot && chmod 0755 ~/.uremot/bin/uremot
```

## Windows, x86_64, in Git Bash

```sh
d=$(mktemp -d) && curl -fsSLo "$d/uremot.exe" https://releases.uremot.com/client/releases/v1.3.8/uremot-1.3.8-windows-x86_64.exe &&
  echo "ce61d5d07deac4b64634ecdeaeb5064a36a312864e2c7219e65418dd0341f0e3  $d/uremot.exe" | sha256sum -c - &&
  mkdir -p ~/.uremot/bin && cp "$d/uremot.exe" ~/.uremot/bin/uremot.exe && chmod 0755 ~/.uremot/bin/uremot.exe
```

Then `~/.uremot/bin/uremot --version` answers `1.3.8`. `uremot update` keeps it current and checks each newer
release's signature.
