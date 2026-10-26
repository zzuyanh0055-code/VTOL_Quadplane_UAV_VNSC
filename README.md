# Autonomous VTOL QuadPlane UAV
### Airframe Design · Aerodynamic Analysis · Flight Performance Simulation

A collaborative UAV project covering mechanical design, fabrication, aerodynamic modeling, and autonomous flight simulation of a separate-lift **4+1 QuadPlane**.

The aircraft combines four vertical-lift motors with a rear pusher, a fixed wing, ailerons, an elevator, and twin rudders. The research workflow connects SolidWorks design and mass properties, OpenVSP/VSPAERO analysis, and Gazebo–ArduPilot SITL simulation. The control study compares the baseline PID controller with an experimental fixed-wing roll-axis INDI controller.

**Thesis:** *Aerodynamic Analysis and Flight Performance Simulation of an Autonomous VTOL QuadPlane UAV* — University of Science and Technology of Hanoi (USTH).

> **Repository status:** Simulation assets, controller modifications, and the disturbance plugin are available. The CAD, aerodynamic, hardware, media, and results sections currently contain upload guides; their project files have not yet been added. Simulation results and physical flight-test evidence will be identified separately.

## Explore the Project

| Area | Contents | Availability |
|---|---|---|
| [Mechanical design](cad/README.md) | SolidWorks assemblies and parts, STEP exports, drawings | Awaiting project files |
| [Aerodynamics](aerodynamics/README.md) | OpenVSP geometry, VSPAERO cases, coefficient data | Awaiting project files |
| [Hardware and fabrication](hardware/README.md) | Airframe construction, avionics, wiring, component list | Awaiting project files |
| [Flight simulation](simulation/README.md) | Gazebo models, worlds, ArduPlane parameters, setup | Available; fresh-environment reproduction pending |
| [Roll controller](controller/ardupilot/README.md) | INDI patch, base commit, modified source | Available |
| [Disturbance plugin](plugins/location_triggered_disturbance/README.md) | Position-triggered body-roll torque | Available |
| [Results](results/README.md) | PID–INDI comparisons, aerodynamic plots, physical test records | Awaiting datasets and figures |
| [Photos and renders](media/README.md) | Physical prototype, assembly photos, CAD and simulation views | Awaiting project images |
| [Research documentation](docs/README.md) | Modeling notes, manuscript archive, attribution, upload checklist | Documentation scaffold available |

## Engineering Workflow

1. **Design and fabrication:** develop the airframe, check packaging and mass distribution, and manufacture and assemble the prototype.
2. **Aerodynamic analysis:** prepare the OpenVSP geometry and VSPAERO cases, then export coefficients with the associated reference quantities and operating conditions.
3. **Dynamic modeling:** document the six-degree-of-freedom formulation and map mass, inertia, aerodynamic, and propulsion parameters into the simulation.
4. **Flight simulation:** connect the Gazebo aircraft to ArduPlane SITL and execute QGroundControl missions.
5. **Controller evaluation:** compare PID and INDI under matched missions and disturbance conditions.
6. **Experimental documentation:** record the physical configuration and test outcomes separately from simulation evidence.

See the [modeling and traceability guide](docs/modeling.md) for the information needed to connect these stages reproducibly.

## Simulation Entry Points

The project environment uses Ubuntu 24.04, Gazebo Sim 8, ArduPlane SITL, and QGroundControl. The custom disturbance plugin uses CMake and C++17.

| Item | File |
|---|---|
| Project aircraft | [model.sdf](simulation/models/VTOL_Quadplane/model.sdf) |
| Runway world | [alti_transition_runway.sdf](simulation/ardupilot_gazebo/worlds/alti_transition_runway.sdf) |
| Parameter set used by the setup guide | [alti_transition_quad.param](simulation/ardupilot_gazebo/config/alti_transition_quad.param) |
| Existing example mission | [Flight_Test.plan](plugins/location_triggered_disturbance/Flight_Test.plan) |
| INDI integration | [Controller instructions](controller/ardupilot/README.md) |
| Disturbance implementation | [LocationTriggeredWrench.cc](plugins/location_triggered_disturbance/src/LocationTriggeredWrench.cc) |

Start with the **[simulation setup guide](simulation/README.md)**. The existing mission is a short disturbance example, not the complete archive of research missions.

## Research Scope and Evidence

The repository supports inspection of the simulation implementation. Quantitative performance comparisons require the corresponding mission, parameter snapshot, disturbance configuration, logs, and analysis method; these have not yet been assembled into a results dataset here.

The INDI implementation modifies the **fixed-wing roll axis only**. It is a research modification, not an official ArduPilot controller. Availability of simulation code does not establish physical flight validation of INDI.

## Completing the Archive

Follow the **[upload checklist / Hướng dẫn bổ sung dữ liệu](docs/upload-checklist.md)**. Prioritize a clear photograph of the completed aircraft, the final SolidWorks assembly and STEP export, the OpenVSP model, and a small set of traceable result figures.

This is a team project. A contributor and role summary, manuscript metadata, and project-original licensing remain to be completed. Existing third-party notices are retained; see [attribution and licensing](docs/attribution.md).
