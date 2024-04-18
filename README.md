### 1. starting out with just the flake.nix and this README

### 2. initialize the lock with versions overriden to a commit before the last scaffolding update

```
nix flake lock --override-input versions "github:holochain/holochain/dfa67b702be5d30b5dc33ff31443a024dd8be7a8?dir=versions/0_2"

# verify scaffolding version and rev
nix eval .#packages.aarch64-linux.hc-scaffold.src.rev
nix eval .#packages.aarch64-linux.hc-scaffold.version
```
