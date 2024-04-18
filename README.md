### 1. starting out with just the flake.nix and this README

the remote branch currently is at dfa67b702be5d30b5dc33ff31443a024dd8be7a8

in the holochain repo:

```
git checkout -B debug-flake-lockfile dfa67b702be5d30b5dc33ff31443a024dd8be7a8
Reset branch 'debug-flake-lockfile'
git push -f
```

### 2. implicitly initialize the lock file by asking for the scaffold rev

```
# verify scaffolding version and rev
nix eval --refresh .#packages.aarch64-linux.hc-scaffold.src.rev
"ddb8b5a950e7be81d704000e662564696bef5d6b"
```

### 3. update to the commit with the scaffolding bump

include the scaffolding bump on the remote branch
in the holochain repo:

```
git cherry-pick f306345b5a14dc656f8bc51727338803f1e42361
git push
```

back in this repo update the flake and check again

```
nix flake update

warning: updating lock file '/tmp/tmp.hOGlQYSF2Q/flake.lock':
• Updated input 'holochain-flake':
    'github:holochain/holochain/dfa67b702be5d30b5dc33ff31443a024dd8be7a8?narHash=sha256-FY95ac2E4lw4kNHDzbBfJsIOVdj%2BUzWZGHTQFvYYzW8%3D' (2024-04-09)
  → 'github:holochain/holochain/4aa8afd66e2b9fbeed49f9d76af80ed3d066e017?narHash=sha256-KLL4yvESe14SjDoAqi2vKzoDNk0qt/zgfmfwHEq2agc%3D' (2024-04-18)
• Updated input 'versions':
    'github:holochain/holochain/dfa67b702be5d30b5dc33ff31443a024dd8be7a8?dir=versions/0_2&narHash=sha256-FY95ac2E4lw4kNHDzbBfJsIOVdj%2BUzWZGHTQFvYYzW8%3D' (2024-04-09)
  → 'github:holochain/holochain/4aa8afd66e2b9fbeed49f9d76af80ed3d066e017?dir=versions/0_2&narHash=sha256-KLL4yvESe14SjDoAqi2vKzoDNk0qt/zgfmfwHEq2agc%3D' (2024-04-18)
• Updated input 'versions/scaffolding':
    'github:holochain/scaffolding/ddb8b5a950e7be81d704000e662564696bef5d6b?narHash=sha256-zjjAmu8t566/PUxo2g0EFZ4GAdUu/UbkGA8hsVbFEoQ%3D' (2024-02-19)
  → 'github:holochain/scaffolding/8c1631200b6168d8e14fdfefb453762ae8feb14c?narHash=sha256-XE%2BzErQBAu7UxSGCrYMHa3YDiuB%2BdqSEVyuLP/dDO9E%3D' (2024-04-09)

nix eval --refresh .#packages.aarch64-linux.hc-scaffold.src.rev
"8c1631200b6168d8e14fdfefb453762ae8feb14c"
```

### scratchpad

```
nix flake lock \
    --override-input holochain-flake "github:holochain/holochain/f306345b5a14dc656f8bc51727338803f1e42361" \
     --override-input versions "github:holochain/holochain/f306345b5a14dc656f8bc51727338803f1e42361?dir=versions/0_2"

nix eval --impure --expr 'let flake = builtins.getFlake (builtins.getEnv "PWD"); in flake.inputs.versions.inputs.scaffolding.rev'
nix eval --impure --expr 'let flake = builtins.getFlake (builtins.getEnv "PWD"); in flake.inputs.holochain-flake.inputs.versions.inputs.scaffolding.rev'
"ddb8b5a950e7be81d704000e662564696bef5d6b"
"ddb8b5a950e7be81d704000e662564696bef5d6b"
```
