# Migration Checklist

Use this order when porting the framework to another Godot 4.6 .NET project.

1. Create a central automation controller.
2. Expose structured runtime state for observation building.
3. Define tactical action enums and masks.
4. Implement heuristic tactical policy first.
5. Add trajectory recording and per-episode summaries.
6. Add headless worker scripts with isolated output directories.
7. Add trainer project that reads all recorded datasets recursively.
8. Add learned policy inference behind the same tactical policy interface.
9. Add online evaluation scripts for heuristic vs learned comparison.
10. Only then start long-run training or PPO refinement.

## Porting Warning

Do not copy over:

- level ids
- goal ids
- hand-tuned thresholds
- project-specific smoke/stone logic

Copy the structure, not the concrete numbers.
