# Location-Triggered Roll Disturbance

[Simulation setup](../../simulation/README.md) · [Vietnamese guide](README_VI.md)

This Gazebo Sim 8 system plugin applies a body-X roll torque to a selected link during its first passage through an axis-aligned world-coordinate box. It rotates the body axis into world coordinates at every simulation step.

## Current Project Configuration

Source: [project model.sdf](../../simulation/models/VTOL_Quadplane/model.sdf).

| Parameter | Value |
|---|---|
| `link_name` | `base_link` |
| `x_min`, `x_max` | 371.5, 416.5 m |
| `y_min`, `y_max` | 925, 965 m |
| `z_min`, `z_max` | 35, 70 m |
| `roll_torque_nm` | 1 N·m |

The source defaults include a 0.60 N·m moment; the project SDF overrides it to 1 N·m.

## Trigger Behavior

1. The plugin arms when the model is created.
2. The first entry into the box starts the disturbance.
3. Torque remains active while the link remains inside the box.
4. Leaving the box stops the torque and locks the trigger.
5. Restart the simulation to re-arm it.

There is **no `duration_s` parameter** in the current implementation. The earlier fixed-duration description does not apply. Check the `Armed`, `ON`, and `OFF` console messages and retain the recorded exposure time.

## Build

From the repository root:

```bash
cmake -S plugins/location_triggered_disturbance -B plugins/location_triggered_disturbance/build
cmake --build plugins/location_triggered_disturbance/build -j"$(nproc)"
```

The resulting library is `plugins/location_triggered_disturbance/build/libLocationTriggeredWrench.so`. Follow the [simulation guide](../../simulation/README.md) for plugin/resource paths and startup.

The main aircraft model already includes the plugin block. The alternate `model_with_location_disturbance.sdf` is retained as an existing snapshot; do not overwrite the main model merely to enable the already-present plugin.

## Evaluation

The [existing mission](Flight_Test.plan) is a short example associated with this trigger region. Use identical mission, parameters, trigger bounds, and torque for a paired comparison, restart Gazebo between runs, and report actual exposure durations. Use `RLL_INDI_EN=0` for PID, `1` for PID with INDI shadow logging, or `2` for active roll INDI.
