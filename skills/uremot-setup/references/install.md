# Install `uremot` 1.3.12

The client release these skills were published with, pinned by version and by each binary's SHA-256. Pick the
machine's platform (`uname -s` and `uname -m`), show the person the block, then run it. It installs only when the
checksum matches: on a mismatch the download is not the reviewed release, so stop and tell the person.

## Linux, x86_64

```sh
d=$(mktemp -d) && curl -fsSLo "$d/uremot" https://releases.uremot.com/client/releases/v1.3.12/uremot-1.3.12-linux-x86_64 &&
  echo "ee789518411f515ebce7b62aa75249422a81184a592b1f755d8134f93df796a3  $d/uremot" | sha256sum -c - &&
  mkdir -p ~/.uremot/bin && cp "$d/uremot" ~/.uremot/bin/uremot && chmod 0755 ~/.uremot/bin/uremot
```

## Linux, aarch64 (arm64)

```sh
d=$(mktemp -d) && curl -fsSLo "$d/uremot" https://releases.uremot.com/client/releases/v1.3.12/uremot-1.3.12-linux-aarch64 &&
  echo "d378d808e8640ceb9defd48642e1dd21d3735aff3b7a6ef26cce6fc9f2ae210a  $d/uremot" | sha256sum -c - &&
  mkdir -p ~/.uremot/bin && cp "$d/uremot" ~/.uremot/bin/uremot && chmod 0755 ~/.uremot/bin/uremot
```

## macOS, Intel or Apple silicon

```sh
d=$(mktemp -d) && curl -fsSLo "$d/uremot" https://releases.uremot.com/client/releases/v1.3.12/uremot-1.3.12-darwin-universal &&
  echo "a1c2900818120efa1587ecf911434e3be51f1e06465d2d8611e5bb78522cfb74  $d/uremot" | shasum -a 256 -c - &&
  mkdir -p ~/.uremot/bin && cp "$d/uremot" ~/.uremot/bin/uremot && chmod 0755 ~/.uremot/bin/uremot
```

## Windows, x86_64, in Git Bash

```sh
d=$(mktemp -d) && curl -fsSLo "$d/uremot.exe" https://releases.uremot.com/client/releases/v1.3.12/uremot-1.3.12-windows-x86_64.exe &&
  echo "8d1af67022580ca8b943785f034ab9410ec9c28796522f3f9370383b61ea9ae6  $d/uremot.exe" | sha256sum -c - &&
  mkdir -p ~/.uremot/bin && cp "$d/uremot.exe" ~/.uremot/bin/uremot.exe && chmod 0755 ~/.uremot/bin/uremot.exe
```

Then `~/.uremot/bin/uremot --version` answers `1.3.12`. `uremot update` keeps it current and checks each newer
release's signature.
