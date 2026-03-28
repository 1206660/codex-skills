---
name: godot-ui-reskin-from-assets
description: Use when restyling or refreshing a Godot 4.x UI by reusing assets already present in the project. Triggers on requests to reskin a main menu, HUD, dialog stack, or other UI scenes using the current themes, icons, fonts, label settings, shaders, panels, and textures instead of creating a brand-new art set.
---

# Godot UI Reskin From Assets

Use this skill when the task is to improve a Godot UI's look and coherence by recomposing what the repository already has.

Default outcome:

`asset audit -> visual direction -> focused scene reskin -> validation`

Do not default to generating new art. Start by mining the existing project for reusable UI material.

## Workflow

### 1. Audit the current UI stack

Inspect:

- menu, HUD, dialog, and overlay scenes
- UI controller scripts
- `Theme`, `StyleBox`, `LabelSettings`, fonts, icons, and shader-backed materials
- reusable panels, buttons, badges, and decorative textures

Group findings into:

- typography
- palette
- button and panel treatments
- iconography
- reusable layout patterns

Read `references/asset-audit.md` before making layout or styling changes.

### 2. Pick one visual direction

Choose a single direction that fits the current assets, such as:

- clean sci-fi console
- fantasy parchment
- modern flat control panel
- retro arcade overlay

Use what the project already supports. Do not mix multiple incompatible looks in one pass.

### 3. Reskin the minimum high-impact surfaces

Prioritize:

- main menu
- HUD root
- one reusable dialog or popup

When time is limited, change the surfaces the player sees first instead of touching every UI scene.

### 4. Recompose instead of repainting

Prefer:

- swapping to existing style resources
- reusing label settings and fonts
- reusing icons and decorative textures
- adjusting spacing, hierarchy, and alignment
- adding light background shapes, glows, or panels from current assets

Avoid a large art pass unless the user explicitly asks for one.

Read `references/reskin-patterns.md` when you need concrete transformation patterns.

### 5. Preserve scene ownership

Keep responsibilities clear:

- scene files own layout
- scripts own interaction and data binding
- theme resources own appearance when possible

Do not move presentation logic into gameplay systems just because the UI changed.

### 6. Validate before stopping

Verify:

- modified scenes open and instantiate
- resource paths resolve
- scripts still attach
- the new look is consistent across menu, HUD, and dialog surfaces
- no duplicate parallel theme stack was created unnecessarily

## Non-Goals

This skill does not imply:

- generating new character art
- redesigning gameplay UX from first principles
- replacing a mature design system without cause
- building a whole UI framework when only a reskin is needed

## References

- For how to audit existing UI assets and sort them into reusable buckets: `references/asset-audit.md`
- For common reskin patterns that reuse existing Godot resources: `references/reskin-patterns.md`
