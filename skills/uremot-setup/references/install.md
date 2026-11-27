# Install `uremot` 1.3.13

The client release these skills were published with, pinned by version and by each binary's SHA-256. Pick the
machine's platform (`uname -s` and `uname -m`), show the person the block, then run it. It installs only when the
checksum matches: on a mismatch the download is not the reviewed release, so stop and tell the person.

## Linux, x86_64

```sh
d=$(mktemp -d) && curl -fsSLo "$d/uremot" https://releases.uremot.com/client/releases/v1.3.13/uremot-1.3.13-linux-x86_64 &&
  echo "65ff39b3eb70dc3819a9aa7719591f1c3cff2bbe76aadfcf1a5e55470240599c  $d/uremot" | sha256sum -c - &&
  mkdir -p ~/.uremot/bin && cp "$d/uremot" ~/.uremot/bin/uremot && chmod 0755 ~/.uremot/bin/uremot
```

## Linux, aarch64 (arm64)

```sh
d=$(mktemp -d) && curl -fsSLo "$d/uremot" https://releases.uremot.com/client/releases/v1.3.13/uremot-1.3.13-linux-aarch64 &&
  echo "9bcf0e0840f46fb8c0e6eb936a870ee91b8b42cc6bf4f03c07ea90d2d8aa8d62  $d/uremot" | sha256sum -c - &&
  mkdir -p ~/.uremot/bin && cp "$d/uremot" ~/.uremot/bin/uremot && chmod 0755 ~/.uremot/bin/uremot
```

## macOS, Intel or Apple silicon

```sh
d=$(mktemp -d) && curl -fsSLo "$d/uremot" https://releases.uremot.com/client/releases/v1.3.13/uremot-1.3.13-darwin-universal &&
  echo "bc2520ab2b91447b1f16b8333a7dba76a9e5c39673de64198a3d9b05e763d56d  $d/uremot" | shasum -a 256 -c - &&
  mkdir -p ~/.uremot/bin && cp "$d/uremot" ~/.uremot/bin/uremot && chmod 0755 ~/.uremot/bin/uremot
```

## Windows, x86_64, in Git Bash

```sh
d=$(mktemp -d) && curl -fsSLo "$d/uremot.exe" https://releases.uremot.com/client/releases/v1.3.13/uremot-1.3.13-windows-x86_64.exe &&
  echo "4410183b840dd7b5e70ee7815559ae76eca598881abeb212835a9a2e8d8f2c02  $d/uremot.exe" | sha256sum -c - &&
  mkdir -p ~/.uremot/bin && cp "$d/uremot.exe" ~/.uremot/bin/uremot.exe && chmod 0755 ~/.uremot/bin/uremot.exe
```

Then `~/.uremot/bin/uremot --version` answers `1.3.13`. `uremot update` keeps it current and checks each newer
release's signature.
