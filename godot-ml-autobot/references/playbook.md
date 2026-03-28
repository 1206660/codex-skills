# Playbook

## Recommended Architecture

Use:

`planner -> route -> ML tactical policy`

Where:

- planner decides the next mission-relevant goal
- route layer decides how to reach it safely
- ML decides local tactical behavior under pressure

This is the default recommendation for stealth and tactics games.

## Minimal Runtime Surface

The automation controller should be able to answer:

- what is the active goal
- what is the current path
- what is the current path length
- what is the current and next-cell risk
- what is the alert/detection state
- what tools are available
- whether a context action is valid now

## Observation Design

Start with structured features:

- mission progress
- goal id and goal kind
- alert / detection
- local risk
- concealment / harsh light
- resource counts and cooldowns
- nearby enemy summaries
- a small local grid patch

Avoid pixel observations unless the project already has a mature vision pipeline.

## Action Design

Prefer symbolic tactical actions rather than raw movement:

- follow path
- hold
- retreat
- re-center to cover
- use distraction
- use smoke
- context action
- abort and replan

## Training Order

1. Make heuristic autoplay work.
2. Record trajectories.
3. Build imitation or linear baseline.
4. Add ONNX inference path.
5. Run online heuristic vs learned comparisons.
6. Add PPO fine-tuning only after the pipeline is stable.

## Evaluation Standard

Use headless batch runs with:

- fixed worker count
- fixed timeout
- stable logging
- reproducible run directories

Always compare against the best retained heuristic or learned baseline.

