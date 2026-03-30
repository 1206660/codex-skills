# Command Matrix

Use these run modes as the default vocabulary for a Godot headless pipeline:

## Smoke

- one worker
- short timeout
- narrow target surface
- used for quick command validation

## Replay or single-run

- one worker
- full target level, scenario, or simulation
- used for deterministic debugging and deep logs

## Suite

- one command that spans a defined range of levels or cases
- fixed shared parameters
- used for regular regression checks

## Batch workers

- multiple Godot processes
- isolated artifact directories per worker
- used for data generation, stochastic validation, or throughput-heavy checks

## Normalization rule

All modes should agree on:

- executable resolution
- project path
- timeout parameter names
- artifact root behavior
- pass and fail reporting
