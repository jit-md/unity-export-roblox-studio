# Unity → Roblox Studio Exporter

> **Agent-friendly repository:** AI coding agents are welcome. Start with [AGENTS.md](AGENTS.md) for instructions and [AGENT-SYNC.md](AGENT-SYNC.md) for A2A v1.0 Git-based handoff and persistence conventions. The [`.a2a/`](.a2a/manifest.json) directory stores machine-readable metadata and, when explicitly recorded, authorized messages, tasks and artifacts. It is not a live A2A server.

An experimental toolchain to **export selected Unity assets and scenes into Roblox Studio**, with a focus on reproducible conversion, clear compatibility reports, and the least possible manual rework.

**Status:** Research and design. No exporter implementation exists yet.

## Goal

Start with a Unity Editor export workflow that packages supported content for import into Roblox Studio. Treat "export a scene" as a staged process, **not** as instant full game conversion: Unity and Roblox have different scripting languages, physics, materials, asset pipelines, and runtime behavior.

## Intended workflow

1. Select a Unity prefab, asset collection, or scene.
2. Analyze dependencies and unsupported content.
3. Export compatible meshes, textures, and optionally animations through verified interchange formats.
4. Generate a manifest with names, hierarchy, transforms, units, asset mappings, and warnings.
5. Import into Roblox Studio using its supported importer; evaluate an optional Roblox-side reconstruction assistant later.
6. Validate visual and structural fidelity with a small test scene and a conversion report.

## Technical starting points

- **FBX** and **glTF/GLB** for 3D asset interchange; **OBJ** for simple static meshes.
- Images/textures in supported formats such as PNG.
- Script, shader, physics and gameplay conversion require separate research and are **not part of the first MVP**.
- Roblox's documentation lists `.fbx`, `.obj`, and `.gltf` for meshes, and also describes `.glb` imports in its glTF hierarchy documentation. Verify format-specific behavior with actual files.
- The initial export target is **files that Roblox Studio can import**, not a direct authenticated upload or guaranteed Roblox-native project file.

## MVP boundaries

**In scope:** one static Unity prefab or a small scene subset; meshes, textures, hierarchy/transforms where supported, deterministic export, conversion manifest, error diagnostics, and a documented Studio import test.

**Later:** animations/rigs, material mapping, terrain, lights, colliders, batch export, Roblox-side importer automation, and optional Open Cloud asset publishing, all subject to technical verification.

**Out of scope for MVP:** Unity C# → Luau source conversion, complete gameplay porting, automatic parity between physics engines, silently importing unlicensed third-party content, and Wako account/task/reward features.

## Separate projects

- **This repository:** Unity-to-Roblox Studio asset/scene export.
- **[Roblox Studio Plugin for Wako](https://github.com/jit-md/roblox-studio-plugin-wako):** project membership, tasks and contribution tracking through Wako. Do not couple the two codebases.

## Repository guides

- [AGENTS.md](AGENTS.md) — coding-agent and contributor rules.
- [docs/PROJECT_VISION.md](docs/PROJECT_VISION.md) — product vision, research, architecture, roadmap and validation criteria.

## References

- [Roblox Studio Importer](https://create.roblox.com/docs/studio/importer)
- [Roblox export settings / model preparation](https://create.roblox.com/docs/art/modeling/export-requirements)
- [Roblox Studio and third-party software](https://create.roblox.com/docs/art/overview-dcc)

## Contribution

The repository is currently public, but license and contribution terms have **not yet been selected**. Opening source does not automatically grant reuse rights. Confirm licensing and dependency provenance before accepting substantial external contributions.
