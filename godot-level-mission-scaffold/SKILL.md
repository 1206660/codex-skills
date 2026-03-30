---
name: godot-level-mission-scaffold
description: Use when scaffolding or restructuring level and mission content in a Godot 4.x project. Triggers on requests to add a clean level content layout, create mission data and scene ownership boundaries, scaffold encounter or objective folders, or standardize how level scenes, resources, and mission scripts map together in a Godot game.
---

# Godot Level Mission Scaffold

Use this skill when the task is to establish or clean up a durable content structure for levels and missions, not to author a full mission by hand.

## Outcome

Build:

`content audit -> level/mission scaffold -> ownership wiring -> validation`

Prefer extending the project's current content model instead of forcing a second system beside it.

## Recommended Content Shape

Prefer a shape like:

```text
levels/
missions/
encounters/
resources/
  missions/
scenes/
  levels/
  missions/
Scripts/
  Missions/
  Game/
```

Adapt names to the repository, but keep data, scenes, and scripts clearly separated.

Read `references/content-layout.md` when choosing the exact layout.

## Workflow

### 1. Audit the current content roots

Inspect:

- level data files
- mission or objective data
- level scenes and mission scenes
- runtime controllers and mission scripts
- any existing encounter, event, or cutscene folders

If the project already has a stable content stack, align to it rather than renaming everything.

### 2. Scaffold the minimum durable tree

Create only the roots needed to separate:

- authored level data
- mission or objective definitions
- reusable mission resources
- mission runtime scripts
- level scenes and mission scenes

Do not introduce more folders than the project can actually keep coherent.

### 3. Wire ownership boundaries

Keep responsibilities obvious:

- level data owns map or spatial content
- mission data owns objectives, triggers, and sequencing
- scenes own composition
- scripts own runtime behavior

Read `references/mission-scaffold-checklist.md` before wiring new mission seams.

### 4. Preserve naming consistency

Use stable naming across:

- level ids
- mission ids
- scene file names
- resource file names
- controller class names

Avoid making the scene naming scheme diverge from the data naming scheme.

### 5. Validate before stopping

Verify:

- new folders match the repository's naming conventions
- scene, resource, and script paths resolve
- a level can find its mission data
- runtime controllers do not depend on ad hoc hardcoded file paths

## Non-Goals

This skill does not imply:

- authoring full mission gameplay
- generating all encounter content automatically
- replacing a mature campaign pipeline without cause
- merging unrelated gameplay systems into mission scaffolding

## References

- For recommended content boundaries and folder purposes: `references/content-layout.md`
- For the minimum checklist when adding a new level or mission seam: `references/mission-scaffold-checklist.md`
