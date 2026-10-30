# AGENTS.md — Unity → Roblox Studio Exporter

For A2A agent handoffs and persistence rules, read [AGENTS-SYNC.md](AGENTS-SYNC.md). Storage is created only when needed; no A2A server or complete transcript recorder is implied.

These instructions apply to all files in this repository. Read `README.md` and `docs/PROJECT_VISION.md` first.

## Mission and scope

Build an experimental, testable **Unity Editor → Roblox Studio** asset and scene export workflow. Prioritize faithful supported assets, visible limitations, and easy inspection over unrealistic one-click game conversion.

This repository is **not** the Wako Roblox Studio contribution plugin. Do not implement Wako identity, wallets, governance, rewards or project-task integrations here.

## Engineering principles

1. **Research before implementation.** Check official Unity and Roblox documentation and record verified behaviors and tested versions. Never invent API names or assert that a format preserves data without testing.
2. **Small, measurable MVP.** Start with a static prefab/scene subset containing meshes, textures and transforms. Prefer repeatable exports with a small fixture scene.
3. **Non-destructive.** Export must never silently change source scenes or assets. Do not overwrite output without explicit behavior.
4. **Determinism and diagnostics.** Stable names, paths and manifests; generate warnings for unsupported or lossy content. Avoid silent omissions.
5. **Separation of concerns.** Unity editor adapter, conversion model, format exporters, manifest/report generator and potential Roblox importer companion must be independently testable.
6. **Compatibility is evidence-based.** Geometry, hierarchy, coordinate systems, scale, UVs, textures, material channels, rigs and animations are distinct test cases.
7. **Respect permissions.** Do not export or upload third-party assets without appropriate rights; respect Roblox asset moderation and published requirements.
8. **No embedded secrets.** Keep API keys, tokens and credentials out of repository files, exports, and logs. Do not publish assets automatically.
9. **Minimize dependencies.** Document licensing and compatibility before adding any dependency or binary.
10. **Communicate honestly.** Record tests actually run, limitations and unresolved decisions in every implementation report.

## Proposed structure (not yet scaffolded)

```
Editor/               # Unity Editor export integration
RuntimeOrCore/        # Engine-independent conversion model if practical
Tests/                # Unit tests and export fixture tests
fixtures/             # Small owned/licensed Unity test content
docs/                 # Architecture, decisions and compatibility findings
research/             # Official format/API research and comparisons
```

The final Unity package layout and assembly definition structure should be chosen after verifying the target Unity version and Unity Package Manager conventions.

## Design / PR checklist

- Define an observable acceptance test for each conversion feature.
- Specify the source Unity object types and intended Roblox representations.
- Document coordinate/axis and unit mapping, hierarchy fidelity and precision loss.
- Handle unsupported materials, scripts, colliders and animations explicitly.
- Include both success and expected-failure fixtures.
- Do not claim direct Roblox Studio insertion unless demonstrated through a supported API.
- Update docs with verified constraints and manual import instructions.

## First milestone

Research Unity export options, Roblox Studio importer requirements, and choose between FBX and glTF as primary MVP formats. Produce one simple, inspectable export and a manual Roblox Studio import validation report.

## Things not to build yet

- Full Unity game converter or automatic C# → Luau translation.
- Wako plugin features.
- Token or revenue functionality.
- Cloud upload, authentication or publishing automation.
- Complex animation/rigging support before static scenes are validated.
