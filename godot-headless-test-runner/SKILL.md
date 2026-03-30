---
name: godot-headless-test-runner
description: Use when setting up or standardizing headless automated runs for a Godot 4.x project, especially with .NET/C#. Triggers on requests to create smoke or batch test commands, run multiple Godot workers, isolate artifacts and logs, normalize exit-code behavior, or build a repeatable headless evaluation pipeline for local machines or servers.
---

# Godot Headless Test Runner

Use this skill when the task is to make Godot test or simulation runs reproducible in headless mode, not to debug one single scene by hand.

## Outcome

Build:

`surface audit -> canonical run modes -> isolated artifacts -> verification`

Prefer multi-process batch workers over ad hoc repeated manual launches.

## Required Inputs

Confirm or derive:

- Godot executable path
- project root
- build command if the project uses .NET/C#
- headless entrypoint or test flag surface
- timeout rules
- worker count
- artifact root for logs, datasets, and summaries

If the project does not expose a stable test entrypoint yet, create that seam first.

## Canonical Run Modes

Support at least these modes:

- smoke: one short headless run for quick validation
- replay or single-run: one full deterministic run
- suite: one command covering a defined level or test range
- batch workers: N isolated headless processes writing to separate artifact directories

Read `references/command-matrix.md` when deciding how to normalize these modes.

## Workflow

### 1. Audit the current run surface

Inspect:

- existing `.bat`, `.ps1`, `.sh`, or CI commands
- Godot flags and custom `--` arguments
- build steps
- current artifact and log output locations

Do not create a second command vocabulary if the repo already has a good one.

### 2. Normalize headless invocations

Make the commands consistent about:

- Godot executable resolution
- working directory
- headless flag usage
- custom test arguments
- exit behavior

### 3. Isolate artifacts per run

Each run or worker should have its own:

- logs
- stdout and stderr captures
- run summaries
- datasets or result payloads
- worker-local state files when needed

Read `references/artifacts-and-exit-codes.md` before finalizing artifact layout.

### 4. Keep exit-code semantics honest

A run should fail clearly when:

- the process crashes
- required summaries are missing
- artifacts are incomplete
- pass/fail thresholds are not met

Do not swallow failures just because a log file exists.

### 5. Validate with repeatable batches

Verify:

- smoke mode works
- batch mode works
- logs and artifacts are isolated per run
- reruns do not overwrite unrelated data
- the same command works on a clean machine once prerequisites are present

## Common Failure Modes

- editor-only assumptions leaking into headless mode
- shared temp or state files across workers
- mixed build and run steps with unclear failure boundaries
- relative paths breaking under different working directories
- process exit code reporting success while test artifacts indicate failure

## References

- For standard run mode definitions and parameter surfaces: `references/command-matrix.md`
- For artifact layout and exit-code rules: `references/artifacts-and-exit-codes.md`
