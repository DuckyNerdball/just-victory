# Contributing to just-victory

Thanks for helping. just-victory gathers the working fixes from
[VIctory](https://github.com/Marvisak/VIctory),
[victory-fork](https://github.com/fxceless/victory-fork) and other forks in
one place, and adds its own. Pull requests, test reports and issues are all
welcome.

## Rule #1: no game data, ever

Read [docs/asset-policy.md](docs/asset-policy.md) first. Nothing from the
game goes into this repository or into issues and pull requests here: no
assets, no shader dumps or disassembly, no strings or tables from the
executable, **no screenshots or videos of the game**. Hashes, offsets, counts,
timings and error messages are fine. A pull request that breaks this is
closed, and the history is rewritten to remove it.

## Where help is most useful

- **Rendering on NVIDIA and AMD**: GPU hangs (`DXGI_ERROR_DEVICE_HUNG`),
  device removals, draws dropped for missing pipelines.
- **Shader translation** (RDNA2 / gfx1013 to DXIL): the refusal reasons
  `victory translate <game dir>\shaders.rpf` lists, most common first.
- **Lighting**: shadowed areas are still black (ray-traced lighting does not
  run yet; see [docs/fork-ray-tracing.md](docs/fork-ray-tracing.md)).
- **Speed**: the world runs at about 1 to 4 frames per second.
- **Porting fixes from other forks**: credit the original author in the
  commit (`Co-Authored-By:` or a link to their commit).

## Building

Windows 11, the Rust toolchain pinned in `rust-toolchain.toml`, and the
Visual Studio 2022 Build Tools (run cargo from a "x64 Native Tools" prompt).

```powershell
cargo build --release -p victory
# Diagnostics build: logs, frame captures, DRED breadcrumbs
cargo build --profile diag -p victory --features diagnostics
```

Copy `victory.example.toml` to `victory.toml` and point `game_dir` at your
own copy of the game.

## Before you open a pull request

```powershell
cargo clippy --workspace --all-targets --locked -- -D warnings
cargo clippy --workspace --all-targets --all-features --locked -- -D warnings
cargo test --workspace --locked
cargo xtask policy
```

In the description, say what changed, why, and how you tested it: GPU and
driver version, how long the run lasted in the world, and what got better
(fewer dropped draws, no device removal, frame time). One topic per pull
request. AI-assisted work is welcome; please say so.

## How pull requests are judged

A pull request is merged when it is tested, keeps rule #1, does not make
another GPU worse where that can be checked, and moves the project forward:
something renders, translates, runs longer or runs faster than before.
Diagnostics that help find the next problem count too.
