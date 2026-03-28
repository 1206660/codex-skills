---
name: godot-ml-autobot
description: Use when building or migrating a machine-learning autotest bot in Godot 4.6 .NET projects. Triggers on requests about automatic bot playtesting, training a tactical policy, recording trajectories, running headless workers, ONNX policy inference, or reusing an ML autobot framework across similar Godot C# stealth or tactics projects.
---

# Godot ML Autobot

Use this skill when the task is not "add a simple scripted bot" but "build a reusable machine-learning autoplay/testing bot pipeline" for a `Godot 4.6 + C#/.NET` project.

Typical requests:
- "Make a machine-learning bot that can auto-run levels."
- "Port this ML autoplay framework to another Godot project."
- "Set up headless workers and trajectory recording."
- "Train a tactical policy and compare heuristic vs learned behavior."
- "Make this bot framework reusable across projects."

## Outcome

Build a practical stack:

`strategic planner -> risk-aware route layer -> ML tactical policy -> trajectory logging -> headless evaluation`

Do not default to end-to-end RL for the whole game unless the project explicitly requires it and already has the infrastructure to support it.

## Default Recommendation

Prefer a hybrid architecture:

1. Keep strategic sequencing in rules/GOAP/task logic.
2. Keep navigation in the existing path/risk system.
3. Use ML only for short-horizon tactical choices such as:
   - hold
   - retreat
   - re-center to cover
   - distraction tool use
   - smoke/tool timing
   - commit vs abort at local choke points

This is the highest-leverage setup for stealth and tactics projects because it:

- preserves debuggability
- improves sample efficiency
- makes failures easier to localize
- ports better between projects

## Required Project Seams

Before implementation, confirm the target project has or can support:

1. One central automation controller
   - owns current goal, path, tactical window, episode lifecycle

2. Structured runtime state
   - detection / alert
   - goal id / kind
   - path length
   - local risk
   - resource counts and cooldowns
   - nearby enemy summaries

3. Discrete tactical actions
   - do not train raw movement-by-frame first

4. Action masking
   - invalid actions must be blocked

5. A pluggable tactical policy interface
   - heuristic fallback
   - learned policy implementation

6. A stable trajectory record format
   - `obs, action, reward, done, next_obs` plus metadata

If any of these are missing, build them first before discussing training.

## Recommended File Shape

In the target project, prefer a structure like:

```text
Scripts/
  Automation/
    AutoBotController.cs
    AutoBotController.ML.cs
    MissionGoapPlanner.cs
  ML/
    ITacticalPolicy.cs
    TacticalObservation.cs
    TacticalAction.cs
    TacticalActionMask.cs
    TacticalDecision.cs
    TacticalFeatureExtractor.cs
    HeuristicTacticalPolicy.cs
    OnnxTacticalPolicy.cs
    TacticalTransitionRecord.cs
tools/
  RLTrainer/
    Program.cs
    TrajectoryDatasetReader.cs
    TrajectoryDatasetWriter.cs
    EvaluationRunner.cs
    PpoTrainer.cs
  windows/
    run_ml_workers.ps1
    run_ml_iterations.ps1
    evaluate_tactical_policy.ps1
```

Adapt naming to the project, but keep the boundaries.

## Workflow

### 1. Build the heuristic bot first

Before any model training:

- make the automation controller able to finish at least the easy half of the level
- keep retries automatic
- make runtime logs readable enough to cluster failures

If the heuristic bot cannot reach meaningful mid-level states, do not start PPO.

### 2. Define structured observations

Prefer structured state over pixels.

Minimum features:
- level identifier
- goal identifier and type
- alert / detection
- current cell risk
- next-step risk
- path length
- concealment / harsh light
- tool charges and cooldowns
- top enemy summaries
- small local patch around the player

### 3. Use symbolic tactical actions

Good first actions:
- `FollowPlannedPath`
- `HoldPosition`
- `AdvanceToBestNeighbor`
- `RetreatToSafestNeighbor`
- `RecenterToCoverNeighbor`
- `UseStoneDistraction`
- `UseSmokeAtSelf`
- `UseSmokeAtForwardAnchor`
- `ContextAction`
- `AbortAndReplan`

### 4. Add action masks before training

Mask out actions when:
- resources are missing
- no legal target exists
- no safe neighbor exists
- context action is not available

This is mandatory for stable behavior.

### 5. Record trajectories

Start with heuristic-generated demonstrations.

Use them to:
- validate feature quality
- validate action masks
- build a first imitation or linear baseline
- expose failure clusters

### 6. Run headless workers

Prefer multi-process headless workers over single-instance visual runs.

For each worker, isolate:
- datasets
- logs
- state files

Use visual runs only for debugging and replay inspection.

### 7. Train and compare online

Do not trust offline accuracy alone.

Always compare with the same evaluation setup:
- same level
- same worker count
- same timeout
- heuristic vs learned policy

Retain a change only if online results show a reproducible gain.

## Validation Rules

- Prefer repeated 6-worker or similar batch runs over one-off wins.
- Treat deeper-but-less-stable behavior as provisional.
- Keep heuristic fallback available at all times.
- Publish or retain only verified positive signal.
- Revert noisy or negative experiments immediately.

## Common Failure Modes

- Model owns too much of the game loop.
- No action mask.
- Training starts before heuristic bot reaches useful states.
- Sampling is done visually instead of headless.
- Transition format keeps changing across experiments.
- Decisions are evaluated only offline, never in live batch runs.

## What To Reuse vs What To Rebuild

Reuse:
- architecture boundaries
- policy interface
- observation/action/mask pattern
- worker scripts
- evaluation flow

Rebuild per project:
- concrete goal ids
- level-specific heuristics
- tool thresholds
- mission-specific reward terms
- objective ordering

## References

Read these as needed:

- For the target architecture and migration checklist: `references/playbook.md`
- For a concrete rollout order in a new project: `references/migration-checklist.md`

