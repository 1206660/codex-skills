# UI Shell Checklist

When building the first UI shell for a Godot project, keep it small and reuse what already exists.

## Scan first

Look for:

- `scenes/ui/` or equivalent
- `Scripts/UI/` or equivalent
- `Theme` resources
- `StyleBox` resources
- `LabelSettings`
- icon folders
- reusable panel or dialog scenes

## Minimum shell

Create only:

- one startup or entry scene
- one main menu scene
- one HUD root scene
- one generic dialog or panel stub

## Reuse hierarchy

Prefer, in this order:

1. Existing project `Theme` resources
2. Existing style resources such as buttons and panels
3. Existing icons, label settings, and fonts
4. Existing layout patterns from menu or HUD scenes
5. Only then create lightweight placeholders

## Guardrails

- Do not invent a second theme stack if one already exists
- Do not duplicate an existing `MainMenu`, `HUDRoot`, or dialog root under a new name
- Do not add a large art pass when existing assets are sufficient
- Do not hardcode paths that should stay reusable across scenes

## Validation

Before stopping, verify:

- every scene opens
- every script attaches
- every resource path resolves
- the startup flow can reach the main menu
- the HUD root can be instantiated without missing resource warnings
