# Project Vision — Unity Export to Roblox Studio

**Draft:** v0.1 — 2026-10-30  
**Repository:** `jit-md/unity-export-roblox-studio`  
**State:** Research / no implementation

## 1. Product thesis

Unity offers a rich authoring environment and many existing assets; Roblox Studio has its own importer, data model and runtime. Creators should be able to **transfer a useful subset of Unity-created content into Roblox Studio** with a reproducible workflow, explicit mapping and an honest compatibility report.

This is not an instant Unity-game-to-Roblox-game conversion promise. The initial value is *asset and scene transfer*, not behavioral equivalence.

## 2. User story

As a Unity creator, I select a prefab or small scene, invoke **Export to Roblox Studio**, receive importable output and a report, then import the result into Roblox Studio with documented steps and inspect the reconstructed content.

Success means the supported meshes/textures/transform structure arrive in Studio with acceptable fidelity and unsupported content is reported before or during export.

## 3. Target workflow

```text
Unity Editor
  | Select prefab / scene subset
  v
Analyzer
  | Dependencies, unsupported types, licensing reminders
  v
Converter / Exporter
  | Meshes, transforms, texture assets, material metadata
  | FBX or glTF (+ optional separate files)
  v
Export package
  | Files + JSON manifest + compatibility report
  v
Roblox Studio Importer
  | Import, verify assets, adjust settings when needed
  v
Validated Roblox scene subset
```

A later, independent companion tool inside Studio **might** read manifests and perform supported reconstruction actions. This is exploratory and is not required for the initial exporter.

## 4. Format findings (official documentation)

| Content | Candidate format | Validation needed |
| --- | --- | --- |
| Static meshes / hierarchy | FBX or glTF/GLB | Object count, nested transforms, naming, geometry and UVs |
| Simple static geometry | OBJ | Materials/hierarchy limitations |
| Textures | PNG/JPG | Channels, alpha and Roblox material mapping |
| Rig / animation | FBX or glTF | Rig and animation compatibility, later milestone |
| Game scripts | None for MVP | C# and Luau differ; manual or dedicated conversion |
| Unity shaders and gameplay physics | None for MVP | Engine-specific, may need approximation/rebuild |

Roblox documentation identifies `.fbx`, `.obj` and `.gltf` as mesh import formats; its glTF import section also references `.glb`. It describes FBX/glTF support for hierarchies, rigs and animations, with format-dependent differences. **Export capability in Unity and end-to-end fidelity must be verified through prototypes.**

Sources:
- https://create.roblox.com/docs/studio/importer
- https://create.roblox.com/docs/art/modeling/export-requirements
- https://create.roblox.com/docs/art/overview-dcc

## 5. Functional requirements — initial MVP

| ID | Requirement | Acceptance check |
| --- | --- | --- |
| FR-01 | Export selected Unity prefab / scene subset | Selected content exported without changing Unity project |
| FR-02 | Preserve supported static mesh geometry and UVs | Test objects inspectable in Roblox Studio |
| FR-03 | Carry basic texture references/assets | Sample textures map with known warnings |
| FR-04 | Preserve hierarchy and transforms when possible | Fixture parents/children compare with Roblox result |
| FR-05 | Generate JSON manifest and warnings | Missing/unsupported elements listed in report |
| FR-06 | Repeatable export | Same input/config produces structurally equivalent output |
| FR-07 | Provide manual Roblox import guide | Another developer reproduces import from clean workspace |

Not every candidate export format satisfies all requirements identically. Choose a primary format after fixture-based comparison.

## 6. Non-functional and safety requirements

- Avoid destructive edits to source scenes, prefabs or materials.
- Explicitly report conversion loss, unsupported object types and incompatible assets.
- Do not upload anything to Roblox without a separate opted-in action and rights review.
- Track versioned Unity and Roblox environments in compatibility test records.
- Do not ship third-party licensed Unity packages or example art without verified redistribution rights.
- Keep naming and manifest schemas deterministic; avoid leaking local absolute paths and personal identifiers.
- No credentials in generated bundles.

## 7. Compatibility concerns to investigate

1. **Coordinate systems and scale:** axis handedness, up direction, local/world transforms, pivots and unit conversion.
2. **Meshes:** mesh splits, normals, tangents, colors, UV channels, object limits, nested models.
3. **Materials:** Unity render pipeline/shader differences versus Roblox PBR and SurfaceAppearance.
4. **Textures:** extraction of external vs embedded assets, normal orientation and channel packing.
5. **Hierarchy:** nested prefabs, inactive objects, empty transforms and naming collisions.
6. **Colliders/physics:** map basic geometry or intentionally defer, never silently claim parity.
7. **Lighting / cameras / terrain:** separate conversion categories, likely out of first MVP.
8. **Animation:** hierarchy, armatures, bone naming, rig requirements and import differences.
9. **Roblox moderation / permissions:** asset upload requirements, moderation and ownership.
10. **Delivery:** Unity Package Manager package vs local Editor tool; manual Studio importer vs optional assisted import.

## 8. Roadmap

**M0 — Research and format spike:** Validate Unity export mechanisms, Roblox importer capabilities and test requirements. Choose versions and formats. Output: short architecture decision record.

**M1 — Static mesh MVP:** Unity Editor command exports one simple prefab; produce manifest, warning report and sample fixture; manually import into Studio.

**M2 — Scene fidelity:** Multiple objects, hierarchy, coordinate conversion, texture/material mapping, stable naming, golden fixture comparisons.

**M3 — Workflow improvement:** Batch selection, dependency reporting, export presets and user-friendly compatibility warnings.

**M4 — Advanced content research:** Rigged models/animations, basic colliders and selected scene metadata after confirmed technical feasibility.

**M5 — Optional Roblox-side companion:** Explore Studio plugin/automation for reconstructed hierarchies and metadata; require explicit permission boundaries.

A full C# to Luau port or generalized gameplay conversion is a separate, much larger research problem and is **not implied by these milestones**.

## 9. Open decisions

- Which Unity editor/version and render pipeline to target first?
- Primary interchange format: FBX, glTF/GLB, or multiple selectable outputs?
- Whether an external exporter library/package is necessary and compatible with licenses.
- How to represent missing features and material differences in the manifest.
- Whether preserving Roblox-native instances requires a separate Studio-side importer.
- License and governance before encouraging public contributions.

## 10. Relationship to other repositories

The Wako Roblox Studio plugin is a **distinct product**, focused on Wako identity, tasks, submissions and contribution attribution. This exporter must not depend on Wako and should remain usable independently.

## 11. First acceptance demo

Create a small original Unity scene with a cube, a textured mesh and a two-level transform hierarchy. Export with no source edits. Import the output in Roblox Studio, compare geometry, scale, orientation and textures, and inspect a report that describes any known conversion losses.
