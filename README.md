# Horizon Mod API

A complete modding API for [Horizon](https://github.com/horizon-rust/horizon) (Minecraft: Java Edition 26.3, rewritten in Rust): **20 capabilities** that let a sandboxed server mod build real tech-mod content — machines, energy networks, GUIs, multiblocks, fluids, mobs, configs — safely, with saves that survive, on servers and in single player.

Built capability by capability, one tested commit each (`Horizon-Forge-Api:` trailers), gated by `pr_ready --quick` per stage.

## What a mod can do (capabilities)

| | |
|---|---|
| block entities, inventories | data on blocks, ticking, saved with the world, shared slots |
| energy, fluids | buffers/tanks, neighbor push, buckets |
| menus, network | chest GUIs with buttons, server↔client channels, real screens |
| block use, item hooks | right-click machines, items with logic, armor |
| machine recipes, multiblocks | JSON recipe types, structure matching, formed/broken events |
| saved data, config | per-world data, live-editable TOML config |
| entities, displays, item displays | own mobs on vanilla bodies, animated world models, riders |
| redstone, effects, game events | signal read/emission, particles & sounds, damage/death/weather events |
| server content, inter-mod | one `.hmod` = content + logic, message bus between mods |

Mods are **WebAssembly components** (wasmtime sandbox, WIT contract): fuel/memory limited, no direct engine access — safe to distribute and enable in the launcher. The `cells` example mod in the patch series exercises every capability.

## What's in this repo

- **`patches/api.mbox`** — the whole API as a git patch series (43 commits, apply with `git am --3way` on top of Horizon's `main`; `horizon-forge install-api` does it for you and skips what's already applied)
- **`wit/horizon.wit`** — the mod contract itself
- **`docs/`** — mod author docs (`mods.md`) and the generated API reference
- **`report/`** — full engineering report (in Russian): how each stage works, verification, limitations, the jar-porting plan

## Applying it

```sh
git clone https://github.com/horizon-rust/horizon && cd horizon
git am --3way < /path/to/api.mbox   # or: horizon-forge install-api --src .
cargo build --profile fast -p horizon-client --features horizon-client/mods
```

## Status

API level 24, 100+ contract functions, every stage committed with its own tests and docs. More capabilities (Forge-parity set: free-layout GUIs, entity models/AI, enchantments, dimensions, …) are in progress — this pack is refreshed as they land.

Authored with AI agents (ZCode/GLM-5.3 + Claude Code), supervised by a human. Commits carry co-author trailers.
