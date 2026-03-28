---
name: init-godot-game
description: Use when initializing or restructuring a Godot 4.x game project, especially with .NET/C#. Triggers on requests to scaffold a reusable project architecture, create standard directories, add a sane Godot plus .NET .gitignore, or generate a starter UI shell by reusing the project's existing themes, icons, label settings, and scene assets instead of inventing a brand-new visual stack.
---

# Init Godot Game

Use this skill when the task is to set up a Godot project foundation, not to implement a single gameplay mechanic.

Default outcome:

`project scaffold -> ignore rules -> UI shell -> validation`

Prefer extending the structure and visual language already present in the project. Do not create a parallel UI system unless the project has no usable one.

## Workflow

### 1. Scan before scaffolding

Inspect the project first:

- existing scene roots such as `scenes/`, `levels/`, `ui/`
- existing script roots such as `Scripts/`, `Scripts/UI/`
- existing themes, icons, label settings, fonts, shaders, and reusable panels
- the current `.gitignore`

If the project already has a stable UI stack, reuse it. Do not introduce a second naming convention.

### 2. Create the minimum durable structure

Prefer a layout like:

```text
assets/
  themes/
  ui/
levels/
resources/
scenes/
  ui/
Scripts/
  Core/
  Game/
  UI/
tools/
docs/
```

Adapt names to the repository, but keep scene, script, asset, and tooling boundaries clear.

Read `references/scaffold-layout.md` when you need the default purpose of each directory.

### 3. Add a sane Godot + .NET .gitignore

Always cover:

- `.godot/`
- `bin/`
- `obj/`
- IDE state such as `.vs/`, `.idea/`, `.vscode/`
- local export binaries and local build logs

Do not blindly replace an existing `.gitignore`. Merge with it and preserve repository-specific entries.

### 4. Build the base UI shell from existing assets

Create only the minimum shell needed to make the project coherent:

- startup or entry scene
- main menu shell
- HUD root
- one reusable dialog or panel stub

Default rule: reuse current project assets first.

Prefer:

- existing `Theme` resources
- existing `StyleBox` resources
- existing icons and label settings
- existing panel layouts or menu scenes

Do not create a new art direction if the project already has one.

Read `references/ui-shell-checklist.md` before creating or reshaping the UI shell.

### 5. Wire scripts and scene ownership cleanly

Keep scene logic and script locations aligned:

- `scenes/ui/*.tscn` pairs with `Scripts/UI/*.cs`
- entry scenes own only entry flow
- HUD roots own runtime UI composition

Avoid scattering UI controller scripts across gameplay folders.

### 6. Validate before stopping

Verify:

- scenes open in Godot
- script paths resolve
- theme and icon resources resolve
- the project does not contain duplicate UI roots for the same responsibility
- the scaffold does not overwrite repository-specific ignore rules

## Non-Goals

This skill does not imply:

- building full gameplay systems
- generating bespoke art packs
- redesigning the whole UI from scratch
- replacing existing mature project structure without cause

## References

- For the default project layout and why each directory exists: `references/scaffold-layout.md`
- For the minimum UI shell and asset reuse checklist: `references/ui-shell-checklist.md`
