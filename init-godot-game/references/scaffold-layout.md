# Scaffold Layout

Use this as the default layout when the project does not already have a strong alternative:

```text
assets/
  themes/
  ui/
docs/
levels/
resources/
scenes/
  ui/
Scripts/
  Core/
  Game/
  UI/
tools/
```

## Directory intent

- `assets/`: imported art, icons, fonts, shaders, and reusable theme resources
- `assets/themes/`: shared `Theme`, `StyleBox`, `LabelSettings`, palette resources
- `assets/ui/`: UI-specific icons, decorative textures, cursor art, and related assets
- `levels/`: authored level data and mission content
- `resources/`: shared `.tres` and runtime data assets
- `scenes/`: playable and reusable scenes
- `scenes/ui/`: menu scenes, HUD roots, dialogs, toasts, overlays
- `Scripts/Core/`: bootstrapping, service location, save/config glue
- `Scripts/Game/`: runtime gameplay systems
- `Scripts/UI/`: controllers for scenes under `scenes/ui/`
- `tools/`: automation scripts, exporters, validators
- `docs/`: plans and technical notes when the repository already keeps them in source control

## Mapping rule

Keep scene ownership obvious:

- UI scenes map to `Scripts/UI/`
- gameplay scenes map to `Scripts/Game/`
- startup and bootstrap logic map to `Scripts/Core/`

If the repository already uses another stable structure, align to that instead of forcing this exact tree.
