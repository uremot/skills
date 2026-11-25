# Install `uremot` 1.3.7

The client release these skills were published with, pinned by version and by each binary's SHA-256. Pick the
machine's platform (`uname -s` and `uname -m`), show the person the block, then run it. It installs only when the
checksum matches: on a mismatch the download is not the reviewed release, so stop and tell the person.

## Linux, x86_64

```sh
d=$(mktemp -d) && curl -fsSLo "$d/uremot" https://releases.uremot.com/client/releases/v1.3.7/uremot-1.3.7-linux-x86_64 &&
  echo "733e30699d64d0cd93a745c4347e59b67468c61a1a9f7d5b888a0a0c6e812137  $d/uremot" | sha256sum -c - &&
  mkdir -p ~/.uremot/bin && cp "$d/uremot" ~/.uremot/bin/uremot && chmod 0755 ~/.uremot/bin/uremot
```

## Linux, aarch64 (arm64)

```sh
d=$(mktemp -d) && curl -fsSLo "$d/uremot" https://releases.uremot.com/client/releases/v1.3.7/uremot-1.3.7-linux-aarch64 &&
  echo "a250866d7479ba67be569cabff3028a23e2afd070903044fdb6cccbc8562a65b  $d/uremot" | sha256sum -c - &&
  mkdir -p ~/.uremot/bin && cp "$d/uremot" ~/.uremot/bin/uremot && chmod 0755 ~/.uremot/bin/uremot
```

## macOS, Intel or Apple silicon

```sh
d=$(mktemp -d) && curl -fsSLo "$d/uremot" https://releases.uremot.com/client/releases/v1.3.7/uremot-1.3.7-darwin-universal &&
  echo "afe026641ab32bda277bdf687af873b9bfa3709563d18c031a5b22395bdfbb09  $d/uremot" | shasum -a 256 -c - &&
  mkdir -p ~/.uremot/bin && cp "$d/uremot" ~/.uremot/bin/uremot && chmod 0755 ~/.uremot/bin/uremot
```

## Windows, x86_64, in Git Bash

```sh
d=$(mktemp -d) && curl -fsSLo "$d/uremot.exe" https://releases.uremot.com/client/releases/v1.3.7/uremot-1.3.7-windows-x86_64.exe &&
  echo "091db1935e4ae4ef23cb8f60c2b41bb37369b9d83e769c3330ecb31a40be38c9  $d/uremot.exe" | sha256sum -c - &&
  mkdir -p ~/.uremot/bin && cp "$d/uremot.exe" ~/.uremot/bin/uremot.exe && chmod 0755 ~/.uremot/bin/uremot.exe
```

Then `~/.uremot/bin/uremot --version` answers `1.3.7`. `uremot update` keeps it current and checks each newer
release's signature.
