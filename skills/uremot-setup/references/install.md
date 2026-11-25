# Install `uremot` 1.3.9

The client release these skills were published with, pinned by version and by each binary's SHA-256. Pick the
machine's platform (`uname -s` and `uname -m`), show the person the block, then run it. It installs only when the
checksum matches: on a mismatch the download is not the reviewed release, so stop and tell the person.

## Linux, x86_64

```sh
d=$(mktemp -d) && curl -fsSLo "$d/uremot" https://releases.uremot.com/client/releases/v1.3.9/uremot-1.3.9-linux-x86_64 &&
  echo "4acd3016f47e009c6886a5e87496a2a3ea623544e23889ac3b54cf823560a130  $d/uremot" | sha256sum -c - &&
  mkdir -p ~/.uremot/bin && cp "$d/uremot" ~/.uremot/bin/uremot && chmod 0755 ~/.uremot/bin/uremot
```

## Linux, aarch64 (arm64)

```sh
d=$(mktemp -d) && curl -fsSLo "$d/uremot" https://releases.uremot.com/client/releases/v1.3.9/uremot-1.3.9-linux-aarch64 &&
  echo "0410468a93a6b89faf4bf112796ebdab33220fc7b7283b277ba9a51758b00d0f  $d/uremot" | sha256sum -c - &&
  mkdir -p ~/.uremot/bin && cp "$d/uremot" ~/.uremot/bin/uremot && chmod 0755 ~/.uremot/bin/uremot
```

## macOS, Intel or Apple silicon

```sh
d=$(mktemp -d) && curl -fsSLo "$d/uremot" https://releases.uremot.com/client/releases/v1.3.9/uremot-1.3.9-darwin-universal &&
  echo "46d9c416e578f673c2d50c087ca793481d94d238cb3fee1a74f4ae9ff7bdcf39  $d/uremot" | shasum -a 256 -c - &&
  mkdir -p ~/.uremot/bin && cp "$d/uremot" ~/.uremot/bin/uremot && chmod 0755 ~/.uremot/bin/uremot
```

## Windows, x86_64, in Git Bash

```sh
d=$(mktemp -d) && curl -fsSLo "$d/uremot.exe" https://releases.uremot.com/client/releases/v1.3.9/uremot-1.3.9-windows-x86_64.exe &&
  echo "fb82128100502390854d49b1a18158ef46a61a8b497767a64d219b12b5a7d767  $d/uremot.exe" | sha256sum -c - &&
  mkdir -p ~/.uremot/bin && cp "$d/uremot.exe" ~/.uremot/bin/uremot.exe && chmod 0755 ~/.uremot/bin/uremot.exe
```

Then `~/.uremot/bin/uremot --version` answers `1.3.9`. `uremot update` keeps it current and checks each newer
release's signature.
