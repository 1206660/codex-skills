# Asset Audit

Before reskinning any Godot UI, inventory the existing reusable pieces.

## Check these sources

- `scenes/ui/` and equivalent UI scene roots
- `Scripts/UI/` and equivalent controller scripts
- `assets/themes/` and other shared style folders
- icon folders
- `LabelSettings`, fonts, and shader-backed materials
- reusable panels, badges, cards, and overlays

## Bucket the findings

Sort assets into:

- typography
- palette
- panel treatments
- button treatments
- iconography
- ambient decoration
- layout patterns

## Reuse rules

- Prefer existing `Theme` and `StyleBox` resources over local per-scene overrides
- Prefer existing label settings over ad hoc font size changes
- Prefer existing icons over generic placeholders
- Prefer existing panel scenes over one-off container restyling

## Output of the audit

Before editing scenes, state:

- which resources will anchor the new look
- which 2-3 scenes will change in this pass
- which existing resources will remain the system default
