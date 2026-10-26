# Flight Simulation

[Back to project overview](../README.md)

This guide is the simulation component of the full UAV project. All shell commands below run from the repository root unless stated otherwise. The documented environment comes from the project; these instructions have not been re-run in a fresh Gazebo environment during this documentation update.

Gazebo Sim and ArduPilot SITL simulation package for a separate-lift Hybrid VTOL fixed-wing UAV with a 4+1 configuration:

- Four vertical-lift motors
- One rear pusher motor
- Ailerons, elevator, and twin rudders
- PID baseline controller
- Experimental roll-axis INDI controller
- Position-triggered roll disturbance plugin

## Simulation Environment

The project was developed and tested with:

- Ubuntu 24.04
- Gazebo Sim 8
- ArduPilot SITL / ArduPlane
- QGroundControl
- CMake and C++17

## Simulation Layout

| Location (relative to repository root) | Purpose |
|---|---|
| `simulation/models/VTOL_Quadplane/` | Project aircraft and visualization meshes |
| `simulation/ardupilot_gazebo/` | Bundled integration source, parameter sets, models, and worlds |
| `simulation/models/alti_transition_quad/` | Reference model and assets used by the project model |
| `simulation/models/runway/` | Runway environment |
| `simulation/missions/` | Mission index and destination for research missions |
| `simulation/terrain/` | Terrain data |
| `controller/ardupilot/` | Roll-axis INDI patch and pinned ArduPilot base |
| `plugins/location_triggered_disturbance/` | Custom disturbance plugin |

Alternative SDF and parameter snapshots are retained. The entry points below are those used by this guide; other filenames do not imply a validated configuration.

## Main Files

- Aircraft model:

  `simulation/models/VTOL_Quadplane/model.sdf`

- Gazebo world:

  `simulation/ardupilot_gazebo/worlds/alti_transition_runway.sdf`

- ArduPlane parameter file:

  `simulation/ardupilot_gazebo/config/alti_transition_quad.param`

- Disturbance plugin source:

  `plugins/location_triggered_disturbance/src/LocationTriggeredWrench.cc`

## Prerequisites and ArduPilot Integration

Install Gazebo Harmonic and the development dependencies described in the [bundled integration guide](ardupilot_gazebo/README.md). Install ArduPilot separately; for the experimental controller, follow the [pinned-base patch instructions](../controller/ardupilot/README.md) before launching SITL.

Clone this project if needed:

```bash
git clone https://github.com/zzuyanh0055-code/VTOL_Quadplane_UAV_VNSC.git
cd VTOL_Quadplane_UAV_VNSC
```

Build the bundled Gazebo–ArduPilot integration from the repository root:

```bash
export GZ_VERSION=harmonic
cmake -S simulation/ardupilot_gazebo \
  -B simulation/ardupilot_gazebo/build \
  -DCMAKE_BUILD_TYPE=RelWithDebInfo
cmake --build simulation/ardupilot_gazebo/build -j"$(nproc)"
```

## Build the Location-Triggered Disturbance Plugin

From the repository root:

```bash
cmake \
  -S plugins/location_triggered_disturbance \
  -B plugins/location_triggered_disturbance/build

cmake --build \
  plugins/location_triggered_disturbance/build \
  -j"$(nproc)"
```

The generated library should be:

```text
plugins/location_triggered_disturbance/build/libLocationTriggeredWrench.so
```

## Configure Gazebo Paths

Run from the repository root:

```bash
export GZ_SIM_SYSTEM_PLUGIN_PATH="$PWD/plugins/location_triggered_disturbance/build:$PWD/simulation/ardupilot_gazebo/build${GZ_SIM_SYSTEM_PLUGIN_PATH:+:$GZ_SIM_SYSTEM_PLUGIN_PATH}"

export GZ_SIM_RESOURCE_PATH="$PWD/simulation/models:$PWD/simulation/ardupilot_gazebo/models:$PWD/simulation/ardupilot_gazebo/worlds${GZ_SIM_RESOURCE_PATH:+:$GZ_SIM_RESOURCE_PATH}"
```

Set these variables in the terminal that starts Gazebo. Keep the project model directory first: duplicate model names exist in the bundled examples.

## Start Gazebo

```bash
gz sim -v4 -r \
simulation/ardupilot_gazebo/worlds/alti_transition_runway.sdf
```

A successful disturbance-plugin load should include a message similar to:

```text
[LocationTriggeredWrench] Armed for link [base_link]
```

## Start ArduPlane SITL

ArduPilot must be installed separately.

Example command:

```bash
cd /path/to/ardupilot

sim_vehicle.py \
  -v ArduPlane \
  -f quadplane \
  --model JSON \
  --add-param-file=/path/to/VTOL_Quadplane_UAV_VNSC/simulation/ardupilot_gazebo/config/alti_transition_quad.param \
  --console
```

Replace `/path/to/...` with the actual paths on the local computer.

## Roll Controller Selection

The experimental parameter `RLL_INDI_EN` selects the roll controller:

| Value | Operating mode |
|---:|---|
| `0` | Conventional PID |
| `1` | PID active with INDI shadow logging |
| `2` | INDI active on the roll axis |

The [custom ArduPilot patch](../controller/ardupilot/README.md) must be applied and built before this parameter becomes available. Mode 1 still uses PID for control; it adds INDI shadow computation and logging.

## Position-Triggered Disturbance

The custom Gazebo system plugin applies a constant body-roll moment while the aircraft remains inside a defined three-dimensional zone.

The current test configuration is stored in:

```text
simulation/models/VTOL_Quadplane/model.sdf
```

The disturbance is activated on zone entry and deactivated when the aircraft exits the zone. The trigger remains locked until the simulation is restarted.

Example startup message:

```text
[LocationTriggeredWrench] Armed for link [base_link];
body-roll torque=1 N.m while the aircraft remains inside the zone.
```

## Missions

See the [mission archive](missions/README.md) and load the existing example into QGroundControl only as the corresponding disturbance example. The full research missions and per-run parameter snapshots are still to be added.

## Test Method

The primary comparison cases are:

1. PID trajectory tracking without disturbance.
2. INDI trajectory tracking without disturbance.
3. PID flight through the fixed disturbance zone.
4. INDI flight through the same disturbance zone.

The same mission, aircraft model, parameter set, waypoint radius, airspeed command, and disturbance configuration should be used for each PID–INDI comparison. Record actual disturbance exposure time as well, because exit time can differ between controllers.

The checked-in aircraft has the disturbance plugin enabled with a 1 N·m moment. For a no-disturbance run, set `roll_torque_nm` to `0` in a recorded local model copy and restart Gazebo. Restore the intended torque before disturbed comparisons. See the [plugin configuration](../plugins/location_triggered_disturbance/README.md) and [results requirements](../results/README.md).

## Runtime Files

Flight logs, Gazebo runtime data, compiled libraries, and build directories are intentionally excluded from Git. These files should be generated locally.

## Validation Status

The repository paths and documentation have been checked during reorganization. Gazebo plugin builds, ArduPlane builds, mission execution, and performance metrics have not been re-tested in this editing environment. Confirm those on the project machine before calling the setup independently reproduced.
