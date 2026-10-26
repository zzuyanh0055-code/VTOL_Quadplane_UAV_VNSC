# Mechanical Design

**Status:** Native airframe CAD, neutral exports, and engineering drawings have not yet been uploaded. Gazebo meshes are already present under [simulation/models](../simulation/models), but they are not a replacement for editable CAD.

| Destination | Upload |
|---|---|
| [solidworks/](solidworks/README.md) | Final assembly and every referenced part/subassembly |
| [exports/](exports/README.md) | STEP assembly, drawings, and selected printable parts |

Keep physical revisions in separate subfolders such as `v1/`, `v2/`, and `v3/` once those files are available. Identify which revision is the final prototype and which generated the simulation geometry.

For each release, record the SolidWorks version, units, assembly entry point, configuration name, material assumptions, total mass, center of gravity, inertia tensor, and the coordinate system used. Explain differences between measured hardware and CAD-derived properties.
