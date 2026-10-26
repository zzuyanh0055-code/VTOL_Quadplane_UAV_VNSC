# Modeling and Parameter Traceability

**Status:** Documentation outline; the derivation and source datasets still need to be added.

The intended chain is SolidWorks mass properties → OpenVSP/VSPAERO aerodynamic analysis → six-degree-of-freedom aircraft formulation → Gazebo model → ArduPlane SITL evaluation.

## Required Records

| Quantity | Required source and context | Simulation destination |
|---|---|---|
| Geometry and control surfaces | CAD revision, dimensions, units, hinge axes | Meshes, links, joints |
| Mass, CG, inertia | CAD report and/or measurements; coordinate frame and origin | Link inertial blocks |
| Aerodynamic coefficients | VSPAERO case, reference quantities, fit range | LiftDrag parameters |
| Propulsion | Motor/propeller configuration, thrust/torque data or identified model | Propeller and actuator model |
| Control mapping | Channel/function, direction, limits, units | ArduPilotPlugin controls and parameter file |
| Disturbance | Body-axis moment, world-coordinate region, exposure | LocationTriggeredWrench settings |

For the dynamic model, define states, inputs, coordinate frames, rotation conventions, force and moment equations, and assumptions. Document body, NED, and Gazebo-world transformations explicitly.

## Configuration Consistency

- Distinguish the measured prototype from CAD and aerodynamic representations.
- Record reference span separately from physical wingspan.
- Record total model mass across all links; a base-link mass alone is not the aircraft total.
- Explain approximations used when converting coefficient curves into simulator parameters.
- Associate every result with the model and parameter revision that generated it.

The current repository provides the implementation in [model.sdf](../simulation/models/VTOL_Quadplane/model.sdf), but the complete derivation and provenance of its coefficients are not yet archived.
