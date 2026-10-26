# Results and Validation

**Status:** Comparative datasets, analysis scripts, and research figures have not yet been uploaded. No performance improvement is claimed by this placeholder.

Organize future uploads by evidence type: `aerodynamics/`, `simulation/`, and `flight-tests/`. Create those subfolders when actual data are added.

## Simulation Comparison Package

For every PID–INDI comparison, include:

1. A run ID, repository commit, ArduPilot base commit, and applied patch.
2. The exact mission, complete parameter snapshot, model/world revision, initial conditions, and controller mode.
3. Disturbance torque, trigger region, actual exposure time, and wind configuration.
4. Source logs or a stable download link, plus extraction/plotting scripts and dependencies.
5. CSV data, metric definitions, analysis interval, units, and comparison figures.

Useful outputs include trajectory versus planned path, cross-track error, roll angle/rate tracking, altitude, control output, and saturation. Compare like-for-like runs and state whether figures describe one run or repeated trials.

The location-triggered plugin applies torque until zone exit, so PID and INDI can have different exposure durations even with identical zone bounds. Report those durations.

## Physical Tests

Keep physical test records separate. Identify the aircraft revision, measured configuration, test objective, procedure, observed outcome, and limitations. Simulation success must not be presented as a completed real-world mission.

## File Handling

Runtime logs are excluded by the root `.gitignore`. Keep large raw logs outside the normal source checkout and document their download location and checksum. Commit compact derived CSV data and figures alongside the analysis method.
